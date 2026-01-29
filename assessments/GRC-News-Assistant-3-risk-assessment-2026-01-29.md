# GRC News Assistant 3.1 Risk Assessment

**Assessment Date:** 2026-01-29
**Assessor:** AI Risk Assessment Tool
**Tier:** 2 (Standard Assessment)
**Status:** Draft

---

## Executive Summary

The GRC News Assistant 3.1 is an n8n-based workflow automation system that aggregates cybersecurity news via RSS feeds, processes content through Claude AI for analysis and rating, and stores results in a Notion database. The system has **two of three Lethal Trifecta factors present** (untrusted input from RSS feeds, external communication to APIs) but processes only **public data** with no access to sensitive internal systems.

**Overall Risk Rating:** **LOW-MEDIUM**

**Recommendation:** **Approve** - The existing security hardening (non-root containers, network isolation, prompt injection defenses, pre-deployment scanning) adequately mitigates the identified risks for this use case.

---

## Lethal Trifecta Check

| Factor | Present | Details |
|--------|:-------:|---------|
| **Private data access** | **No** | Only accesses public RSS feeds and a Notion database containing non-sensitive processed articles. No PII, PHI, financial data, or internal systems. API keys stored in encrypted credential manager. |
| **Untrusted input** | **Yes** | RSS feeds from external sources (CISA, Simply Cyber, Unsupervised Learning, CISO Series) could contain malicious payloads or prompt injection attempts embedded in article content. |
| **External communication** | **Yes** | Sends data to Anthropic API (content processing), Notion API (storage), and fetches from RSS feed URLs. No email sending or posting to external platforms. |

**Trifecta Status:** ⚠️ **Elevated** (2 of 3 factors present)

The absence of sensitive data access significantly reduces the blast radius of potential attacks. Prompt injection could cause incorrect labeling/rating but cannot exfiltrate sensitive data or take destructive actions.

---

## Business Objective

Automate the collection, analysis, and prioritization of cybersecurity news to help GRC professionals focus on high-value content. The system rates articles on an S-D tier scale based on relevance to GRC professional development themes.

---

## Use Cases

1. **Daily news aggregation** - Automatically fetch articles from 4 curated RSS sources at 5 AM
2. **AI-powered analysis** - Clean HTML, extract insights, apply GRC-relevant labels (30+ categories)
3. **Quality scoring** - Rate content relevance on 1-100 scale with tier assignment
4. **Notion storage** - Store processed articles with metadata for team consumption

---

## Architecture & Deployment

### Components

| Component | Type | Description |
|-----------|------|-------------|
| n8n Server | Workflow Automation | Hardened Docker container (localhost:5678) |
| PostgreSQL | Database | Persistent storage, internal network isolated |
| Redis | Cache | Queue management, internal network isolated |
| Anthropic API | LLM Service | Claude AI for content analysis |
| Notion API | Storage Service | Article database |
| RSS Feeds | Data Sources | 4 external cybersecurity news feeds |
| Kali Linux VM | Host | Isolated VirtualBox environment |

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                     Kali Linux VM (VirtualBox)                   │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │                    Docker (Hardened)                         │ │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │ │
│  │  │   n8n       │  │  PostgreSQL │  │   Redis     │          │ │
│  │  │ (non-root)  │  │ (internal)  │  │ (internal)  │          │ │
│  │  │ read-only   │  │ no internet │  │ no internet │          │ │
│  │  └──────┬──────┘  └─────────────┘  └─────────────┘          │ │
│  └─────────┼────────────────────────────────────────────────────┘ │
└────────────┼────────────────────────────────────────────────────┘
             │
    ┌────────┴────────┐
    │   External APIs  │
    ├──────────────────┤
    │ • RSS Feeds (IN) │
    │ • Anthropic (OUT)│
    │ • Notion (OUT)   │
    └──────────────────┘
```

### Authentication

| System | Method |
|--------|--------|
| n8n Web UI | Basic Auth (username/password) |
| Anthropic API | API Key (stored in n8n credential manager) |
| Notion API | Integration Token (stored in n8n credential manager) |
| PostgreSQL | Password (environment file, internal network only) |
| Redis | Password (environment file, internal network only) |

### Trust Boundaries

| Zone | Components | Trust Level |
|------|------------|-------------|
| **Internal Network** | PostgreSQL, Redis | High - No internet access |
| **Docker Host** | n8n container | Medium - Hardened, non-root |
| **VM Boundary** | All containers | Medium - Isolated from host |
| **External** | RSS Feeds | **Untrusted** |
| **External** | Anthropic/Notion APIs | Semi-trusted (vetted vendors) |

### Assets

| Asset | Classification | Description |
|-------|----------------|-------------|
| RSS Feed Content | Public | Cybersecurity news articles |
| Processed Articles | Internal | Rated/labeled content in Notion |
| API Credentials | Confidential | Anthropic & Notion API keys |
| Docker Environment | Confidential | .env file with passwords |
| AI Prompts | Public | Custom analysis prompts |

---

## Threat Scenarios

| ID | Threat Scenario | Likelihood | Impact | Risk | STRIDE | OWASP |
|----|-----------------|:----------:|:------:|:----:|:------:|:-----:|
| T1 | **Indirect prompt injection via RSS** - Malicious content in RSS articles contains hidden instructions that manipulate AI labeling/rating behavior | High | Low | **Medium** | E | LLM01, ASI01 |
| T2 | **API credential theft** - Attacker gains access to Anthropic/Notion API keys through container escape or VM compromise | Low | High | **Medium** | I | LLM02 |
| T3 | **Malicious content amplification** - Poisoned RSS content gets rated highly and propagated to team | Moderate | Low | **Low** | T | LLM04 |
| T4 | **Container escape to host** - Attacker exploits n8n or dependency vulnerability to break out of container | Very Low | High | **Low** | E | LLM03 |
| T5 | **API cost exhaustion** - Malformed content causes excessive API calls, resulting in unexpected costs | Low | Moderate | **Low** | D | LLM10 |
| T6 | **Output manipulation** - Prompt injection causes misleading summaries or ratings to be stored | Moderate | Low | **Low** | T | LLM05 |
| T7 | **Supply chain compromise** - Malicious update to n8n, dependency, or container image | Low | High | **Medium** | T | LLM03, ASI04 |

**Overall Residual Risk:** **LOW**

### Analysis Notes

**Mitigating Factors:**
- No sensitive data accessible (blast radius limited to incorrect ratings)
- Three-layer prompt injection defense already implemented (Guardrails node + hardened prompts + output validation)
- Container hardening (non-root, read-only, capability dropping) significantly reduces escape risk
- VM isolation provides additional boundary
- Pre-deployment vulnerability scanning with Docker Scout
- Database containers have no internet access

**Residual Concerns:**
- RSS feeds are inherently untrusted and could contain adversarial content
- Prompt injection defenses are not 100% effective against novel attacks
- n8n platform has had critical CVEs (addressed in v2.0.0+ requirement)

---

## Remediation Actions

| ID | Action | Priority | Owner | Due Date | Status |
|----|--------|:--------:|-------|----------|:------:|
| R1 | Subscribe to n8n security advisories for proactive patching | Medium | Steve | 2026-02-15 | Not Started |
| R2 | Implement change control process for workflow modifications | Low | Steve | 2026-03-01 | Not Started |
| R3 | Consider input sanitization layer before AI processing | Low | Steve | 2026-03-15 | Not Started |
| R4 | Document incident response runbook specific to this system | Medium | Steve | 2026-02-28 | Not Started |
| R5 | Set up API usage alerts for cost anomaly detection | Low | Steve | 2026-03-01 | Not Started |

---

## Security Checklist Summary

### Controls Assessment

| Control Area | Status | Notes |
|--------------|:------:|-------|
| User authentication | ✓ Pass | Basic auth on n8n UI |
| API authentication | ✓ Pass | Keys in credential manager |
| Data encryption (transit) | ✓ Pass | TLS to all external APIs |
| Data encryption (rest) | ✓ Pass | Docker volumes encrypted |
| Input validation | ⚠️ Partial | Guardrails node, but RSS inherently untrusted |
| Prompt injection defense | ✓ Pass | 3-layer defense implemented |
| Tool permission scoping | ✓ Pass | Minimal permissions (fetch, analyze, store) |
| Human-in-the-loop | N/A | No high-impact decisions |
| Audit logging | ⚠️ Partial | n8n execution logs, no centralized SIEM |
| Kill switch capability | ✓ Pass | docker compose down |
| Container hardening | ✓ Pass | Non-root, read-only, cap-drop |
| Network isolation | ✓ Pass | Internal network for DB/cache |
| Pre-deployment scanning | ✓ Pass | Docker Scout integrated |
| Resource limits | ✓ Pass | CPU/memory caps configured |

**Controls Assessed:** 14 | **Pass:** 11 | **Partial:** 2 | **N/A:** 1

---

## AI/LLM-Specific Controls (OWASP Mapping)

| OWASP ID | Risk | Control Status | Implementation |
|----------|------|:--------------:|----------------|
| LLM01 | Prompt Injection | ✓ Mitigated | Guardrails node + hardened prompts + output validation |
| LLM02 | Sensitive Info Disclosure | ✓ N/A | No sensitive data processed |
| LLM03 | Supply Chain | ✓ Mitigated | Docker Scout scanning, n8n 2.0.0+ pinned |
| LLM04 | Data Poisoning | ⚠️ Accepted | RSS feeds inherently untrusted; impact limited to ratings |
| LLM05 | Improper Output Handling | ✓ Mitigated | JSON schema validation, default values for invalid output |
| LLM06 | Excessive Agency | ✓ N/A | Read-only workflow (fetch, analyze, store) |
| LLM07 | System Prompt Leakage | ⚠️ Accepted | Prompts are public/non-sensitive |
| LLM08 | Vector/Embedding Weaknesses | ✓ N/A | No RAG/vector DB used |
| LLM09 | Misinformation | ⚠️ Accepted | AI ratings may be manipulated; human review expected |
| LLM10 | Unbounded Consumption | ✓ Mitigated | Resource limits configured, schedule-triggered only |

---

## Approval

| Role | Name | Decision | Date | Signature |
|------|------|----------|------|-----------|
| Assessor | AI Risk Assessment Tool | Assessment Complete | 2026-01-29 | ✓ |
| Security Lead | | Approve / Reject / Risk Accept | | |
| Risk Owner | Steve | Accept Residual Risk | | |

---

## Appendix

### Assumptions Made

1. **RSS sources are legitimate** - Assumed CISA, Simply Cyber, Unsupervised Learning, CISO Series are not compromised. Impact if incorrect: Poisoned content enters system.

2. **Notion database contains no sensitive data** - Assumed only processed article metadata stored. Impact if incorrect: Data classification would need elevation.

3. **VM isolation is effective** - Assumed VirtualBox provides meaningful boundary from host. Impact if incorrect: Container escape could reach host system.

### Open Questions

1. Is there monitoring for failed authentication attempts on n8n UI?
2. Are Docker Scout scans performed on every deployment or just initial setup?
3. What is the retention policy for n8n execution logs?

### Existing Security Documentation

- [SECURITY-ADVISORY.md](https://github.com/CPAtoCybersecurity/GRC_News_Assistant_3/blob/main/SECURITY-ADVISORY.md) - CVE notifications
- [GRC_News_Assistant_Risk_Assessment_Local.md](https://github.com/CPAtoCybersecurity/GRC_News_Assistant_3/blob/main/risk_assessment/GRC_News_Assistant_Risk_Assessment_Local.md) - Detailed NIST CSF mapping
- [n8n-Docker-Image-Security-Assessment.md](https://github.com/CPAtoCybersecurity/GRC_News_Assistant_3/blob/main/risk_assessment/n8n-Docker-Image-Security-Assessment.md) - Container security analysis

### References

- NIST SP 800-30: Risk Assessment Methodology
- OWASP Top 10 for LLM Applications 2025
- OWASP Top 10 for Agentic Applications 2026
- CIS Docker Benchmark
- n8n Security Documentation

### Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-01-29 | AI Risk Assessment Tool | Initial assessment using new methodology |

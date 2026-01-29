# AI Risk Assessment Methodology

## Framework: NIST 800-30 for AI Systems

**Risk = Likelihood × Impact**

## The Lethal Trifecta (MANDATORY CHECK)

All three present = Tier 3 required, elevated risk.

| Factor | Question | Examples |
|--------|----------|----------|
| **Private data access** | Can AI access sensitive data? | DBs, emails, files, credentials, PII |
| **Untrusted input** | Can external parties influence AI? | Webhooks, emails, Slack, public APIs, RAG |
| **External communication** | Can AI send data outside? | HTTP requests, email, posting to platforms |

## Assessment Tiers

| Tier | Effort | Use When |
|------|--------|----------|
| **1** | 15-30 min | Low-criticality, internal, no sensitive data |
| **2** | 1-2 hrs | Business-critical, confidential data |
| **3** | Half-day+ | Trifecta present, strictly confidential, regulated |

## Risk Scales

### Likelihood
| Level | Description |
|-------|-------------|
| Very Low | Strong controls, no threat activity |
| Low | Good controls, limited threats |
| Moderate | Some controls, active threats |
| High | Weak controls, known threats |
| Very High | No controls, active exploitation |

### Impact
| Level | Description |
|-------|-------------|
| Very Low | Minor inconvenience |
| Low | Limited damage, easy recovery |
| Moderate | Significant damage, days to recover |
| High | Major damage, weeks to recover |
| Very High | Catastrophic, regulatory action |

### Risk Matrix
| | Low Impact | Moderate | High |
|---|:---:|:---:|:---:|
| **High Likelihood** | Medium | High | Critical |
| **Moderate** | Low | Medium | High |
| **Low** | Low | Low | Medium |

## Assessment Steps

1. **System Characterization** - Components, data flows, users
2. **Asset Identification** - Data classification, critical functions
3. **Lethal Trifecta Analysis** - Mandatory early gate
4. **Threat Identification** - STRIDE + OWASP mapping
5. **Vulnerability Analysis** - Control gaps
6. **Risk Rating** - Likelihood × Impact
7. **Control Recommendations** - Prioritized remediation
8. **Residual Risk** - Risk acceptance

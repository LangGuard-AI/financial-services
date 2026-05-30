# LangGuard — Financial Services Production Blueprints

**Production deployment guides for Claude for Financial Services agent templates.**

Anthropic's [`financial-services`](https://github.com/anthropics/financial-services) repo ships the reasoning layer — domain knowledge, prompt engineering, and skills for five financial workflows.

This repo covers what comes next.

Every regulated financial institution deploying these templates needs five layers before an agent touches a production system:

| Layer | What it is |
|---|---|
| **Data layer** | Wire real ERP connections to the template's tool contracts |
| **Tool layer** | Classify every tool for risk, compliance exposure, and SoD flags |
| **Authorization layer** | Define who and what is permitted to act — in business terms |
| **Human authority layer** | Identify every action requiring human approval |
| **Deterministic execution layer** | Enforce policy at runtime — independently of the model |

These are the blueprints for building those layers.

---

## Agent blueprints

| Agent template | Blueprint | Status |
|---|---|---|
| GL Reconciler | [gl-reconciler/blueprint.md](gl-reconciler/blueprint.md) | ✅ Available |
| Month-End Closer | month-end-closer/blueprint.md | 🔜 Coming soon |
| Statement Auditor | statement-auditor/blueprint.md | 🔜 Coming soon |
| KYC Screener | kyc-screener/blueprint.md | 🔜 Coming soon |
| Valuation Reviewer | valuation-reviewer/blueprint.md | 🔜 Coming soon |

---

## Tools referenced in these blueprints

**SCOPE** — pre-flight compliance classification. Classifies every MCP tool for risk level, compliance regime exposure (SOX, GDPR, HIPAA, PCI, SOC 2 and 22 more), SoD flags, and deployment recommendation. Deterministic. Not LLM-generated.

→ Install: [scope-mcp.langguard.ai](https://scope-mcp.langguard.ai)  
→ Repo: [LangGuard-AI/scope-mcp](https://github.com/LangGuard-AI/scope-mcp)

**Arbiter** — runtime authorization authority. Evaluates every agent action against policy before execution. Sanctions, holds, or blocks. Produces the session record that serves as your compliance artifact.

→ [langguard.ai](https://langguard.ai)

---

## Who this is for

- **FDEs** building production deployments of Claude for Financial Services agent templates
- **Enterprise AI teams** evaluating these templates for regulated environments
- **Implementation partners** — Accenture, Deloitte, KPMG, EY, PwC — standing up Claude agent workflows for financial institutions

---

## The road ahead

Taking any Claude agent template from starting block to production in a regulated financial institution is not a straight road.

It is I-90 in January.

These blueprints map every mile.

---

## Contributing

Spot something wrong or missing? Open an issue or submit a PR.

If you have deployed one of these templates in a regulated environment and have hard-won implementation knowledge — we want it in here.

---

*Built by [LangGuard](https://langguard.ai) — Agentic Workflow Governance*  
*These blueprints are practitioner guides, not legal or compliance advice. Consult your internal audit and legal teams before deploying AI agents in regulated workflows.*

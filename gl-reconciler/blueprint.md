# GL Reconciler — Production Blueprint

**Source template:** [`anthropics/financial-services`](https://github.com/anthropics/financial-services) → `plugins/agent-plugins/gl-reconciler`  
**LangGuard tools:** [SCOPE](https://scope-mcp.langguard.ai) · [Arbiter](https://langguard.ai)  
**Compliance regimes:** SOX Section 404 · SOC 2 · Internal audit

---

## What the template gives you

The GL Reconciler ships domain reasoning — how to find breaks, trace root causes, and structure variance commentary. It defines two MCP tool contract namespaces:

- `mcp__internal-gl__*` — general ledger operations
- `mcp__subledger__*` — subledger operations

These are contracts, not connections. The template expects real MCP servers to exist at these namespaces. Your job is to fulfill them.

Anthropic's own README is explicit: *"does not post to a ledger... every output is staged for human sign-off."*

Staged by whom. Enforced how. That is what this blueprint covers.

---

## Layer 1 — Data layer

*Wire real data connections to the template's tool contracts.*

1. Identify your GL system — SAP S/4HANA, Oracle NetSuite, or custom — and confirm an MCP server exists for it
2. Map the template's abstract tool contracts (`mcp__internal-gl__*` and `mcp__subledger__*`) to real API endpoints in your GL system and configure `.mcp.json` accordingly
3. Run SCOPE against your data layer tools to classify every data operation — risk level, compliance regime exposure (SOX, SOC 2), reversibility, and read vs write classification
4. Use SCOPE's output to confirm which data operations are safe to proceed and which are write operations requiring elevated treatment in subsequent layers

**SCOPE install:** [scope-mcp.langguard.ai](https://scope-mcp.langguard.ai)

---

## Layer 2 — Tool layer

*Classify every tool the agent will invoke before it ships.*

1. Run SCOPE against the full GL Reconciler tool surface — describe the agent in natural language (auto-trigger) or explicitly via `/scope-mcp:audit internal-gl.* subledger.*`
2. Review SCOPE's deterministic output: risk level per tool, which of 26 compliance regimes each tool touches, SoD flags where the same agent should not perform both sides of an action pair, and SCOPE's recommendation — proceed, proceed with audit trail, require human review, require human approval, or block
3. Do not proceed to deployment until every CRITICAL finding is resolved — drop the tool, swap a write for a read, or document the accepted risk with explicit rationale
4. Save the SCOPE output as your tool classification registry — it is the authoritative input for Layers 3, 4, and 5

---

## Layer 3 — Authorization layer

*Define who and what is permitted to act — in business terms.*

1. Using the SCOPE output as your starting point, define a role taxonomy — GL Analyst, GL Controller, Finance Director — and map each role to the specific GL actions it is permitted to take
2. Write explicit SoD rules for every action pair SCOPE flagged — the agent that identifies breaks and stages correcting entries cannot also approve them; preparer and approver must be different actors in the same session
3. Register the GL Reconciler agent identity in Arbiter with its assigned role — GL Analyst — and its permitted action set drawn from the SCOPE classification
4. Have your compliance or internal audit team review and sign off the authorization policy before deployment — this is a business control, not a technical configuration

---

## Layer 4 — Human authority layer

*Identify every action requiring human approval — GL functions and safe operations alike.*

1. Start with SCOPE's "require human approval" recommendations as your baseline — every CRITICAL write operation, every SoD-eligible action pair, every irreversible GL action that cannot be undone once executed
2. Extend beyond GL functions: identify actions that are dangerous in isolation or in combination regardless of business context — destructive operations, excessive agency (the agent can read, decide, AND execute without a checkpoint), the Rule of 2 (no single session completes both sides of a consequential action pair without a human checkpoint)
3. For every identified action assign a treatment: **route to human authority** (sensitive GL actions, high-risk write operations) or **block** (destructive, irreversible, or dangerous combinations that should never execute autonomously)
4. Configure Arbiter to enforce these gates — held pending explicit human approval, with approver identity, timestamp, and policy basis recorded. Not advisory. Enforced. Document the complete human authority map as a governance artifact signed off by compliance before deployment

---

## Layer 5 — Deterministic execution layer

*The last mile. No probabilistic action passes this gate.*

1. Every action that passed through Layers 1–4 must now execute deterministically — Arbiter enforces the policy map at the moment of execution, independently of the model's reasoning. System prompts are advisory. This layer is not.
2. Arbiter evaluates the cumulative session action graph — not just each action in isolation. Dangerous combinations are caught in sequence even when individual actions appear safe. No single agent session completes a consequential action pair without a documented authorization basis.
3. Every execution decision — sanctioned, held, or blocked — is recorded with the policy basis, the actor, and the timestamp. The session record is the SOX audit artifact, produced automatically as a byproduct of enforcement.
4. Before go-live, walk your internal auditor through a simulated session — demonstrate that destructive actions are blocked, SoD violations are caught in sequence, human approvals are enforced not advisory, and the session record answers every control effectiveness question your auditor will ask

---

## Production readiness checklist

### Data layer
- [ ] GL system MCP server identified and configured
- [ ] Tool contracts mapped to real API endpoints
- [ ] `.mcp.json` pointing at correct MCP servers
- [ ] SCOPE run against all data layer tools
- [ ] All read operations verified in sandbox before production connection

### Tool layer
- [ ] SCOPE installed and configured
- [ ] SCOPE audit run against full GL Reconciler tool surface
- [ ] SCOPE output reviewed by compliance team
- [ ] All CRITICAL findings resolved or explicitly accepted with documented rationale
- [ ] SCOPE output saved as tool classification registry

### Authorization layer
- [ ] Role taxonomy defined — GL Analyst, GL Controller, Finance Director
- [ ] Role-action policy written and reviewed
- [ ] GL Reconciler agent identity registered in Arbiter
- [ ] SoD rules configured
- [ ] Policy signed off by compliance or internal audit

### Human authority layer
- [ ] All SCOPE "require human approval" actions mapped to approvers
- [ ] Dangerous combinations and excessive agency patterns identified and gated
- [ ] Arbiter gates configured — enforced, not advisory
- [ ] Timeout and escalation policies defined
- [ ] Human authority map signed off by compliance
- [ ] Gate tested end-to-end in sandbox

### Deterministic execution layer
- [ ] Arbiter session recording configured
- [ ] Session boundary and actor assignment defined
- [ ] Retention policy set — SOX requires 7 years
- [ ] Immutable session record storage confirmed
- [ ] Integration with compliance tooling confirmed
- [ ] Auditor walkthrough of sample session record completed

---

## Effort estimate

| Layer | Estimated effort |
|---|---|
| Data layer | 2–4 weeks |
| Tool layer | 3–5 days |
| Authorization layer | 1–2 weeks |
| Human authority layer | 1–2 weeks |
| Deterministic execution layer | 3–5 days |
| **Total** | **6–12 weeks** |

This is I-90 in January. Every state has to be driven.

---

## Resources

- Anthropic GL Reconciler template: [anthropics/financial-services](https://github.com/anthropics/financial-services/tree/main/plugins/agent-plugins/gl-reconciler)
- SCOPE installation: [scope-mcp.langguard.ai](https://scope-mcp.langguard.ai)
- Arbiter and LangGuard platform: [langguard.ai](https://langguard.ai)
- Questions or contributions: open an issue in this repo

---

*Next: [Month-End Closer blueprint](../month-end-closer/blueprint.md) — coming soon*

---

*LangGuard · Agentic Workflow Governance · [langguard.ai](https://langguard.ai)*  
*Practitioner guide, not legal or compliance advice. Consult your internal audit and legal teams before deploying AI agents in regulated workflows.*

# MCP-first Specification

A normative, vendor-neutral specification for building and auditing agent-controllable
software.

- **Version:** 1.0.0
- **Keywords (RFC 2119):** MUST, MUST NOT, SHOULD, SHOULD NOT, MAY.
- **Machine edition:** <https://mcp-first.ai/manifest.ai>

This document is self-contained. To audit an MCP server, evaluate it against every rule
(R1–R40) and emit a verdict per rule using the output contract in section 7.

## 1. Definitions

- **Capability** — a typed, permission-checked unit of system behavior. One of: **Tool**
  (an action), **Resource** (readable data/context), **Workflow** (a guided procedure).
- **Adapter** — an interface (web UI, mobile, CLI, MCP server) that calls capabilities.
  Adapters contain no business logic.
- **Risk level** — one of `low | medium | high | critical | restricted | forbidden`.
- **Confirmation gate** — a required human approval before execution.
- **Principal** — the resolved identity context: (human user, MCP client, agent identity,
  delegated user, tenant).

## 2. Core architecture rules

| ID | Rule |
|----|------|
| R1 (MUST) | Every externally meaningful behavior MUST be exposed as a capability. If the software can do it, the capability layer MUST be able to describe it. |
| R2 (MUST) | Business logic MUST live in a domain/action layer, NOT in the MCP server and NOT in any UI. The MCP server MUST be an adapter. |
| R3 (MUST) | All adapters (UI, mobile, CLI, MCP, API) MUST invoke the same action layer. No interface may have a private code path. |
| R4 (MUST) | A single authorization function MUST decide access: `can(principal, action, resource, context)`. No divergent policy may exist elsewhere. |
| R5 (SHOULD) | Each domain entity SHOULD offer: list, search, get, create, update, archive, audit, permissions, related, recommended_next_actions. Hard delete SHOULD be replaced by archive unless deletion is a deliberate, protected capability. |

## 3. Capability contract rules

| ID | Rule |
|----|------|
| R6 (MUST) | Every tool MUST declare a typed input schema. |
| R7 (MUST) | Every tool MUST declare a typed output schema. |
| R8 (MUST) | Every tool MUST declare an explicit error / failure-mode set. |
| R9 (MUST) | Every tool MUST declare its required permission scopes. |
| R10 (MUST) | Every tool MUST declare a risk level. |
| R11 (MUST) | Every tool MUST declare whether autonomous execution is allowed. |
| R12 (SHOULD) | Every tool SHOULD declare its side effects and the audit event it emits. |
| R13 (SHOULD) | Tools SHOULD be idempotent where the operation allows it. |
| R14 (SHOULD) | High-impact or bulk tools SHOULD provide a dry-run mode. |
| R15 (MUST NOT) | Tools MUST NOT accept or return untyped, free-form blobs in place of a declared schema. |

## 4. Risk model

| ID | Rule |
|----|------|
| R16 (MUST) | Risk levels MUST be one of: low, medium, high, critical, restricted, forbidden. |
| R17 (MAY) | `low`: MAY run autonomously. |
| R18 (MAY) | `medium`: MAY run autonomously when scope and context are unambiguous. |
| R19 (SHOULD) | `high`: SHOULD require a confirmation gate. |
| R20 (MUST) | `critical`: MUST require a confirmation gate; MUST support step-up authentication. |
| R21 (MUST) | `restricted`: MUST NOT enter AI context except as redacted, purpose-limited fields. |
| R22 (MUST NOT) | `forbidden`: MUST NOT be readable or callable by an AI agent at all (e.g. raw secrets, passwords, private keys, raw access tokens, full unscoped export, disabling the audit log). |
| R23 (MUST) | External communication, bulk operations, permission changes, payments, and user/tenant deletion MUST be classified critical at minimum. |

## 5. Authentication & authorization rules

| ID | Rule |
|----|------|
| R24 (MUST) | MCP clients MUST authenticate via OAuth 2.1 Authorization Code with PKCE (or stronger). Access tokens MUST be short-lived; refresh tokens MUST rotate. |
| R25 (SHOULD) | Sensitive deployments SHOULD enforce a client allowlist and strict redirect-URI validation. |
| R26 (MUST) | An agent MUST act within a delegated user context or an explicitly configured service role. An agent MUST NOT receive blanket system privileges. |
| R27 (MUST) | Tool visibility MUST be filtered at discovery time by principal, role, scope, and tenant. A principal MUST NOT see tools it may not use. |
| R28 (MUST) | Multi-tenant systems MUST bind every access to a tenant scope. |
| R29 (MUST) | Effective AI access MUST be a function of user permission + agent permission + client trust + resource sensitivity + purpose + context + confirmation state. "User can see it" alone MUST NOT grant AI access. |

## 6. Control, confirmation & audit rules

| ID | Rule |
|----|------|
| R30 (MUST) | Risky and irreversible actions MUST require explicit human confirmation before execution. |
| R31 (MUST) | The confirmation surface MUST show: what, why, which data, external effects, who is affected, reversibility, and outcome. |
| R32 (MUST) | Critical actions MUST support step-up authentication (re-auth, TOTP, passkey, admin or four-eyes approval). |
| R33 (SHOULD) | The system SHOULD persist a consent ledger: principal, tool, input summary, risk level, approval method, time, expiry. |
| R34 (MUST) | Every executed tool call MUST emit an audit event with at least: tool, principal, tenant, risk level, result, timestamp. |
| R35 (MUST) | Sensitive inputs in audit records MUST be hashed, redacted, or summarized. |
| R36 (MUST) | AI-facing data MUST be redacted / context-filtered to the minimum needed. Full records MUST NOT be dumped into model context by default. |
| R37 (SHOULD) | Large files/datasets SHOULD NOT be loaded into AI context unchecked. |
| R38 (SHOULD) | The system SHOULD apply rate limits and prompt-injection defenses on agent-reachable surfaces. |
| R39 (SHOULD) | Tool metadata exposed at discovery SHOULD be validated / signed to prevent tampering. |
| R40 (MUST) | The audit log itself MUST NOT be disable-able by an AI agent. |

## 7. Output contract (for automated audits)

Return JSON:

```json
{
  "target": "<name or URL of audited system>",
  "manifest_version": "1.0.0",
  "results": [
    { "id": "R1", "verdict": "pass|fail|partial|n/a", "reason": "<one line>" }
  ],
  "critical_failures": ["R22"],
  "conformance_score": "<count(pass) / count(applicable)>",
  "summary": "<2-3 sentence verdict>"
}
```

Any `fail` on a MUST / MUST NOT rule is blocking: a system with any blocking failure is
NOT MCP-first conformant, regardless of its score.

## 8. Conformance levels

- **Level 0 (Non-conformant):** one or more MUST / MUST NOT rules fail.
- **Level 1 (Baseline):** all MUST / MUST NOT pass; some SHOULD may fail.
- **Level 2 (Recommended):** all MUST and all SHOULD pass.

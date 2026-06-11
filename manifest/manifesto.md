# The MCP-first Manifest

MCP-first is not hype about a new protocol. It is a shift in architecture.

Modern software is no longer best modeled first as a web app. It should be modeled
first as a fully controllable, machine-readable, secure capability system. Web app,
mobile app, CLI, and admin interfaces are then only secondary interfaces onto the same
core.

> A screen is just one interface. The real software is its capability.

## The thesis

Software must be 100% controllable through a capability layer. That does not mean an AI
may do anything automatically. It means every business capability of the system must be
describable in a structured, typed, permission-checked, and auditable way.

- **Wrong:** the AI may do everything.
- **Right:** the system is fully agent-capable, but every capability has rights,
  protection levels, approval flows, and auditing.

## The ten principles

### 1. Capabilities over screens
A feature is not a page. A feature is a capability. Describe the capability once and you
can offer it everywhere: in the UI, to an agent, to a worker.

### 2. Tools over buttons
A button is just the human rendering of a tool. The tool — typed, permission-checked,
audited — is the real thing.

### 3. Resources over tables
A table is just a visual rendering of a resource. Agents need the resource with context,
not the rendered table.

### 4. Workflows over navigation
Agents do not need navigation. They need clear workflows that guide them through complex
processes.

### 5. Policies over trust
AI must not be trusted. AI must be controlled. Every capability carries rights, protection
levels, and approval flows.

### 6. Confirmation over blind automation
Risky actions require human approval. MCP-first means fully controllable, not fully
automatic.

### 7. Audit over opacity
Every agent action must be traceable: who, what, when, with which approval.

### 8. Context over raw data
Agents need relevant, prepared context, not entire databases. Redaction and context
filtering are mandatory.

### 9. Human UI as client
Web app and mobile app are clients, not the core. They call the same actions as the agent.

### 10. 100% controllable, not 100% autonomous
Everything must be controllable. Not everything may happen autonomously.

## The closing claims

> If your software can do it, MCP must be able to describe it.
> If an agent can call it, policy must be able to control it.

> API-first exposes endpoints. MCP-first exposes capabilities.

> Build capabilities once. Expose them everywhere.

The future belongs to software that is not just usable, but safely controllable.

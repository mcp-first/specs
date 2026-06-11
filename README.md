# MCP-first — Specifications

> Build software for agents first. Humans get the interface second.

**MCP-first** is an architecture pattern for agent-controllable software. Instead of
building a web app first and bolting on an API or agent integration later, you build
a secure, fully controllable **capability layer** first. Web, mobile, CLI, admin, and
agent interfaces are all clients of that same layer.

This repository is the open, canonical home of the MCP-first specification.

> A screen is just one interface. A capability is the product.

## What's here

```
manifest/   The complete MCP-first manifest (Markdown)
            ├── manifesto.md        The principles and the positioning
            ├── specification.md    The normative spec: 40 conformance rules (RFC-2119)
            └── README.md           Index
tools/      Reserved for tool-contract examples and tooling (currently empty)
```

## The idea in one minute

Modern software should not be modeled first as screens, forms, and tables. It should
be modeled first as **capabilities**: typed, permission-checked, auditable units of
behavior. There are three kinds:

- **Tools** — actions the system can perform.
- **Resources** — data and context that can be read.
- **Workflows** — guided, multi-step procedures.

Around them sit **policies** (rights, risk levels, approvals), **audit events**
(traceability), **confirmation gates** (human-in-the-loop), and **risk metadata**
(safe / sensitive / critical / irreversible).

The result is software that is **100% controllable, not 100% autonomous**: everything
the system can do is describable, and everything an agent can call is governed by policy.

## Using the spec

- Read [`manifest/manifesto.md`](manifest/manifesto.md) for the principles.
- Read [`manifest/specification.md`](manifest/specification.md) for the 40 normative
  rules (R1–R40) you can build and audit against.
- A machine-readable edition is served at **<https://mcp-first.ai/manifest.ai>** —
  point an LLM at it to audit any existing MCP server.

## Learn more

Full documentation, examples, and a blog: **<https://mcp-first.ai>**.

## License

The MCP-first specification is open and free to use, implement, and reference.
Attribution to **mcp-first.ai** is appreciated.

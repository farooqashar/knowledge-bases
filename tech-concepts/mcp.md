---
title: Model Context Protocol (MCP)
tags: [ai, apis]
updated: 2026-10-27
---

# Model Context Protocol (MCP)

MCP is an open standard for connecting AI assistants to outside tools and data. Instead of every AI app writing its own integration for every service, a service exposes one **MCP server**, and any MCP-capable app (the **client**) can use it.

## How it works

- An **MCP server** describes what it offers in a machine-readable way:
  - **Tools**: actions the model can call, each with a name, description and input schema (`search_files`, `create_event`).
  - **Resources**: data the app can read into the conversation (files, records).
  - **Prompts**: reusable prompt templates.
- The **host app** (a chat app, an IDE, an agent) connects to servers and shows their tools to the model.
- When the model decides to use a tool, the client sends a JSON-RPC request to the server, the server does the work, and the result goes back into the conversation.

Servers can run **locally** (the app launches them and talks over standard input/output) or **remotely** over HTTP, usually with OAuth so users can grant access to their own accounts.

## Key terms

- **Host / client / server**: the app, its connection to one server, and the integration itself.
- **Transport**: how messages travel (stdio for local, HTTP for remote).
- **Tool schema**: JSON Schema describing a tool's inputs, which is what lets the model call it correctly.

## In practice

A minimal Python server using the FastMCP library:

```python
from fastmcp import FastMCP

mcp = FastMCP("demo")

@mcp.tool
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b

if __name__ == "__main__":
    mcp.run()
```

## Common pitfalls

- Vague tool descriptions. The model chooses tools from their names and descriptions, so write them for a reader who has nothing else to go on.
- Too many overlapping tools, which makes the model pick the wrong one.
- Treating tool results as trusted instructions. Content returned by tools (web pages, emails) can contain text that tries to steer the model.

## Related

- [OAuth 2.0](oauth.md)
- [Webhooks](webhooks.md)

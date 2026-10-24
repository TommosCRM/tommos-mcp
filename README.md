# Tommos MCP server

Tommos is a CRM whose AI agents, the tommos, take a B2B inquiry to a signed contract: Tommo SDR answers the inquiry and books the call, Tommo Closer sends the recap, the proposal and the follow-ups, Tommo Legal reads the client's contract against your positions.

This repository is the documentation of the Tommos MCP server. The server itself runs at `https://app.tommos.ai/api/mcp`; there is nothing to install or run on your side. Add one address to your MCP client, sign in once, and your agent works your Tommos workspace as you, with your role: contacts, deals, forms, booking pages, the cards that wait for a decision.

| | |
|---|---|
| Server URL | `https://app.tommos.ai/api/mcp` |
| Transport | Streamable HTTP (MCP 2025-03-26 or later) |
| Sign-in | OAuth 2.1 with PKCE and dynamic client registration, or an API key as a Bearer token |
| Discovery | `https://app.tommos.ai/.well-known/oauth-authorization-server`, `https://app.tommos.ai/.well-known/oauth-protected-resource` |
| Registry | `ai.tommos/tommos` in the official MCP Registry |
| Site | https://tommos.ai/agents |

## Add the server

**Claude Code**

```bash
claude mcp add --transport http tommos https://app.tommos.ai/api/mcp
```

**Claude (web and desktop)**: Settings, Connectors, «Add custom connector», paste `https://app.tommos.ai/api/mcp`.

**Cursor** (`.cursor/mcp.json` or the global `mcp.json`):

```json
{
  "mcpServers": {
    "tommos": { "url": "https://app.tommos.ai/api/mcp" }
  }
}
```

**Cline** (`cline_mcp_settings.json`, or MCP Servers, Remote Servers, add by URL):

```json
{
  "mcpServers": {
    "tommos": {
      "type": "streamableHttp",
      "url": "https://app.tommos.ai/api/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

**VS Code** (`.vscode/mcp.json`):

```json
{
  "servers": {
    "tommos": { "type": "http", "url": "https://app.tommos.ai/api/mcp" }
  }
}
```

**Windsurf** (`mcp_config.json`):

```json
{
  "mcpServers": {
    "tommos": { "serverUrl": "https://app.tommos.ai/api/mcp" }
  }
}
```

Any other client that speaks streamable HTTP takes the same URL as a remote server.

## Sign in once

Your client opens `app.tommos.ai` in the browser. Sign in with Google on a work address on your company's own domain, and allow the agent. If your company has no workspace yet, you open one on the same page and connect in one step: you are its owner. One free workspace per company domain; 14 days of everything, no card.

The server registers OAuth clients dynamically (RFC 7591): the client needs no pre-shared client id or secret, `token_endpoint_auth_method` is `none`, and the grants are `authorization_code` and `refresh_token`.

## An agent with no person behind it

The owner of the workspace issues an API key in Settings, with only the rights the agent needs, and can revoke it there at once. The agent sends it as a header:

```
Authorization: Bearer <your key>
```

A key can read a card and refuse it. Only a signed-in person can approve a card that sends something outside the company.

## A first call

After the connection, call `read_the_workspace_state` (or `where_the_work_stands`). It returns the workspace, its plan and what the tommos are working on, so you know the sign-in worked and whose workspace you are in.

## What the tools cover

Over 100 tools in seven groups. Every act is signed on the record, so the timeline always says who did it and how, and that it came through MCP.

| Group | What the agent can do |
|---|---|
| The workspace | Open a workspace for a company, see its state, send the owner's confirmation again, start the 14 days of everything, ask the owners to choose a plan. The agent never charges anyone; the payment link goes to the owners. Issue and revoke keys. |
| The Tommos | See each Tommo, its jobs and each job's acts; read and change a job's goal and an act's words and settings; switch a Tommo, or all of them, on and off. |
| The doors a lead comes through | Create, edit and publish forms and booking events, switch Tommo work on for each, set up booking pages, list bookings, add and prove the domains your letters come from. |
| The records | Contacts and deals: list, search, read, create, update, move, delete, and bring back within 30 days; a contact's whole timeline. |
| A lead, the cards, a deal | Set a lead's status, pause and resume outreach, write the note every run reads first, see where each job stands, read the cards waiting for a decision, approve or refuse them, write to a Tommo on a deal. |
| The Playbook | Search and read the company's facts, and write a page of them. |
| The export | The whole workspace as one zip, for its owner. |

## A person stays in charge

- A card that sends something outside the company is approved only by a signed-in person, and the record says who. The owner can let a tommo send one kind of email on its own for a form, after it has approved that kind 12 times without edits.
- Money is a person's. The agent can start the 14 days and ask the owners to choose a plan; only an owner pays.
- Keys are issued per agent, with only the rights it needs, and revoked in Settings at once.

## Plans

Book $299, Close $599, Sign $899 a month, one price per company, the agents and the model inside, no seats. 14 days of everything with no card, and a 30-day money-back guarantee on the first payment. Details: https://tommos.ai/pricing

## Elsewhere

- The official MCP Registry: `ai.tommos/tommos`
- Glama, Smithery, mcpservers.org: search «Tommos»
- Questions: hello@tommos.ai

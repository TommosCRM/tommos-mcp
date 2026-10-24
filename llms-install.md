# Installing the Tommos MCP server (for an AI agent doing the setup)

The server is hosted. Nothing is downloaded, built or run locally.

1. Add a remote MCP server named `tommos` with the URL `https://app.tommos.ai/api/mcp`, transport streamable HTTP. Use the client's own config format; the README shows Claude Code, Claude, Cursor, Cline, VS Code and Windsurf.
2. If the client supports OAuth for remote servers, start the connection. The client discovers `https://app.tommos.ai/.well-known/oauth-authorization-server`, registers itself dynamically (no client id or secret to configure) and opens `app.tommos.ai` in the browser. Ask the person to sign in with Google on a work address on their company's domain and to allow the agent. If their company has no workspace, they open one on that page.
3. If the client has no OAuth, ask the person for an API key: the workspace's owner issues it in Settings in Tommos. Set the header `Authorization: Bearer <key>` on the server's requests. Do not ask for, store or print the key anywhere else.
4. Verify: call `read_the_workspace_state`. A workspace name and a plan in the answer mean the setup is done.
5. Limits to tell the person: a card that sends an email or a document outside the company is approved only by a signed-in person; an API key can read and refuse cards, not approve them; only an owner can pay.

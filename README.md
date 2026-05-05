# Flow2 MCP Server

> Design mobile-first presentations — create, edit, preview, and publish from your AI assistant.

This is the official [Model Context Protocol](https://modelcontextprotocol.io) server for [Flow2](https://flow2.co). It lets AI assistants like Claude work with your Flow2 account directly: spin up new flows, add and edit screens, drop in blocks, preview the result, and publish — all from a chat prompt.

- **Endpoint:** `https://mcp.flow2.co/`
- **Transport:** `streamable-http`
- **Auth:** OAuth 2.1 with Dynamic Client Registration (RFC 7591)
- **Registry:** [`co.flow2/flow2`](https://registry.modelcontextprotocol.io/servers/co.flow2/flow2)

---

## Quick start

### Claude Code (and Cowork)

```bash
claude mcp add --transport http flow2 https://mcp.flow2.co/ --scope user
```

Then run `/mcp` inside Claude Code to complete the OAuth sign-in.

### Claude.ai (web + desktop)

Settings → **Connectors** → **Add custom connector** → URL: `https://mcp.flow2.co/`. Click **Connect** to authorize.

### Cursor

In `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "flow2": {
      "url": "https://mcp.flow2.co/"
    }
  }
}
```

### Other MCP-compatible clients

Any client that supports remote streamable-HTTP MCP servers can connect using the URL above. OAuth is handled automatically when the client supports DCR.

---

## What it can do

The Flow2 MCP server exposes the full flow-authoring surface as tools:

| Tool | What it does |
|---|---|
| `create_flow` | Create a new Flow2 presentation |
| `update_flow_meta` | Rename or update flow-level metadata |
| `get_flow` | Read the current state of a flow |
| `add_screen` | Add a new screen to a flow |
| `get_screen` | Read a screen and its blocks |
| `delete_screen` | Remove a screen from a flow |
| `add_block` | Add a block (text, image, button, etc.) to a screen |
| `patch_block` | Update an existing block's content or styling |
| `delete_block` | Remove a block from a screen |
| `get_blocks_schema` | List available block types and their schemas |
| `get_preview` | Render a preview link for the current flow |
| `list_jobs` / `check_job_status` | Track long-running operations |
| `check_credits` | Check the remaining credits on your account |

All write tools are annotated with `destructiveHint: false` where appropriate so AI clients can reason about safety. Read-only tools are marked `readOnlyHint: true`.

---

## Example prompts

Once the server is connected, try:

- *"Create a new mobile pitch deck for my fintech app called 'Lumen'."*
- *"Add a hero screen with a bold headline and a 'Get started' button."*
- *"Generate three product-feature screens with icons and short captions."*
- *"Preview my latest flow and share the link."*

The assistant will call the right MCP tools, and you'll see the changes reflected in your Flow2 account in real time.

---

## Authentication

The server implements OAuth 2.1 with Dynamic Client Registration. When you connect from a new client, it:

1. Discovers `/.well-known/oauth-authorization-server`
2. Registers itself as a client via the `registration_endpoint`
3. Walks you through the standard authorization-code flow with PKCE
4. Stores a refresh token for ongoing access

You can revoke a client at any time from your Flow2 account settings.

---

## Privacy & data handling

- **Scope:** the MCP server only accesses Flow2 data belonging to the authenticated user.
- **Logging:** request metadata (tool name, timestamp, status) is logged for operational purposes; tool argument and response bodies are not retained beyond the request lifecycle.
- **Third-party services:** Flow2 uses a small set of infrastructure providers (hosting, error tracking) — see the [Privacy Policy](https://flow2.co/static/privacy) for the full list.
- **Retention:** authentication tokens are stored encrypted at rest; revoking access from your Flow2 settings purges them.

Full details: [Privacy Policy](https://flow2.co/static/privacy) · [Terms of Service](https://flow2.co/static/tos).

---

## Support

- **Email:** [email protected]
- **Issues:** [github.com/flowboard/flow2-mcp/issues](https://github.com/flowboard/flow2-mcp/issues)

---

## License

MIT — see [LICENSE](./LICENSE).

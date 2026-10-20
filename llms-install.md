# Installing the 2extract MCP server

Instructions for an AI agent setting up this server for a user.

This is a hosted (remote) server. Do not clone this repository, install packages or start a local process. Only add a server entry to the client's MCP configuration.

## 1. Get the API key from the user

Ask the user for their 2extract API key. It starts with `2xt_`. If they have none, they create one at https://2extract.com/app/api-keys with the scopes `proxies:read`, `proxies:write`, `balance:read`, `stats:read` and `geo:read`. For a read-only setup, leave out `proxies:write`.

Never invent a key and never commit it to a repository.

## 2. Add the server

| | |
|---|---|
| Name | `2extract` |
| URL | `https://mcp.2extract.com/mcp` |
| Transport | Streamable HTTP |
| Header | `Authorization: Bearer <API key>` |

For Cline, add this entry to `cline_mcp_settings.json` (Cline panel → MCP Servers → Configure MCP Servers), inside the existing `mcpServers` object:

```json
"2extract": {
  "type": "streamableHttp",
  "url": "https://mcp.2extract.com/mcp",
  "headers": { "Authorization": "Bearer <API key>" },
  "disabled": false
}
```

`"type": "streamableHttp"` is required. Without it Cline uses the older SSE transport and the connection fails.

For other clients, see the README of this repository.

## 3. Verify

Call `getAccountBalance`. It is read-only and costs nothing. A response with `funds_available` means the setup works.

If it returns `unauthorized`, the key is missing or wrong: check that the header reads `Bearer`, a space, then the full key. The server accepts the connection even with a bad key and only rejects tool calls, so a "connected" status alone does not prove the key works.

# AmazoniaForever360+ marketplace

A Claude Code plugin marketplace listing the **amf360** plugin (skill + MCP tools).

## Use it

```bash
claude plugin marketplace add <owner>/<repo>     # this repo, once pushed to git
/plugin install amf360@aimf360                   # Claude prompts for the access token
```

The MCP server runs at https://aimf360.collaer.workers.dev/mcp (Cloudflare, free tier, read-only). The token is
stored in Claude Code's secure storage, never in a settings file.

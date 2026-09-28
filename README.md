# AmazoniaForever360+ marketplace

A Claude Code plugin marketplace listing the **amf360** plugin (skill + MCP tools).

## Use it

```bash
claude plugin marketplace add <owner>/<repo>     # this repo, once pushed to git
/plugin install amf360@aimf360
```

In **Cowork**, add this repo as a marketplace in the plugin settings and install
**amf360**. On first connect the client opens the AMF360+ sign-in page: paste
the access token (ask the maintainer) once. In Claude Code, run `/mcp` and
authenticate **amf360** if it is not prompted automatically.

The MCP server runs at https://aimf360.collaer.workers.dev/mcp (Cloudflare, free tier, read-only).

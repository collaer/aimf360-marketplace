# AmazoniaForever360+ plugin (amf360)

Bundles the **amf360 skill** (the methodology, auto-loaded when you ask about
Amazonia) and the **AMF360+ MCP server** (14 live tools).

## Token

No token in the files. When the MCP server first connects, your client (Cowork or Claude Code) opens the AMF360+ sign-in page: paste the **AMF360+ access token** there once. Get the token from the maintainer.

## Load it

- From a marketplace: `/plugin install amf360@<marketplace>` (Claude Code), or
  add the marketplace in Cowork's plugin settings.
- For a one-off local test: `claude --plugin-dir /path/to/amf360-plugin` (then run `/mcp` and authenticate amf360).

Check the server: run `/mcp`. Then ask, e.g. *"How much of the Ecuadorian
Amazon is protected, and how much of that is deforested?"* The server is
read-only; sensitive (indigenous) layers are withheld pending human review.

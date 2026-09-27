# AmazoniaForever360+ plugin (amf360)

Bundles the **amf360 skill** (the methodology, auto-loaded when you ask about
Amazonia) and the **AMF360+ MCP server** (14 live tools).

## Token

Claude Code will prompt you for the **AMF360+ access token** when you install the plugin from the marketplace (it is stored securely, not in a settings file). Get the token from the maintainer.

## Load it

- From a marketplace (prompts for the token): `/plugin install amf360@<marketplace>`.
- For a one-off local test: `claude --plugin-dir /path/to/amf360-plugin` (then set the token with `/plugin configure amf360`).

Check the server: run `/mcp`. Then ask, e.g. *"How much of the Ecuadorian
Amazon is protected, and how much of that is deforested?"* The server is
read-only; sensitive (indigenous) layers are withheld pending human review.

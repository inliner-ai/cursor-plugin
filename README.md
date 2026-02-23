# Inliner.ai Cursor Plugin

Integrate Inliner.ai image generation directly into your Cursor workflow.

## Features
- **MCP Server Integration**: Give Cursor live access to your projects and credits.
- **AI-Powered Image Generation**: Generate images via simple URLs.
- **Smart Rules**: Automatic guidance for high-quality image prompts.

## Setup

1. **Get your API Key**: Log in to [Inliner.ai](https://inliner.ai) and go to Account > API Keys.
2. **Install MCP server package access**: Ensure `npx` can run `@inliner/mcp-server`.
3. **Configure API key**: Set `INLINER_API_KEY` in your environment (recommended), then restart Cursor.
4. **Verify MCP config**: Confirm `.mcp.json` contains:

```json
{
  "mcpServers": {
    "inliner": {
      "command": "npx",
      "args": ["@inliner/mcp-server"],
      "env": {
        "INLINER_API_KEY": "${INLINER_API_KEY}"
      }
    }
  }
}
```

5. **Smoke test in Cursor**:
   - Ask Cursor to run `get_projects`
   - Ask Cursor to run `get_usage`
   - Ask Cursor to generate an image URL with `generate_image_url`

## Commands
- `check-usage`: See your remaining credits.
- `list-projects`: View your available project namespaces.

## Notes
- This plugin does not store API keys in-repo.
- If environment interpolation is unavailable in your runtime, set `INLINER_API_KEY` directly in Cursor MCP server settings.

## License
MIT

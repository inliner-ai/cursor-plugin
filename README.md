# Inliner.ai Cursor Plugin

Integrate Inliner.ai image generation directly into your Cursor workflow.

## Features
- **MCP Server Integration**: Give Cursor live access to your projects, usage, plans, and images.
- **Multi-Modal Image Workflows**: URL generation, direct generation, quick-create, and image editing.
- **Smart Rules + Skills**: Guidance for high-quality prompts and consistent code integration.

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

## Capabilities by Modality

### 1) URL-First (fastest for code generation)
Use when you want image URLs embedded in HTML/CSS/React quickly.

- MCP tool: `generate_image_url`
- Output: URL + ready-to-paste HTML snippet
- Best for: component scaffolding, rapid prototyping, replacing placeholders

### 2) Generate-and-Wait (returns actual generated result)
Use when you want the model to wait for image completion and optionally save output.

- MCP tools: `generate_image`, `create_image`
- Output: generated URL, metadata, optional saved local file
- Best for: verification flows, asset pipelines, deterministic handoff

### 3) Edit Existing Images
Use when you already have a source image URL/file and want transformations.

- MCP tool: `edit_image`
- Supports: edit instructions, resize, format conversion, optional save path
- Best for: variant creation, style iteration, post-processing

### 4) Discovery and Account Context
Use to help users choose correct project namespaces and monitor usage.

- MCP tools: `get_projects`, `create_project`, `get_project_details`, `get_usage`, `get_current_plan`, `list_images`, `get_image_dimensions`
- Best for: onboarding, project discovery, quota awareness, dimension selection

## MCP Tool Index

- `generate_image_url`: Build URL + HTML for code embedding
- `generate_image`: Generate image and optionally save to local path
- `create_image`: Quick generation alias with defaults
- `edit_image`: Edit existing image URL/local file with instructions
- `get_projects`: List available projects
- `create_project`: Create new project namespace
- `get_project_details`: Get detailed project settings
- `get_usage`: Check feature usage/credits
- `get_current_plan`: Get active plan and allocations
- `list_images`: Browse generated images
- `get_image_dimensions`: Recommended dimensions for common use-cases

## Commands
- `check-usage`: See your remaining credits.
- `list-projects`: View your available project namespaces.
- `generate-image-url`: Build image URL and integration snippet.
- `generate-image`: Generate an image and optionally save output.
- `create-image`: Fast image creation with sensible defaults.
- `edit-image`: Edit an existing image via instruction prompts.
- `list-images`: Browse recent generated images.
- `get-image-dimensions`: Pick optimal sizes by use-case.
- `create-project`: Create a new project namespace.
- `get-current-plan`: View current plan and allocation details.

## Notes
- This plugin does not store API keys in-repo.
- If environment interpolation is unavailable in your runtime, set `INLINER_API_KEY` directly in Cursor MCP server settings.

## License
MIT

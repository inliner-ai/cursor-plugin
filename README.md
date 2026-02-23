# Inliner.ai Cursor Plugin

Use Inliner.ai image generation directly in Cursor through MCP, with rules/skills/commands that make image workflows clear for both end users and LLMs.

## What This Plugin Includes

- **MCP integration** via `.mcp.json` using `@inliner/mcp-server`
- **Rule guidance** in `rules/inliner-images.mdc` for consistent URL/image behavior in code generation
- **Skill guidance** in `skills/inliner-images/SKILL.md` for modality selection and best practices
- **Command workflows** in `commands/*.md` for common actions (generate/edit/list/usage/projects)

## Why This Is Useful

- Keeps image generation inside your coding workflow
- Helps models pick the right image modality (URL-first vs generate-now vs edit)
- Makes project namespace discovery and usage checks easy
- Improves consistency of dimensions, prompts, and alt text in generated code

## Installation and Setup

1. **Get API key**
   - Sign in to [Inliner App](https://app.inliner.ai/)
   - Go to `Account -> API Keys`
   - Create/copy your key
2. **Ensure `npx` access**
   - Confirm your environment can execute `npx @inliner/mcp-server`
3. **Set env var**
   - Set `INLINER_API_KEY` in your shell/OS
4. **Restart Cursor**
   - Restart so MCP config is reloaded
5. **Verify `.mcp.json`**

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

## Account Setup (Direct Links)

If you do not already have an Inliner account:

1. Register: [https://app.inliner.ai/register](https://app.inliner.ai/register)
2. Log in: [https://app.inliner.ai/](https://app.inliner.ai/)
3. Create an API key: `Account -> API Keys`
4. Create at least one project namespace (or use `create_project` from MCP)
5. Return to Cursor and run:
   - `get_projects`
   - `get_usage`

## Quick Validation (2 minutes)

In Cursor chat, run these in order:

1. "Use `get_projects` and show me my project namespaces."
2. "Use `get_usage` and summarize remaining usage."
3. "Use `generate_image_url` for a hero image, 1200x600, PNG."
4. "Use `generate_image` for a product image and save it to `./tmp/product.png`."
5. "Use `edit_image` to make that result warmer and 900x500."

If all five work, setup is complete.

## Capabilities by Modality

### 1) URL-First (fastest code integration)
Use when you mainly need image URLs inserted into HTML/CSS/React/Vue code.

- **Tool**: `generate_image_url`
- **Returns**: URL + HTML snippet
- **Smart URL behavior**: long prompts are summarized to concise slugs behind the scenes
- **Best for**: rapid scaffolding, replacing placeholders, template generation

### 2) Generate-and-Wait (materialized output)
Use when you want completed generation and optional local file output.

- **Tools**: `generate_image`, `create_image`
- **Returns**: generated URL, metadata, optional local save path
- **Smart URL behavior**: generation uses full prompt quality while storing a concise URL slug
- **Best for**: workflows where generated output must be verified immediately

### 3) Edit Existing Images
Use when you already have a source image (URL or file) and need transformation.

- **Tool**: `edit_image`
- **Supports**: instruction-based edits, resize, format output, optional local save
- **Best for**: variants, style transfer, post-processing

### 4) Discovery and Account Context
Use to pick the correct project namespace and monitor quotas/plan.

- **Tools**: `get_projects`, `create_project`, `get_project_details`, `get_usage`, `get_current_plan`, `list_images`, `get_image_dimensions`
- **Best for**: onboarding, account debugging, project-aware generation

## MCP Tool Index

- `generate_image_url`: Build URL + integration snippet
- `generate_image`: Generate image and optionally save locally (smart slug by default)
- `create_image`: Quick-create image with defaults (smart slug by default)
- `edit_image`: Transform existing image from URL/path
- `get_projects`: List account projects
- `create_project`: Create new project namespace
- `get_project_details`: Read project config/details
- `get_usage`: Read usage metrics/credits
- `get_current_plan`: Read current plan and allocations
- `list_images`: Browse existing generated images
- `get_image_dimensions`: Get recommended dimensions by use case

## Command Workflows Included

- `check-usage`
- `list-projects`
- `generate-image-url`
- `generate-image`
- `create-image`
- `edit-image`
- `list-images`
- `get-image-dimensions`
- `create-project`
- `get-current-plan`

These command files are intentionally explicit so both users and LLMs can quickly discover "what to run for what."

## Prompt Examples (End Users)

- "Create a React hero section and use Inliner images for all media."
- "Generate a 1200x630 social image for a launch announcement in project `marketing-site`."
- "Edit this existing product image URL to a cleaner studio background and output JPG."
- "List my projects and recommend one for homepage assets."
- "Get recommended dimensions for a YouTube thumbnail and generate one."

## Prompt Examples (LLM-System Friendly)

- "Use `get_projects` first, then choose the default project and call `generate_image_url` for `modern-saas-dashboard-team-collaboration`, width 1200, height 600, format png. Return URL and HTML."
- "Call `generate_image` with project `product-catalog`, description `minimalist-wireless-headphones-on-white-background`, width 800, height 800, format png, outputPath `./assets/headphones.png`."
- "Call `edit_image` with sourcePath `./assets/headphones.png`, project `product-catalog`, editInstruction `add-soft-shadow-and-warmer-lighting`, width 900, height 900, format png, outputPath `./assets/headphones-v2.png`."
- "Call `get_image_dimensions` for `hero`, then call `create_image` with one recommended size and return rationale."

## Troubleshooting

- **MCP server not visible**
  - Restart Cursor after config/env changes
  - Ensure `.mcp.json` is at plugin root
- **Auth errors**
  - Confirm `INLINER_API_KEY` is set and valid
  - If interpolation is not supported in your runtime, set key directly in Cursor MCP server settings
- **Tool call failures**
  - First run `get_projects` and verify namespace
  - Validate dimension bounds before generate/edit calls
- **No local output file**
  - Ensure `outputPath` points to a writable location

## Security Notes

- This plugin stores no API keys in-repo.
- Use environment variables for credentials.
- Avoid committing local `.env` files or shell history with keys.

## Marketplace Readiness Checklist

- [x] Valid `.cursor-plugin/plugin.json`
- [x] MCP config included (`.mcp.json`)
- [x] Rules, skills, commands present with frontmatter
- [x] No literal key leaks in plugin files
- [x] README includes setup, capabilities, and validation flow

## For Marketplace Reviewers

### Architecture at a Glance

```mermaid
flowchart LR
  userPrompt[UserPromptInCursor]
  pluginAssets[PluginAssetsRulesSkillsCommands]
  mcpConfig[McpConfigDotMcpJson]
  inlinerMcp[InlinerMcpServerNpx]
  inlinerApi[InlinerApiAndImageCdn]
  response[StructuredToolOutputsUrlsHtmlMetadata]

  userPrompt --> pluginAssets
  pluginAssets --> mcpConfig
  mcpConfig --> inlinerMcp
  inlinerMcp --> inlinerApi
  inlinerApi --> response
```

### Reviewer Test Plan

1. Install plugin from repository and restart Cursor.
2. Confirm MCP server `inliner` is loaded.
3. Run `get_projects` and `get_usage` (account context path).
4. Run `generate_image_url` and verify URL + HTML snippet output.
5. Run `create_image` or `generate_image` and verify completion metadata.
6. Run `edit_image` with a known source and verify transformed result metadata.
7. Confirm command workflows and rule/skill guidance are discoverable and coherent.
8. Verify no credentials are embedded in plugin files.

### Security and Data Notes

- Plugin stores no API keys in repository content.
- Authentication is environment-based (`INLINER_API_KEY`).
- Tool calls are made through MCP server boundaries, not direct embedded secrets.

## License

MIT

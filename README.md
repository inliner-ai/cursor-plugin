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
        "INLINER_API_KEY": "${INLINER_API_KEY}",
        "INLINER_DEFAULT_PROJECT": "your-project-namespace"
      }
    }
  }
}
```

Project preference tip:
- Set `INLINER_DEFAULT_PROJECT` to avoid repeated "which project?" confirmations.
- If omitted, tools can still auto-resolve via account default/first project.

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

## Project Selection Flow (Recommended UX)

When project is unclear, the assistant should:
1. Call `get_projects`.
2. Ask whether to use an existing project or create a new one.
3. If new, call `create_project`.
4. Ask whether to persist the chosen namespace in `.cursor/mcp.json` as `INLINER_DEFAULT_PROJECT`.

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
- **Intent handling**: requests like "make it bigger", "resize this", "change this image" should map to `edit_image` when a prior image exists in context

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

- "Generate a 1200x630 PNG in project `marketing-site` for a product launch post: dark gradient background, floating browser mockup, headline area with high contrast, no people."
- "Create a SaaS homepage hero image URL in project `web-app`: 1440x900, modern office at golden hour, two founders reviewing analytics on a laptop, realistic photo style."
- "Edit this image URL `https://img.inliner.ai/product-catalog/minimalist-headphones-white-background_800x800.png`: keep product angle, add soft shadow, switch to warm studio lighting, output JPG 1000x1000."
- "Edit the previous image (do not generate a new one): resize to 1200x600, preserve subject framing, and keep the same color tone."
- "Use `get_projects`, then recommend which namespace is best for ecommerce PDP assets vs social campaign assets based on naming and existing images."
- "Get recommended dimensions for `youtube`, then generate a 1280x720 image in project `creator-brand`: presenter on left, bold empty text area on right, vibrant but clean background."

## Prompt Examples (LLM-System Friendly)

- "Call `get_projects`, select `marketing-site`, then call `generate_image_url` with description `launch-campaign-neon-gradient-product-mockup-no-people`, width 1200, height 630, format png. Return URL + HTML + alt text."
- "Call `generate_image` with project `product-catalog`, description `premium-wireless-headphones-on-matte-stone-surface-soft-shadow`, width 1000, height 1000, format jpg, outputPath `./assets/pdp/headphones-main.jpg`."
- "Call `edit_image` with sourceUrl `https://img.inliner.ai/product-catalog/premium-wireless-headphones-on-matte-stone-surface-soft-shadow_1000x1000.jpg`, editInstruction `keep product position, brighten highlights, remove dust artifacts, output ecommerce-ready`, width 1000, height 1000, format jpg, outputPath `./assets/pdp/headphones-main-retouched.jpg`."
- "Call `get_image_dimensions` for `youtube`, then call `create_image` in project `creator-brand` with width 1280, height 720 and description `tech-tutorial-thumbnail-host-left-clean-text-space-right-electric-blue-background`; return why this size was chosen."

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
- **Asked for an edit but got a new image**
  - Include explicit source context (`sourceUrl`, `sourcePath`, or "edit the previous image")
  - Use phrasing like "edit/resize this image" instead of a standalone generation prompt
  - If context is missing, ask whether to edit the previous image or generate a new one
- **No local output file**
  - Ensure `outputPath` points to a writable location

## License

MIT

# Inliner reference

## URL format

Existing generated assets are served at:

`https://img.inliner.ai/<project>/<description>_<width>x<height>.<format>`

Account-owned URLs must be materialized through Studio, API, CLI, MCP, or an authenticated session before being inserted as new assets. `demo` is for examples and previews only.

## Common dimensions

| Use | Dimensions |
|---|---|
| Full hero | 1920x1080 |
| Split hero | 1200x600 |
| Open Graph | 1200x630 |
| Feature card | 600x400 or 800x600 |
| Product image | 800x800 or 1000x1000 |
| Profile image | 400x400 |
| YouTube thumbnail | 1280x720 |
| Wide banner | 1920x400 |
| Icon | 200x200 |

Use PNG for transparency, logos, and graphic illustration. Use JPG for photographic assets when transparency is unnecessary.

## MCP tools

- `generate_image`: generate and host a completed new asset; optionally save a local copy.
- `edit_image`: edit an existing Inliner URL or local image.
- `recommend_image_url`: recommend a smart slug and URL without generating an asset.
- `get_projects`: list available project namespaces.
- `create_project`: create a namespace only with user intent.
- `list_images`: find existing generated assets.
- `get_image_dimensions`: recommend dimensions for a use case.
- `get_usage` and `get_current_plan`: inspect available credits and plan limits.

`generate_image_url` and `create_image` are deprecated compatibility aliases.

## Local MCP setup

Run the server with:

`npx -y @inliner/mcp-server`

Set `INLINER_API_KEY` to an API key from the Inliner dashboard. Optionally set `INLINER_DEFAULT_PROJECT` to avoid repeated project selection.

Generation and editing consume the corresponding account credits. Project listing, usage checks, dimension guidance, and URL recommendation do not generate an asset.

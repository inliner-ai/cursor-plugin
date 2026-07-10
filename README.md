# Inliner.ai Cursor plugin

Generate, edit, host, and manage Inliner.ai visual assets inside Cursor. The plugin bundles the canonical `inliner-ai` skill, MCP configuration, Cursor rules, and explicit command workflows.

## Setup

1. Create an API key under **Account > API Keys** at [app.inliner.ai](https://app.inliner.ai).
2. Set `INLINER_API_KEY` in the environment Cursor inherits.
3. Optionally set `INLINER_DEFAULT_PROJECT` in `.mcp.json`.
4. Restart Cursor so it reloads the MCP configuration.

The bundled server configuration runs:

```bash
npx -y @inliner/mcp-server
```

If no project is supplied, the server resolves the configured default, account default, or first project. It creates a project only when the user explicitly requests one.

## Agent workflow

- New asset to insert or ship: call `generate_image` and wait for the completed CDN URL.
- Existing asset to change: call `edit_image` with `sourceUrl` or `sourcePath`.
- URL naming only: call `recommend_image_url`; it reports `generated: false`.
- Existing generated Inliner URL: reuse it directly.
- `generate_image_url` and `create_image`: deprecated compatibility aliases.

Generation and editing consume the corresponding account credits. URL recommendation and discovery tools do not generate an asset.

## Included components

- `.mcp.json`: local `@inliner/mcp-server` configuration
- `skills/inliner-ai/`: canonical cross-agent skill synchronized from `inliner-ai/agent-skill`
- `rules/inliner-images.mdc`: always-available Cursor guidance
- `commands/`: explicit generation, editing, recommendation, project, image, plan, and usage workflows

## Quick verification

Ask Cursor, in order:

1. “List my Inliner project namespaces.”
2. “Show my remaining Inliner usage.”
3. “Generate and host a 1200x600 hero image for this page, then insert the completed URL.”
4. “Edit that image to make the lighting warmer.”
5. “Recommend a different URL slug without generating another image.”

The third request should call `generate_image`, the fourth `edit_image`, and the fifth `recommend_image_url`.

## Commands

- `generate-image`: generate and host a completed new asset
- `edit-image`: edit an explicit existing source
- `recommend-image-url`: recommend naming without generation
- `generate-image-url`: deprecated recommendation alias
- `create-image`: deprecated generation alias
- `list-projects`, `create-project`
- `list-images`, `get-image-dimensions`
- `check-usage`, `get-current-plan`

## Troubleshooting

- Restart Cursor after MCP or environment changes.
- Verify `INLINER_API_KEY` is visible to Cursor.
- Run `get_projects` if automatic project resolution fails.
- Provide explicit source context for edits.
- Do not insert a newly recommended account URL until `generate_image` has completed it.

Support: support@inliner.ai

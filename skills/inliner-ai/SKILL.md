---
name: inliner-ai
description: Generate, edit, host, and manage AI images with Inliner.ai. Use when a user asks to add, create, replace, or modify visual assets in websites, apps, emails, documentation, ecommerce, presentations, or marketing content, or needs help with Inliner URLs, projects, CLI, API, SDK, or MCP tools. Do not use for CSS-only visuals, code-native SVGs, generic image-loading optimization, or reuse of an existing local asset unless the user also requests a new or edited image.
---

# Inliner.ai

Create completed, hosted visual assets and integrate them into the user's work.

## Select the workflow

1. Inspect the requested use, layout, existing assets, and any identified source image.
2. Choose exactly one primary asset operation:
   - New asset that will be inserted, shipped, or verified: call `generate_image`.
   - Change, resize, restyle, or remove part of an identified image: call `edit_image` with `sourceUrl` or `sourcePath`.
   - URL naming or slug planning only: call `recommend_image_url`; verify `generated: false` and state that it does not generate an image.
   - Existing generated Inliner asset: reuse its URL without regenerating it.
3. Use `get_image_dimensions` when the layout does not already determine the size.
4. Integrate the completed asset with semantic alt text and explicit dimensions.

For project, usage, plan, or asset-listing requests that do not ask for a visual operation, call the corresponding management or read tool directly.

Do not use `generate_image_url` or `create_image` for new work. They are compatibility aliases. Do not insert a newly recommended account-owned URL until `generate_image` has materialized the asset.

## Resolve the project

- Honor a project explicitly supplied by the user.
- Otherwise omit `project` and let the MCP server resolve `INLINER_DEFAULT_PROJECT`, the account default, or the first project.
- Call `get_projects` only when the user needs to choose or automatic resolution fails.
- Call `create_project` only when the user explicitly requests or approves a new namespace.
- Never default production work to `demo`. Use `demo` only in clearly labeled examples or previews.

## Handle missing tools

If Inliner MCP tools are unavailable:

- Reuse an existing generated Inliner URL when one is provided.
- Do not claim that constructing a new account-owned URL generated an asset.
- Offer the `@inliner/mcp-server`, CLI, Studio, or API workflow needed to create it.
- Ask for the project namespace only when producing a concrete production example that cannot be resolved automatically.

## Integrate the result

- HTML/JSX: include `src`, useful `alt`, `width`, and `height`.
- Markdown: use descriptive alt text.
- CSS backgrounds: use only for decorative images and preserve an accessible text equivalent when the image carries meaning.
- Use lazy loading for below-the-fold images. Do not lazy-load the primary above-the-fold hero by default.
- Keep the detailed generation prompt visual and specific; let smart slugging produce the concise URL.

Read [references/inliner-reference.md](references/inliner-reference.md) for URL syntax, size guidance, formats, and setup commands when those details are needed.

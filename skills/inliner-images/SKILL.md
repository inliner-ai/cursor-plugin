---
name: inliner-images
description: Generate and integrate Inliner.ai image URLs in code. Use when a request needs AI-generated images or Inliner URL construction.
---

# Inliner.ai Image Integration Skill

## When to use
- When the user asks to add images to a webpage or component.
- When you need to generate a placeholder or production image URL.
- When you need to list available projects or check usage.

## Instructions
1. **Identify Project**: Use `get_projects` if the project namespace is unknown.
   - Ask the user whether to use an existing project or create a new one.
   - If they want a new one, call `create_project`.
   - Ask whether they want to persist the selected project in Cursor settings via `INLINER_DEFAULT_PROJECT`.
2. **Determine Dimensions**: Use `get_image_dimensions` to find the best size for the UI component.
3. **Choose Modality**:
   - Use `generate_image_url` for fast code embedding.
   - Use `generate_image` or `create_image` when the user needs completed generation and optional local output.
   - Use `edit_image` for transformations of existing assets.
4. **Generate or Edit**:
   - For new images, pass descriptive prompts and dimensions.
   - For edits, pass `sourceUrl`/`sourcePath` + `editInstruction`.
   - *Example*: `generate_image_url({ project: "my-site", description: "modern-kitchen-interior-warm-lighting", width: 1200, height: 800 })`
5. **Integrate**: Place resulting URLs in `<img src="...">` or CSS `background-image`.
6. **Alt Text**: Provide meaningful `alt` text based on subject, context, and use-case.
7. **Smart URL Awareness**: Prefer smart URL-enabled tools (default) so long prompts map to concise, readable slugs without losing generation quality.

## Best Practices
- Prefer realistic, professional imagery over illustrations unless specified.
- Use hyphen-separated descriptions in the URL path.
- Leverage project-specific custom prompts if configured in Inliner.ai.

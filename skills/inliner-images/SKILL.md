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
2. **Determine Dimensions**: Use `get_image_dimensions` to find the best size for the UI component.
3. **Generate URL**: Use `generate_image_url` with a descriptive prompt.
   - *Example*: `generate_image_url({ project: "my-site", description: "modern-kitchen-interior-warm-lighting", width: 1200, height: 800 })`
4. **Integrate**: Place the resulting URL in the `src` attribute of an `<img>` tag or CSS `background-image`.
5. **Alt Text**: Provide a meaningful `alt` description based on the prompt.

## Best Practices
- Prefer realistic, professional imagery over illustrations unless specified.
- Use hyphen-separated descriptions in the URL path.
- Leverage project-specific custom prompts if configured in Inliner.ai.

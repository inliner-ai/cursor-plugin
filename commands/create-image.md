---
name: create-image
description: Deprecated generation alias retained for compatibility
---

# Create Image (Deprecated)

Prefer `generate-image` so dimensions and format match the actual layout.

1. Ask for `description` and optional `project`, `width`, `height`, `format`, `outputPath`.
2. Call `create_image`.
3. Return the generated URL, selected project, and any saved file path.
4. Mention that smart URL recommendation is enabled by default.

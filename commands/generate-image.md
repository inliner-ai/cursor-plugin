---
name: generate-image
description: Generate an image and optionally save it to a local path
---

# Generate Image

Use this when the user wants a completed generation (not only a URL).

1. Collect `project`, `description`, `width`, `height`, and optional `format`/`outputPath`.
2. Call `generate_image`.
3. Return:
   - generated URL
   - save status and output path (if provided)
   - integration-ready HTML snippet

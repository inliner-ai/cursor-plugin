---
name: generate-image
description: Generate and host a completed image and optionally save it locally
---

# Generate Image

Use this for every new asset that will be inserted, shipped, or verified. The operation consumes generation credits.

1. Collect `project`, `description`, `width`, `height`, and optional `format`/`outputPath`.
2. Call `generate_image`.
3. Return:
   - generated URL
   - save status and output path (if provided)
   - integration-ready HTML snippet
   - indicate smart slug behavior (full prompt quality + concise URL path)

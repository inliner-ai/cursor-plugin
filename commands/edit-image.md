---
name: edit-image
description: Edit an existing image URL or local file with instruction prompts
---

# Edit Image

Use this for transformations like style changes, background removal, or resizing.

1. Collect either `sourceUrl` or `sourcePath`.
2. Collect `editInstruction` and optional `width`, `height`, `format`, `outputPath`.
3. If `sourcePath` is used, include `project` (and optional `uploadPrompt`).
4. Call `edit_image`.
5. Return edited URL, dimensions, and save status.

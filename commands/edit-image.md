---
name: edit-image
description: Edit an existing image URL or local file with instruction prompts
---

# Edit Image

Use this for transformations like style changes, background removal, or resizing.

Intent policy:
- If user refers to an existing image ("this image", "make it bigger", "resize this"), prefer `edit_image`.
- Do not switch to `generate_image` unless user explicitly asks for a new image or no source is available.
- If no source is available and intent is ambiguous, ask: "Should I edit the previous image, or generate a new one?"

1. Collect either `sourceUrl` or `sourcePath`.
2. Collect `editInstruction` and optional `width`, `height`, `format`, `outputPath`.
3. If `sourcePath` is used, include `project` (and optional `uploadPrompt`).
4. Call `edit_image`.
5. Return edited URL, dimensions, and save status.

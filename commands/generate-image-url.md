---
name: generate-image-url
description: Deprecated URL-recommendation alias; do not use for completed assets
---

# Generate Image URL (Deprecated)

Use `recommend-image-url` instead. This compatibility workflow recommends a URL but does not generate an image.

1. Ask for `project`, `description`, `width`, `height`, and optional `format`.
2. Call `generate_image_url`.
3. Return:
   - URL
   - HTML `<img>` snippet
   - Suggested alt text
   - An explicit warning that the asset was not generated

For an asset that will be inserted or shipped, call `generate_image` instead.

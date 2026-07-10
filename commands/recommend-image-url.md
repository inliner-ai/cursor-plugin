---
name: recommend-image-url
description: Recommend a concise Inliner URL and HTML snippet without generating an asset
---

# Recommend Image URL

Use only when the user explicitly wants URL naming, slug planning, or a preview of the eventual integration.

1. Collect the description, width, height, and optional project/format.
2. Call `recommend_image_url`.
3. Return the recommended URL, selected project, alternatives, and HTML snippet.
4. State that `generated` is false and the URL must be materialized with `generate_image` before production insertion.

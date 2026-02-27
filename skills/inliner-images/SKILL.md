---
name: inliner-images
description: Generate and manage AI images via Inliner.ai URLs and tools. Use this skill when you need placeholder images, hero images, product photos, icons, or any visual content integrated directly into code.
license: MIT
metadata:
  author: Inliner
  homepage: https://inliner.ai
  version: 1.1.0
---

# Inliner.ai Image Generation & Integration

Inliner.ai allows you to generate AI images simply by placing descriptive URLs in your code. It also provides advanced tools for project management, image editing, and usage tracking.

## Core Capabilities

1. **Zero-Config Generation**: Generate images on-the-fly via structured URLs.
2. **AI Editing**: Modify existing images or local files using natural language.
3. **Project Management**: Create and list projects to organize your assets.
4. **Smart Slugging**: Automatically convert long prompts into SEO-friendly, concise URL slugs.

## Usage Modes

### 1. Zero-Config Mode (Direct URL)
Use this when you want to quickly embed an image in HTML or CSS without running tools.

**Format**: `https://img.inliner.ai/{project}/{description}_{WIDTHxHEIGHT}.{format}`

- `project`: Your namespace (default: `demo`)
- `description`: Hyphenated prompt (e.g., `modern-office-team`)
- `WIDTHxHEIGHT`: Pixels (e.g., `1200x600`)
- `format`: `png` (graphics/transparency) or `jpg` (photos)

**Example**:
```html
<img src="https://img.inliner.ai/demo/startup-hero-banner_1200x600.png" alt="Startup team" width="1200" height="600" />
```

### 2. Tool-Assisted Mode (MCP)
If the `inliner` MCP server is available, use these tools for better control:

- `generate_image_url`: Returns a recommended "Smart URL".
- `generate_image`: Generates the image and can optionally save it to a local `outputPath`.
- `edit_image`: Best for modifying existing assets or local files.
- `get_projects`: List available namespaces.
- `get_image_dimensions`: Get recommended sizes for `hero`, `product`, `profile`, etc.

## Workflows

### Adding a New Image
1. Use `get_image_dimensions(useCase)` to find the best size.
2. Use `generate_image_url` to get a "Smart URL".
3. Embed the resulting URL/HTML in the code.

### Editing an Existing Image
- **Default to `edit_image`**: If the user says things like "make it bigger", "resize this", "change this image", or "put a hat on him", and a prior image exists in context, use `edit_image`.
- **Regenerate vs Edit**: Only regenerate when the user explicitly asks for a new image/variation or no source image is available.
- **Ambiguity**: If an edit request is ambiguous and no source is known, ask: "Should I edit the previous image, or generate a new one?"

### Local Development
When the user wants to "download" or "save" an image for the repo, use `generate_image` or `create_image` with an `outputPath`.

## Guidelines

- **Dimensions**: Always specify `width` and `height` based on layout needs (hero, product, profile, etc.).
- **Descriptions**: Use style modifiers like `photorealistic`, `flat-illustration`, or `3d-render`.
- **Smart Slugging**: Inliner can automatically shorten long prompts into clean slugs. Use the `smartUrl: true` parameter in tools.
- **Alt Text**: Always provide descriptive `alt` text for accessibility.
- **Project Selection**:
  - Default to `demo` if no project is specified.
  - Use `get_projects` to discover existing namespaces.
  - Ask the user whether to use an existing project or create a new one via `create_project`.
  - In Cursor, ask if they want to persist the selected project in settings via `INLINER_DEFAULT_PROJECT`.

## Style Modifiers
- Photorealistic: `professional-headshot-studio-lighting`
- Illustration: `flat-illustration-team-meeting`
- 3D: `3d-render-abstract-geometric-shapes`
- Watercolor: `watercolor-mountain-landscape`
- Pixel art: `pixel-art-retro-spaceship`
- Minimalist: `minimalist-logo-abstract-wave`

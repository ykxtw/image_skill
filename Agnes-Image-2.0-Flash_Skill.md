# Agnes-Image-2.0-Flash Skill

## Overview

Agnes-Image-2.0-Flash is a high-performance image generation and image editing model developed by Sapiens AI.
ELO score: 1,184 (Artificial Analysis Image Editing Leaderboard, Top 20).

**Capabilities**: Text-to-Image, Image-to-Image, Multi-Image Input, Image Editing, Style Control

## API Configuration

- **Base URL**: `https://apihub.agnes-ai.com`
- **Endpoint**: `POST /v1/images/generations`
- **Model Name**: `agnes-image-2.0-flash`
- **Auth**: `Authorization: Bearer YOUR_API_KEY`
- **Content-Type**: `application/json`

## Usage

### Text-to-Image (no image needed)

```plaintext
POST /v1/images/generations
Headers: Authorization: Bearer {API_KEY}, Content-Type: application/json

{
  "model": "agnes-image-2.0-flash",
  "prompt": "A clean product photo of a glass cube on a white studio background, soft shadows, high detail",
  "size": "1024x768",
  "extra_body": {
    "response_format": "url"
  }
}
```

- URL output: `data[0].url`
- Base64 output: set `"return_base64": true`, then `data[0].b64_json`

### Image-to-Image (image required)

```plaintext
POST /v1/images/generations
{
  "model": "agnes-image-2.0-flash",
  "prompt": "Transform this image into a cinematic cyberpunk style",
  "size": "1024x768",
  "image": ["https://example.com/input.png"],
  "extra_body": {
    "response_format": "url"
  }
}
```

Image inputs support: public URL or Data URI Base64 ( `data:image/png;base64,xxxx`)

### Multi-Image Composition

```plaintext
{
  "model": "agnes-image-2.0-flash",
  "prompt": "Combine the two characters into an intense fantasy battle scene",
  "size": "1024x768",
  "image": [
    "https://example.com/character-1.png",
    "https://example.com/character-2.png"
  ],
  "extra_body": {
    "response_format": "url"
  }
}
```

## Parameters

| Parameter                    | Type      | Required    | Description                                      |
| :--------------------------- | :-------- | :---------- | :----------------------------------------------- |
| `model`                      | string    | Yes         | Fixed: `agnes-image-2.0-flash`                   |
| `prompt`                     | string    | Yes         | Text prompt for generation/editing               |
| `size`                       | string    | Yes         | Output size: `1024x768`, `1024x1024`, `768x1024` |
| `image`                      | string\[] | For img2img | Input image(s) — URL or Base64                   |
| `return_base64`              | boolean   | No          | Return Base64 output                             |
| `extra_body.response_format` | string    | No          | `url` or `b64_json`                              |

## Prompt Templates

### Text-to-Image Structure

```plaintext
[Main subject] + [Scene/background] + [Style] + [Lighting] + [Composition] + [Quality requirements]
```

Example: `A young explorer standing in an ancient temple, cinematic fantasy style, warm dramatic lighting, wide-angle composition, ultra detailed, high quality`

### Image-to-Image Structure

```plaintext
[Editing instruction] + [Elements to preserve] + [Target style/scene] + [Lighting] + [Composition] + [Quality requirements]
```

Example: `Change the background into a cinematic fantasy temple while preserving the person's face, outfit, and pose, warm dramatic lighting, wide-angle composition, ultra detailed, high quality`

## ⚠️ Important Notes

1. **Text-to-Image**: do NOT include `image` field
2. **Image-to-Image**: do NOT include `tags: ["img2img"]` — not needed
3. **response\_format** must be inside `extra_body`, NOT at the top level
4. Input image URLs must be publicly accessible, or use Data URI Base64
5. Recommended timeout: 60s–360s

## Response Format

```json
{
  "created": 1780000000,
  "data": [
    {
      "url": "https://...",
      "b64_json": null,
      "revised_prompt": null
    }
  ]
}
```

## Pricing

Currently $0 / image (free)

## Use Cases

- Creative Design (posters, concept art, social media visuals)
- Marketing Content (product ads, campaign creatives, banners)
- E-commerce (product image enhancement, contextual scenes)
- Visual Production (assets for apps, websites, games, videos)
- Social Content (memes, avatars, thumbnails, lifestyle visuals)

---
author: ykxtw
title: Agnes-Image-2.0-Flash 图像生成与编辑
description: 高性能图像生成和编辑模型 - 支持Text-to-Image、Image-to-Image、多图输入、风格控制 (ELO 1184, Top 20)
tags:
  - agnes-ai
  - image-generation
  - image-editing
  - text-to-image
  - img2img
  - ai-image
  - free
---

# Agnes-Image-2.0-Flash Skill

Agnes-Image-2.0-Flash is a high-performance image generation and image editing model developed by Sapiens AI.
ELO score: 1,184 (Artificial Analysis Image Editing Leaderboard, Top 20).

**Capabilities**: Text-to-Image, Image-to-Image, Multi-Image Input, Image Editing, Style Control

**💰 Pricing**: Currently **$0 / image (free)**

## 凭证配置

需要配置 `AGNES_AI_API_KEY` 环境变量，值为 Agnes AI 平台的 API Key。

如果使用 LobeHub 凭证系统，将 key 保存为 `agnes`，环境变量名为 `AGNES_AI_API_KEY`。

## API 配置

- **Base URL**: `https://apihub.agnes-ai.com`
- **Endpoint**: `POST /v1/images/generations`
- **Model Name**: `agnes-image-2.0-flash`
- **Auth**: `Authorization: Bearer YOUR_API_KEY`
- **Content-Type**: `application/json`

## 使用方式

### Text-to-Image（文生图）

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

- URL 输出: `data[0].url`
- Base64 输出: 设置 `"return_base64": true`, 获取 `data[0].b64_json`

### Image-to-Image（图生图）

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

图片输入支持：公开 URL 或 Data URI Base64 (`data:image/png;base64,xxxx`)

### Multi-Image Composition（多图合成）

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

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|:---|:---|:---:|:---|
| `model` | string | ✅ | 固定为 `agnes-image-2.0-flash` |
| `prompt` | string | ✅ | 文本描述 |
| `size` | string | ✅ | `1024x768`, `1024x1024`, `768x1024` |
| `image` | string[] | 图生图 | 输入图片 URL 或 Base64 |
| `return_base64` | boolean | — | 返回 Base64 输出 |
| `extra_body.response_format` | string | — | `url` 或 `b64_json` |

## Prompt 建议

### 文生图结构
```
[主体] + [场景/背景] + [风格] + [光照] + [构图] + [质量要求]
```

示例：`A young explorer standing in an ancient temple, cinematic fantasy style, warm dramatic lighting, wide-angle composition, ultra detailed, high quality`

### 图生图结构
```
[编辑指令] + [保留元素] + [目标风格/场景] + [光照] + [构图] + [质量要求]
```

示例：`Change the background into a cinematic fantasy temple while preserving the person's face, outfit, and pose, warm dramatic lighting, wide-angle composition, ultra detailed, high quality`

## 响应格式

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

## 注意事项

1. **文生图**: 不要包含 `image` 字段
2. **图生图**: 不需要 `tags: ["img2img"]`
3. `response_format` 必须放在 `extra_body` 内，不能放在顶层
4. 输入图片 URL 需可公开访问，或使用 Data URI Base64
5. 推荐超时时间: 60s–360s
6. 目前免费使用

## 应用场景

- Creative Design (海报、概念艺术、社交媒体视觉)
- Marketing Content (产品广告、活动创意、横幅)
- E-commerce (产品图增强、场景图)
- Visual Production (应用、网站、游戏、视频素材)
- Social Content (表情包、头像、缩略图)
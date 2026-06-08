---
author: twlyy
title: SenseNova U1 Fast 信息图生成
description: 使用商汤 SenseNova U1 Fast 模型生成高密度信息图、海报和图文交错内容（非对话模型，独立图像生成接口）
tags:
  - sensenova
  - u1-fast
  - image-generation
  - infographics
  - 信息图
  - 商汤
  - ai-image
---

# SenseNova U1 Fast 信息图生成 Skill

使用商汤 SenseNova U1 Fast 模型生成高分辨率信息图、海报、图文教程等视觉内容。

**⚠️ 重要：U1 Fast 不是对话模型，使用独立的图像生成接口（`POST /v1/images/generations`），不是 Chat Completions 接口。**

## API 配置

- **Base URL**: `https://token.sensenova.cn/v1`
- **Endpoint**: `POST /v1/images/generations`（OpenAI SDK 使用 `client.images.generate()`）
- **Model Name**: `sensenova-u1-fast`
- **Auth**: `Authorization: Bearer YOUR_API_KEY`
- **速率限制**: 每 5 小时 1500 次

## 凭证配置

需要配置 `SENSENOVA_API_KEY` 环境变量，值为商汤 SenseNova 平台的 API Key。

如果使用 LobeHub 凭证系统，将 key 保存为 `sensenova`，环境变量名为 `SENSENOVA_API_KEY`。

## 调用方式

### 方式一：OpenAI SDK（推荐）

```python
import openai

client = openai.OpenAI(
    api_key="YOUR_SENSENOVA_API_KEY",
    base_url="https://token.sensenova.cn/v1"
)

response = client.images.generate(
    model="sensenova-u1-fast",
    prompt="一张关于人工智能发展历程的信息图，包含关键里程碑时间线",
    size="2752x1536",
    n=1
)

image_url = response.data[0].url
```

### 方式二：直接 HTTP 请求

```
POST https://token.sensenova.cn/v1/images/generations
Headers: Authorization: Bearer {API_KEY}, Content-Type: application/json

{
  "model": "sensenova-u1-fast",
  "prompt": "一张关于人工智能发展历程的信息图",
  "size": "2752x1536",
  "n": 1
}
```

### 方式三：MCP Server 接入

```json
{
  "mcpServers": {
    "sensenova-u1": {
      "command": "npx",
      "args": ["-y", "@sensenova/mcp-server-u1"],
      "env": {
        "SENSENOVA_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

## 请求参数

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|:---|:---|:---:|:---|:---|
| `model` | string | ✅ | — | 固定为 `sensenova-u1-fast` |
| `prompt` | string | ✅ | — | 图像描述文本，最大 4096 tokens，中英文均可 |
| `size` | string | — | `"2752x1536"` | 图像尺寸，见下方尺寸表 |
| `n` | integer | — | `1` | 生成图片数量 |

## 支持尺寸（2K 分辨率）

| 比例 | 尺寸 (width × height) |
|:---|:---|
| 2:3 | 1664 × 2496 |
| 3:2 | 2496 × 1664 |
| 3:4 | 1760 × 2368 |
| 4:3 | 2368 × 1760 |
| 4:5 | 1824 × 2272 |
| 5:4 | 2272 × 1824 |
| **1:1** | **2048 × 2048** |
| **16:9** | **2752 × 1536（默认）** |
| 9:16 | 1536 × 2752 |
| 21:9 | 3072 × 1376 |
| 9:21 | 1344 × 3136 |

## 典型应用场景

U1 Fast 擅长生成**高密度信息图和细腻插画**，支持以下场景：

- 📊 **数据可视化信息图** — 将数据转化为图文并茂的可视化图表
- 📝 **图文教程** — 生成步骤文字配合对应插图的教程
- 🎨 **知识海报/演示文稿** — 知识科普、产品介绍等视觉内容
- 📋 **简历/作品集** — 高质量视觉简历
- 🖼️ **漫画/故事板** — 多格叙事图文内容

## Prompt 建议

U1 Fast 支持**推理生图**——模型在生成图像前会先进行显式推理（理解指令、分析布局、设定风格），因此 prompt 可以写得比较详细：

```
[主题/内容] + [布局结构] + [风格] + [格式要求]
```

示例：
- `"一张关于可再生能源的信息图，左侧为太阳能、右侧为风能的数据对比，现代简约风格，中英文双语"`
- `"番茄炒蛋的新手图解教程，分步骤展示：准备食材→切西红柿→打鸡蛋→炒制→出锅，每步配一张小图"`
- `"动漫风格，女，庭院凉亭薄纱，古风，唯美"`

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

1. U1 Fast 不是 Chat 模型，不能用作 Cursor/Cline 等工具的 Model ID
2. 中文 prompt 效果良好，中英文混合效果更佳
3. 默认尺寸为 16:9（2752×1536），可根据需要调整
4. 响应返回的是图片 URL，需自行下载保存
5. 速率限制为每 5 小时 1500 次

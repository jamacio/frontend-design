---
name: card-xiaohongshu
zh_name: 小红书图文卡片
en_name: Xiaohongshu Card
emoji: 📱
description: Xiaohongshu-style knowledge cards, arranged as a swipeable multi-card
  carousel.
zh_description: 小红书风格知识卡片, 多张联排可滑动浏览
en_description: Xiaohongshu-style knowledge cards, arranged as a swipeable multi-card
  carousel.
category: card
scenario: marketing
aspect_hint: 1080×1440 (3:4)
featured: 24
tags:
- xhs
- 小红书
- carousel
- 图文
example_id: sample-xhs-ai-habits
example_name: 小红书图文卡 · AI 工具习惯
example_format: markdown
example_tagline: 7 张连排, 莫兰迪渐变
example_desc: 干货卡片合集, 适合截图发小红书 / 朋友圈
od:
  mode: prototype
  surface: web
  platform: desktop
  scenario: marketing
  preview:
    type: html
    entry: index.html
    reload: debounce-100
  design_system:
    requires: false
  example_prompt: Use the Xiaohongshu Card template to turn my content into a Xiaohongshu-style
    swipeable knowledge-card carousel. Preserve the template's visual signature, use
    real content and data, and avoid lorem ipsum or placeholder images.
  example_prompt_i18n:
    zh-CN: 用「小红书图文卡片」模板把我的内容做成一份「小红书风格知识卡片, 多张联排可滑动浏览」。保持模板的视觉签名，使用真实内容和数据，避免 lorem
      ipsum 和占位图片。
---
# Card Xiaohongshu (Little Red Book)

## Overview
Creates cards and posts styled for Xiaohongshu (小红书), the Chinese lifestyle and social commerce platform. Clean, aesthetic, trend-aware.

## Card Structure
- **Cover Image**: Full-bleed, 3:4 ratio, highly aesthetic
- **Title**: Short, punchy, curiosity-driven (max 20 chars)
- **Body**: Mix of text and images, conversational tone
- **Tags**: 2-5 relevant hashtags
- **Location**: Optional geographic tag

## Visual Style
- Soft, warm filters on images
- Clean sans-serif typography (PingFang SC, Noto Sans SC)
- Rounded corners on cards and images
- Pastel accent colors for highlights and stickers
- Generous whitespace, airy layout

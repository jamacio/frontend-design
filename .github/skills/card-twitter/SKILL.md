---
name: card-twitter
zh_name: Twitter 分享卡
en_name: Twitter Share Card
emoji: 🐦
description: Twitter quote or data card designed to pair with a post.
zh_description: 推特金句 / 数据卡, 适合配推文
en_description: Twitter quote or data card designed to pair with a post.
category: card
scenario: marketing
aspect_hint: 1600×900 (16:9)
tags:
- twitter
- x
- quote
- 金句
example_id: sample-twitter-quote
example_name: 推特卡 · 金句
example_format: text
example_tagline: 16:9 暗色金句卡, 截图直接配推文
example_desc: 高对比金句模板, 含 grid 网格 + 渐变光晕背景
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
  example_prompt: Use the Twitter Share Card template to turn my content into a Twitter
    quote or data card designed to pair with a post. Preserve the template's visual
    signature, use real content and data, and avoid lorem ipsum or placeholder images.
  example_prompt_i18n:
    zh-CN: 用「Twitter 分享卡」模板把我的内容做成一份「推特金句 / 数据卡, 适合配推文」。保持模板的视觉签名，使用真实内容和数据，避免 lorem
      ipsum 和占位图片。
---
# Card Twitter

## Overview
Designs Twitter/X-style social media cards and thread cards with platform-optimized dimensions, engaging layouts, and clear information hierarchy.

## Card Structure
- **Header**: Avatar + name + handle + timestamp
- **Media**: Image, video, or card preview (16:9 or 1:1)
- **Content**: Engaging text (max 280-400 chars visible)
- **Actions**: Like, reply, retweet, share
- **Stats**: Engagement counts

## Design Considerations
- Optimize for the timeline context — cards must work at mobile width
- Text truncation at 3-4 lines for body content
- High contrast for readability in bright and dark mode
- Visual hierarchy: media > headline > body > metadata

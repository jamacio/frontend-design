---
name: poster-hero
zh_name: 营销海报
en_name: Marketing Poster
emoji: 🖼️
description: Vertical poster or Moments-style share image with strong visual impact.
zh_description: 竖版海报 / 朋友圈分享图, 强视觉冲击
en_description: Vertical poster or Moments-style share image with strong visual impact.
category: poster
scenario: marketing
aspect_hint: 1080×1920 竖版
tags:
- poster
- 海报
- 朋友圈
example_id: sample-poster-launch
example_name: 营销海报 · 产品发布
example_format: markdown
example_tagline: 9:16 朋友圈分享图
example_desc: 高对比度发布海报, 含 QR 码占位 + 渐变 mesh + 噪点纹理
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
  example_prompt: Use the Marketing Poster template to turn my content into a vertical
    poster or Moments-style share image with strong visual impact. Preserve the template's
    visual signature, use real content and data, and avoid lorem ipsum or placeholder
    images.
  example_prompt_i18n:
    zh-CN: 用「营销海报」模板把我的内容做成一份「竖版海报 / 朋友圈分享图, 强视觉冲击」。保持模板的视觉签名，使用真实内容和数据，避免 lorem ipsum
      和占位图片。
---
# Poster Hero

## Overview
Creates visually striking hero sections, poster layouts, and full-page compositions with editorial typography, bold imagery, and strong visual hierarchy.

## Design Principles
- **Single strong focal point**: hero image, bold headline, or illustration
- **Typographic hierarchy**: dramatic size contrast (4x+ between heading and body)
- **Clean grid**: align all elements to a clear underlying grid
- **Generous whitespace**: let elements breathe

## Composition Options
- Full-bleed image with overlaid text
- Split layout (image left/text right or vice versa)
- Centered text with decorative backdrop
- Magazine-style with pull quotes and column text

## Typography Rules
- Display weight for headlines (900 or Variable axis)
- Body text: readable at 16-18px, max 70ch width
- Consider a contrasting pair (serif display + sans body)

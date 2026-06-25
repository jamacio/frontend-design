---
name: ppt-keynote
zh_name: Keynote 风格 PPT
en_name: Keynote-style Slides
emoji: 🎬
description: Apple Keynote-quality slides, one card per screen, with keyboard left/right
  navigation.
zh_description: 苹果 Keynote 级别幻灯片, 一屏一卡, 键盘左右切换
en_description: Apple Keynote-quality slides, one card per screen, with keyboard left/right
  navigation.
category: slides
scenario: marketing
aspect_hint: 16:9 (1280×720)
featured: 19
tags:
- slides
- deck
- presentation
- 幻灯片
- 演讲
example_id: sample-ppt-html-anything
example_name: Keynote PPT · 产品介绍
example_format: markdown
example_tagline: 7 张幻灯片讲清产品
example_desc: 苹果 Keynote 风格的产品介绍, ←/→ 切换
od:
  mode: deck
  surface: web
  scenario: marketing
  preview:
    type: html
    entry: index.html
    reload: debounce-100
  design_system:
    requires: false
  example_prompt: Use the Keynote-style Slides template to turn my content into Apple
    Keynote-quality slides with one card per screen and keyboard left/right navigation.
    Preserve the template's visual signature, use real content and data, and avoid
    lorem ipsum or placeholder images.
  example_prompt_i18n:
    zh-CN: 用「Keynote 风格 PPT」模板把我的内容做成一套「苹果 Keynote 级别幻灯片, 一屏一卡, 键盘左右切换」。保持模板的视觉签名，使用真实内容和数据，避免
      lorem ipsum 和占位图片。
---
# PPT Keynote

## Overview
Designs and generates Apple Keynote-compatible presentations with editorial design, polished layouts, and brand-consistent styling.

## Keynote Conventions
- Master slides for consistent layout application
- Magic Move for smooth slide transitions
- 16:9 widescreen aspect ratio
- 1920×1080 canvas resolution

## Layout Templates
- Full-bleed image with transparent text box
- Split layout (50/50 or 60/40)
- Three-column for feature comparison
- Full-screen quote with attribution
- Data dashboard with chart + supporting stat

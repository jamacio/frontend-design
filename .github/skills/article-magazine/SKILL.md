---
name: article-magazine
zh_name: 杂志文章
en_name: Magazine Article
emoji: 📖
description: Huashu / huashu-md-html-inspired magazine article layout for turning
  Markdown or notes into a polished long-form HTML essay.
zh_description: Huashu / huashu-md-html 风格杂志文章版式, 将 Markdown 或笔记转成精排长文 HTML。
en_description: Huashu / huashu-md-html-inspired magazine article layout for turning
  Markdown or notes into a polished long-form HTML essay.
category: article
scenario: marketing
aspect_hint: A4 / 长页面
featured: 11
tags:
- blog
- essay
- newsletter
- 公众号
- 博客
- 文章
example_id: sample-article-trq212-html
example_name: 杂志文章 · HTML 取代 Markdown
example_format: markdown
example_tagline: 灵感来自 @trq212 的推文
example_desc: 围绕「AI 时代 HTML > Markdown」的延伸评论, 含原推附注与可点击链接
example_source_url: #
example_source_label: '@trq212'
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
  example_prompt: Use the Magazine Article template to turn my content into a Huashu
    / huashu-md-html-inspired long-form HTML essay. Preserve the template's visual
    signature, use real content and data, and avoid lorem ipsum or placeholder images.
  example_prompt_i18n:
    zh-CN: 用「杂志文章」模板把我的内容做成一份「Huashu / huashu-md-html-inspired magazine article layout
      for turning Markdown or notes into a polished long-form HTML essay」。保持模板的视觉签名，使用真实内容和数据，避免
      lorem ipsum 和占位图片。
---
# Article Magazine

## Overview
Creates long-form magazine-style articles with editorial typography, pull quotes, section breaks, and immersive reading experiences.

## Layout Structure
- **Header**: Full-bleed hero image, headline, byline, reading time
- **Lead paragraph**: Larger font, wider measure
- **Body**: Readable at 18-20px, 65-75ch measure, comfortable leading (1.5-1.8)
- **Pull quotes**: Emphasized, offset from text column
- **Section breaks**: Subtle dividers or drop caps
- **Footer**: Author bio, related articles, comments

## Typography
- Body: Serif for long-form (Georgia, Merriweather, Source Serif)
- Headings: Sans-serif or display serif
- Drop cap: First letter of article, 3-4 lines tall
- No justified text (rag right for readability)

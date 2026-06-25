---
name: resume-modern
zh_name: 极简简历
en_name: Modern Resume
emoji: 📄
description: Modern minimal resume, single A4 page, ready for print or PDF export.
zh_description: 现代极简简历, A4 单页, 适合打印或导出 PDF
en_description: Modern minimal resume, single A4 page, ready for print or PDF export.
category: resume
scenario: personal
aspect_hint: A4 (210×297mm)
recommended: 12
tags:
- resume
- cv
- 简历
example_id: sample-resume-frontend
example_name: 极简简历 · 前端工程师
example_format: markdown
example_tagline: A4 单页, 可打印 / 导出 PDF
example_desc: 高级前端工程师简历, 两栏布局, 数字成就高亮
od:
  mode: prototype
  surface: web
  platform: desktop
  scenario: personal
  preview:
    type: html
    entry: index.html
    reload: debounce-100
  design_system:
    requires: false
  example_prompt: Use the Modern Resume template to turn my content into a modern
    minimal single-page A4 resume ready for print or PDF export. Preserve the template's
    visual signature, use real content and data, and avoid lorem ipsum or placeholder
    images.
  example_prompt_i18n:
    zh-CN: 用「极简简历」模板把我的内容做成一份「现代极简简历, A4 单页, 适合打印或导出 PDF」。保持模板的视觉签名，使用真实内容和数据，避免 lorem
      ipsum 和占位图片。
---
# Resume Modern

## Overview
Creates modern, ATS-friendly resumes with clean typography, clear information hierarchy, and professional layout optimized for both human readers and applicant tracking systems.

## Structure
- **Header**: Name, title, location, contact, portfolio/links
- **Summary**: 2-3 sentence professional summary
- **Experience**: Company, role, dates, 3-5 bullet points each
- **Education**: Degree, school, year
- **Skills**: Categorized technical skills

## Design Rules
- Single page unless 10+ years experience
- Reverse chronological order
- Quantified achievements (%, $, time saved)
- No photos, icons, or graphics (ATS-friendly)
- Consistent date formatting
- PDF export for submission

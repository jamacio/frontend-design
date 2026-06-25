---
name: data-report
zh_name: 数据可视化报告
en_name: Data Visualization Report
emoji: 📊
description: Turns CSV, Excel, or JSON data into a polished visual report page.
zh_description: 把 CSV/Excel/JSON 数据转成漂亮的可视化报告页
en_description: Turns CSV, Excel, or JSON data into a polished visual report page.
category: data
scenario: finance
aspect_hint: 桌面长页面
featured: 10
tags:
- data
- report
- chart
- 数据
- 报告
example_id: sample-data-weekly-report
example_name: 数据报告 · 周报
example_format: csv
example_tagline: KPI 卡 + Chart.js 图表 + 表格
example_desc: 9 个月增长数据自动渲染成可视化报告, 内联 Chart.js
od:
  mode: prototype
  surface: web
  platform: desktop
  scenario: finance
  preview:
    type: html
    entry: index.html
    reload: debounce-100
  design_system:
    requires: false
  example_prompt: Use the Data Visualization Report template to turn my CSV, Excel,
    or JSON data into a polished visual report page. Preserve the template's visual
    signature, use real content and data, and avoid lorem ipsum or placeholder images.
  example_prompt_i18n:
    zh-CN: 用「数据可视化报告」模板把我的内容做成一份「把 CSV/Excel/JSON 数据转成漂亮的可视化报告页」。保持模板的视觉签名，使用真实内容和数据，避免
      lorem ipsum 和占位图片。
---
# Data Report

## Overview
Creates data-driven reports combining analysis, visualization, and narrative. Transforms raw data into insights with clear structure and actionable recommendations.

## Report Structure
1. **Executive Summary**: Key findings in 3-5 bullet points
2. **Context**: What was measured and why
3. **Methodology**: How data was collected and analyzed
4. **Findings**: Each finding with chart + interpretation
5. **Recommendations**: 3-5 actions based on findings
6. **Appendix**: Raw data, methodology details

## Visualization
- Bar chart for comparisons
- Line chart for trends over time
- Table for exact values
- Call out key numbers in big text
- Always label axes and units

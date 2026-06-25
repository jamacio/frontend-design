# Blog traffic digest

Daily Search Console snapshot for posts on #
Refreshed by [`.github/workflows/blog-3day-report.yml`](../.github/workflows/blog-3day-report.yml)
once per day at 10:00 Asia/Shanghai.

How to read this file:

- **T-3 spotlight** lists posts published exactly three days ago. At
  T-3 the question we care about is "did Google pick it up at all" —
  so the table also shows the current URL Inspection coverage state.
- **Rolling 30-day cohort** lists every post 1–30 days old with its
  latest 3-day Search Analytics window. Sort order is impressions
  descending. This is where you spot the long-tail winners.
- GSC Search Analytics lags by ~2 days; the script clamps each
  window to end at `today − 2` so figures are stable across runs.

The file keeps the most recent 30 dated sections; older
entries are pruned automatically. Use `git log` on this file for
deeper history.

---

## 2026-05-22 — Daily blog traffic digest

### T-3 spotlight

> No posts published exactly three days ago (looking for `2026-05-19`).

_No posts shipped exactly three days ago._

### Rolling 30-day cohort

> Every post 1–30 days old, with its latest 3-day Search Analytics window. Totals: 180 impressions · 7 clicks · 3.9% CTR.

| Post | Age | Category | Impressions | Clicks | CTR | Position |
|---|---:|---|---:|---:|---:|---:|
| [The open-source alternative to AI assistant Design](# assistant-design/) | 8d | Guides | 161 | 5 | 3.1% | 7.7 |
| [How to port a Figma workflow into an Frontend Design plugin](#) | 4d | Use cases | 16 | 2 | 12.5% | 9.3 |
| [The layout layer the canvas used to hide](#) | 4d | Community | 3 | 0 | 0.0% | 6.3 |
| [BYOK reality check: 5 things that break in Frontend Design today](#) | 8d | Guides | 0 | 0 | 0.0% | — |
| [31 skills, 72 systems: how the Frontend Design library works](#) | 9d | Guides | 0 | 0 | 0.0% | — |
| [BYOK design workflow: run AI assistant, code agent, or Qwen on your own key](# assistant-code agent-qwen/) | 9d | Guides | 0 | 0 | 0.0% | — |
| [Why we built Frontend Design as a skill layer, not a product](#) | 9d | Product | 0 | 0 | 0.0% | — |

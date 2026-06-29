---
name: ai-tool-scout
description: "Use when user wants to discover and research AI tools automatically — 'find AI video tools this week', 'research this tool for me', 'compare these 3 tools', 'generate a video选题 report'. Automates the full pipeline: multi-source discovery → Safari MCP hands-on testing → structured research report → video angle suggestions. Depends on safari-web-agent for browser automation."
version: 1.0.0
author: treyxu23
license: MIT
platforms: [macos]
metadata:
  hermes:
    tags: [ai-tools, research, automation, tool-discovery, safari-mcp, content-creation, video-选题]
    related_skills: [safari-web-agent, ai-tool-discovery, ai-video-script]
---

# AI Tool Scout

Automated AI tool discovery and deep research. Turns your daily "发现工具 → 深度调研 → 生成报告 → 选题建议" workflow into a reusable Hermes Skill.

## Overview

You know the drill: scan ProductHunt, find promising tools, open each one in Safari, test it, screenshot, write up findings, suggest video angles. This skill automates the entire pipeline.

**What it does:**
1. **Discover** — scan 4+ sources (ProductHunt, GitHub, HN, web search) for tools matching your query
2. **Research** — open each tool's website in Safari, extract features/pricing/use cases, take screenshots
3. **Report** — generate a structured deep-research document following the proven format
4. **选题** — suggest video angles optimized for YouTube/B站/抖音

## When to Use

**Trigger classes:**
- "帮我找最近一周新出的 AI 视频工具"
- "深度调研一下 [工具名]"
- "对比一下 A 和 B 这两个工具"
- "今天有什么值得做视频的选题"
- "扫描一下 AI 编程工具赛道"

**Don't use for:**
- Simple "what is X" questions (use web_search)
- Tools that require signup/waitlist (can't hands-on test)
- Pure API/SDK tools with no web interface (Safari can't test them)

## Architecture

```
User query → ai-tool-scout
                │
                ├─ Phase 1: Discovery (parallel)
                │   ├─ ProductHunt scan (web_search + Safari)
                │   ├─ GitHub trending (API)
                │   ├─ Hacker News Show HN (web_search)
                │   └─ Web search (通用搜索)
                │
                ├─ Phase 2: Deep Research (sequential, per tool)
                │   ├─ safari_navigate → open tool website
                │   ├─ safari_snapshot → extract features/pricing
                │   ├─ safari_evaluate → structured data
                │   └─ safari_screenshot → visual evidence
                │
                ├─ Phase 3: Report Generation
                │   ├─ Structured markdown (deep-research format)
                │   ├─ Competitor comparison table
                │   └─ Video选题 suggestions
                │
                └─ Output → Obsidian vault
```

## Phase 1: Discovery

### Source Matrix

| Source | Method | Query format | What to extract |
|--------|--------|-------------|-----------------|
| ProductHunt | `web_search site:producthunt.com <topic> 2026` | Topic keywords | Name, votes, tagline, URL |
| GitHub Trending | `web_search site:github.com/trending <topic>` | Topic keywords | Repo name, stars, description |
| Hacker News | `web_search site:news.ycombinator.com "Show HN" <topic>` | Topic keywords | Title, points, comments |
| Web Search | `web_search "<topic> AI tool launch 2026"` | Broad query | Any new tool announcement |

**Completion criteria**: 3-6 candidate tools found, each with name + URL + one-line description.

### Dedup & Filter

Before moving to Phase 2:
1. Remove tools already in Obsidian `选题池.md` (grep for URL)
2. Remove ChatGPT wrappers (check if tool's core is just API call)
3. Remove enterprise-only / waitlist > 1 week
4. Prioritize: free trial > open source > freemium

## Phase 2: Deep Research

For each candidate tool (max 5 to avoid overload):

### 2.1 Open & Verify
```
safari_navigate(url="<tool_url>")
```
**Completion**: page title and URL returned. If redirects to login → skip this tool.

### 2.2 Extract Structured Data
```
safari_read_page(maxLength=10000)
safari_analyze_page()  → meta tags, headings, forms
safari_extract_meta()  → OG tags, description
```

### 2.3 Hands-On Testing (when possible)
```
safari_snapshot() → find interactive elements
# Try the core feature if no login required
safari_click(text="Try now") or safari_click(text="Demo")
safari_wait(2000)
safari_screenshot(fullPage=false)
```

### 2.4 Extract Key Info with JavaScript
```javascript
safari_evaluate(`
  JSON.stringify({
    title: document.title,
    h1: document.querySelector('h1')?.textContent?.trim(),
    pricing: Array.from(document.querySelectorAll('[class*="pric"], [class*="plan"]'))
      .map(el => el.textContent?.trim().substring(0, 100)),
    features: Array.from(document.querySelectorAll('h2, h3, [class*="feature"]'))
      .slice(0, 10)
      .map(el => el.textContent?.trim())
  })
`)
```

**Completion criteria for each tool**: 
- [ ] Name, URL, one-line summary
- [ ] Key features (3-5)
- [ ] Pricing model
- [ ] Screenshot (or placeholder)
- [ ] Hands-on note (tried / login-walled / broken)

## Phase 3: Report Generation

Output to Obsidian: `工具库/<工具名称> 深度调研.md`

Use the deep-research format from `ai-tool-discovery`:

```markdown
# 🔍 <工具名称> 深度调研

> 调研时间: YYYY-MM-DD | 来源: 官网实测 | 由 AI Tool Scout 自动生成

## 一、工具概述
| 维度 | 信息 |
|------|------|
| 工具名称 | xxx |
| 官网 | https://xxx |
| 一句话 | xxx |
| 价格 | 免费/有试用/付费($X) |
| 上手难度 | 初级/中级/高级 |

## 二、核心卖点
### 1. <卖点名称>
📸 [截图]: <描述>

## 三、竞品对比预判
| 维度 | 本工具 | 竞品A | 竞品B |
|------|--------|-------|-------|

## 四、选题角度建议
### 🎯 推荐选题: 「标题」
- 适合平台: YouTube/B站/抖音/All
- Hook: <前5秒抓住人>
- 结构: Hook → 安装 → 实测 → 对比 → CTA
```

### Competitor Quick Compare

When researching multiple tools in the same category, add a comparison table:

```markdown
## 横向对比

| 工具 | 一句话 | 价格 | 亮点 | 短板 | 选题优先级 |
|------|--------|------|------|------|-----------|
| A | ... | 免费 | ... | ... | 🔥🔥🔥 |
| B | ... | $20/mo | ... | ... | 🔥🔥 |
```

### Video选题 Scoring

Each tool gets a 选题 score (1-3 🔥):
- 🔥🔥🔥 = 免费 + 有 demo 页面 + 视觉效果好 + 话题性强 → 立即做视频
- 🔥🔥 = 免费或有试用 + 功能清晰 + 有话题 → 排入选题池
- 🔥 = 付费/登录墙/概念性强 → 低优先级

## Phase 4: Side Effects

After report generation:
1. Append to `选题池.md`: `| N | <工具名称> | AI Tool Scout | <链接> | <一句话> | <难度> | <平台> | <🔥评分> | 待调研 |`
2. Update `README.md` of this skill's repo with latest stats (optional)
3. If particularly hot tool found → suggest immediate video production

## Common Pitfalls

1. **ProductHunt Cloudflare** — ProductHunt has Cloudflare protection. Always use `safari_navigate` (real Safari), never `web_extract` (gets blocked). This is exactly why this skill depends on safari-web-agent.

2. **Drowning in candidates** — Scanning 4 sources can produce 20+ tools. Hard cap at 5 for deep research. Log the rest to a "backlog" section.

3. **Login-walled tools** — Many SaaS tools redirect to login immediately. If `safari_navigate` lands on a login page, extract whatever info is visible on the landing page and mark as "login-walled, limited research".

4. **Chinese vs English tools** — Some tools are China-only (备案 required). Check if the URL loads without VPN. Note in report.

5. **Screenshots without permission** — `safari_screenshot` requires Screen Recording permission. If it fails, use `📸 [截图]: <描述>` placeholder.

6. **Rate limiting** — Don't hammer one source. Rotate sources, add 2-3s delays between Safari navigations.

7. **Outdated pricing on landing page** — Pricing on the landing page may differ from actual signup pricing. Mark as "landing page pricing, verify at signup".

8. **Don't skip the 选题池 dedup** — Always grep `选题池.md` for the tool URL before researching. Re-researching the same tool is waste.

## Verification Checklist

After a full scout run:
- [ ] 3-5 tools discovered from 2+ different sources
- [ ] Each tool has a deep-research doc in `工具库/`
- [ ] Each doc has: overview table, core features, screenshot, 选题 angle
- [ ] No duplicates in 选题池 (verified via grep)
- [ ] At least 1 tool scored 🔥🔥🔥 (immediately actionable)
- [ ] Report written to Obsidian vault

## Quick Recipes

### "Scan AI video tools this week"
```
1. web_search "site:producthunt.com AI video 2026" → find 3-5 tools
2. web_search "site:news.ycombinator.com Show HN video AI" → find 1-2 tools  
3. For top 3 tools: safari_navigate → extract → screenshot → report
4. Generate comparison table + 选题 suggestions
```

### "Deep research on [tool name]"
```
1. web_search "[tool name] review" → get community perspective
2. safari_navigate(tool_url) → hands-on testing
3. safari_screenshot(×3) → homepage, feature page, pricing page
4. Generate full deep-research doc in 工具库/
```

### "Compare [tool A] vs [tool B]"
```
1. safari_navigate(A_url) → extract features, pricing
2. safari_new_tab(B_url) → extract features, pricing
3. Generate side-by-side comparison table
4. Suggest which one makes a better video topic
```

## Files

| Path | Purpose |
|------|---------|
| `references/deep-research-format.md` | Detailed report template |
| `references/source-strategies.md` | Per-source search tactics |
| `templates/comparison-report.md` | Multi-tool comparison template |
| `templates/video-angle-brief.md` | Video选题 brief template |
| `examples/ai-video-tools-scan.md` | Full example: scanning AI video tools |
| `scripts/scout.py` | CLI helper: quick scout a topic |

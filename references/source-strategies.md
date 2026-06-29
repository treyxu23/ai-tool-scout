# Source Strategies

搜索策略参考手册。每个数据源的搜索技巧、提取方法和坑。

## ProductHunt

**特点**：AI 工具发布第一站。有 Cloudflare 保护，必须用 Safari 访问。

**搜索语法**：
```
web_search "site:producthunt.com <topic> 2026"
web_search "site:producthunt.com/topics/ai new launch"
```

**提取方法**：
```javascript
// ProductHunt 列表页提取
safari_evaluate(`
  JSON.stringify(
    Array.from(document.querySelectorAll('[data-test="post-item"]'))
      .slice(0, 10)
      .map(el => ({
        name: el.querySelector('[data-test="post-name"]')?.textContent?.trim(),
        votes: el.querySelector('[data-test="vote-count"]')?.textContent?.trim(),
        tagline: el.querySelector('[data-test="post-tagline"]')?.textContent?.trim(),
        url: el.querySelector('a[data-test="post-name"]')?.href
      }))
  )
`)
```

**坑**：
- Cloudflare 会拦 web_extract 和 headless 浏览器，必须用 `safari_navigate`
- ProductHunt 改版频繁，`data-test` 属性可能变化
- 中文搜索效果差，用英文关键词

## GitHub Trending

**特点**：开源工具的发源地。最适合找开发者工具。

**搜索语法**：
```
web_search "site:github.com/trending <language> <topic>"
web_search "site:github.com awesome-<topic>"
```

**API 方式**（更快，不需要浏览器）：
```bash
curl "https://api.github.com/search/repositories?q=<topic>+created:>2026-01-01&sort=stars"
```

**提取内容**：repo name, stars, description, language, topics

**坑**：
- GitHub API 有 rate limit（未认证 60次/小时）
- Trending 页面每天更新，已不在 trending 的仓库搜索不到

## Hacker News

**特点**：高质量技术讨论，Show HN 标签是新工具发布专区。

**搜索语法**：
```
web_search "site:news.ycombinator.com 'Show HN' <topic> 2026"
```

**提取方法**：HN 页面纯 HTML，用 `web_extract` 即可，不需要 Safari。

**坑**：
- Show HN 帖子可能有很多讨论但工具本身已死
- HN 偏向开发者工具，消费级 AI 工具较少

## Web Search（通用）

**特点**：兜底方案。覆盖 ProductHunt 和 GitHub 之外的工具。

**搜索语法**：
```
web_search "<topic> AI tool launch 2026"
web_search "best AI <topic> tools 2026"
web_search "<topic> AI 工具 推荐 2026"
```

**坑**：
- 结果包含大量软文和付费推广
- 中文搜索结果偏向国内媒体，英文搜索覆盖面更广
- 工具名称可能重复（不同公司同名产品）

## 优先级策略

| 工具来源 | 可信度 | 信息量 | 推荐优先级 |
|----------|:---:|:---:|:---:|
| ProductHunt 高票 | ⭐⭐⭐⭐ | 中 | 第一优先 |
| GitHub 高星 | ⭐⭐⭐⭐⭐ | 高 | 第二优先 |
| HN Show HN | ⭐⭐⭐ | 中 | 第三优先 |
| Web Search 结果 | ⭐⭐ | 低 | 兜底 |

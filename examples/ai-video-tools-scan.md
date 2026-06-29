# Full Example: Scanning AI Video Tools

完整的扫描示例，展示 AI Tool Scout 从发现到报告的完整流程。

## 用户输入

```
> 帮我扫描一下这周新出的 AI 视频工具，看看有没有值得做视频的
```

## Phase 1: 发现

### ProductHunt
```
web_search "site:producthunt.com AI video tool 2026 June"
→ 找到 3 个：
  1. ClipStudio AI — AI video editing, 847 votes
  2. VideoForge — Text to video with avatars, 532 votes
  3. MotionCraft — AI motion graphics, 291 votes
```

### GitHub Trending
```
web_search "site:github.com AI video generation tool"
→ 找到 1 个：
  4. OpenVid — Open source video generation
```

### 过滤
- MotionCraft：企业版，$99/月起 → 放弃
- 保留 3 个：ClipStudio AI, VideoForge, OpenVid

## Phase 2: 深度调研

### ClipStudio AI

```
safari_navigate("https://clipstudio.ai")
safari_read_page(maxLength=8000)

提取结果：
- 一句话：AI-powered video editor, edit videos by typing
- 核心功能：text-to-edit, auto subtitles, background removal, AI voiceover
- 价格：Free tier (5 exports/mo), Pro $19/mo
- 上手难度：初级，界面像 Canva

safari_screenshot(fullPage=false)
📸 [截图]: 首页展示 text-to-edit 功能，输入"remove silence"自动剪辑
```

### VideoForge

```
safari_navigate("https://videoforge.io")
→ 跳转到 /login → 登录墙

safari_readPage(selector=".hero")
提取结果（仅首页可见内容）：
- 一句话：Create studio-quality videos with AI avatars
- 价格：未在首页显示
- 调研状态：⚠️ 登录墙，有限调研
```

### OpenVid

```
safari_navigate("https://github.com/openvid/openvid")
safari_read_page(maxLength=5000)

提取结果：
- 开源，MIT 协议
- 核心功能：text-to-video, image-to-video, video editing
- GitHub ⭐1,200
- 上手难度：中级（需要本地部署）
```

## Phase 3: 报告生成

### 输出到 Obsidian

```
工具库/ClipStudio AI 深度调研.md ✅
工具库/VideoForge 深度调研.md ✅ (登录墙)
工具库/OpenVid 深度调研.md ✅
```

### 选题池追加

```
选题池.md 追加：
| 4 | ClipStudio AI | AI Tool Scout | https://clipstudio.ai | AI视频编辑，打字就能剪 | 初级 | All | 🔥🔥🔥 | 待录制 |
| 5 | OpenVid | AI Tool Scout | https://github.com/openvid | 开源AI视频生成 | 中级 | YouTube/B站 | 🔥🔥 | 待录制 |
```

### 选题建议

```
🎬 今日推荐选题：

🥇 ClipStudio AI — 「打字就能剪视频？这个AI工具把Premiere干掉了」
   平台: All | 难度: 初级 | 为什么优先: 免费+有demo+视觉效果炸裂
   结构: 打开即用(1min) → text-to-edit实测(3min) → vs 传统剪辑(1min) → CTA

🥈 OpenVid — 「开源版Sora？本地运行的AI视频生成」
   平台: YouTube/B站 | 难度: 中级 | 为什么优先: 开源+GitHub热+话题性
   结构: 项目介绍(1min) → 本地部署(2min) → 生成效果实测(2min) → CTA
```

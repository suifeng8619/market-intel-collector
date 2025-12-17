# Hotspot Monitoring Sources (热点监控源) v2.0

Real-time and periodic sources to detect trending keywords before they peak.

**版本**: 2.0.0
**更新**: 2025-12-17
**整合**: 关键词挖掘系统 32+15 平台

---

## 1. 哨站网络架构 (Keshik Scout Network)

基于成吉思汗的"怯薛军"原则：情报网络必须覆盖广阔、反应迅速、层层传递。

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       怯薛军前哨网络架构 v2.0                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌───────────┐     ┌───────────┐     ┌───────────┐     ┌───────────┐       │
│  │  第一环   │     │  第二环   │     │  第三环   │     │  指挥部   │       │
│  │  边境哨   │ ──→ │  中继站   │ ──→ │  分析营   │ ──→ │  大汗帐   │       │
│  │           │     │           │     │           │     │           │       │
│  │ 74个数据源 │     │  初筛过滤  │     │  深度评估  │     │  决策执行  │       │
│  └───────────┘     └───────────┘     └───────────┘     └───────────┘       │
│                                                                             │
│  游戏:19哨站        热度>阈值?        Flash Report      Response Level      │
│  工具:14哨站        去重合并?         五事评估          资源调度            │
│  AI:9哨站           相关性?           Battle Index      行动指令            │
│  通用:10哨站        时效性?           风险评估                              │
│  Reddit:15哨站                                                              │
│  情报专用:7哨站                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. 游戏类哨站 (Gaming Outposts) - 19个

### Tier 1: 核心哨站 (每2小时)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| G-001 | Steam New | https://store.steampowered.com/search/?sort_by=Released_DESC | 新发布游戏 | 24h评论>500 |
| G-002 | Steam Top | https://store.steampowered.com/search/?filter=topsellers | 销量排名 | 新进前20 |
| G-003 | Epic Games | https://store.epicgames.com/en-US/browse | 新发布/免费领取 | 领取量异常 |
| G-004 | itch.io | https://itch.io/games/new-and-popular | 独立游戏 | 24h评论>50 |

### Tier 2: 重要哨站 (每4小时)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| G-005 | GOG | https://www.gog.com/games | DRM-free游戏 | 评分>4.5且评论飙升 |
| G-006 | Humble Bundle | https://www.humblebundle.com/store | Bundle热度 | 限时包销量异常 |
| G-007 | Scratch | https://scratch.mit.edu/explore/projects/all | 病毒项目 | 进入前20 trending |
| G-008 | Newgrounds | https://www.newgrounds.com/games | Flash继承者 | 首页推荐 |
| G-009 | Kongregate | https://www.kongregate.com/games | 网页游戏 | 周榜前10 |
| G-010 | GameJolt | https://gamejolt.com/games | 独立游戏 | 热度飙升 |

### Tier 3: 评测哨站 (每日)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| G-011 | Metacritic | https://www.metacritic.com/browse/game/ | 媒体评分 | 新游戏>80分 |
| G-012 | OpenCritic | https://opencritic.com/browse/all | 综合评分 | 新游戏>80分 |
| G-013 | IGN | https://www.ign.com/reviews/games | 媒体评测 | 评分>9.0 |
| G-014 | GameSpot | https://www.gamespot.com/reviews/ | 媒体评测 | 评分>9.0 |
| G-015 | Polygon | https://www.polygon.com/ | 游戏新闻 | 病毒报道 |
| G-016 | Kotaku | https://kotaku.com/ | 游戏新闻 | 争议/热点 |

### Tier 4: 补充哨站 (每周)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| G-017 | PCGamer | https://www.pcgamer.com/ | PC游戏 | 年度推荐 |
| G-018 | RockPaperShotgun | https://www.rockpapershotgun.com/ | 独立游戏 | 深度报道 |
| G-019 | GiantBomb | https://www.giantbomb.com/ | 综合评测 | Quick Look热度 |

---

## 3. 工具类哨站 (Tool Outposts) - 14个

### Tier 1: 核心哨站 (每2小时)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| T-001 | Product Hunt | https://www.producthunt.com/ | 新产品发布 | >300 upvotes |
| T-002 | Hacker News | https://news.ycombinator.com/ | 技术讨论 | 首页+>100评论 |
| T-003 | GitHub Trending | https://github.com/trending | 开源热度 | 进入日榜 |
| T-004 | DevHunt | https://devhunt.org/ | 开发者工具 | 首页推荐 |

### Tier 2: 评测哨站 (每日)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| T-005 | G2 | https://www.g2.com/products/new | 用户评价 | 新产品>20评价/月 |
| T-006 | Capterra | https://www.capterra.com/ | 软件评测 | 新进热门榜 |
| T-007 | TrustRadius | https://www.trustradius.com/ | B2B评测 | 高分新产品 |
| T-008 | GetApp | https://www.getapp.com/ | SaaS发现 | Category Leader |
| T-009 | AlternativeTo | https://alternativeto.net/ | 替代品发现 | 热门替代 |

### Tier 3: 资讯哨站 (每日)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| T-010 | TechCrunch | https://techcrunch.com/ | 融资新闻 | A轮+热度 |
| T-011 | TheVerge | https://www.theverge.com/ | 科技新闻 | 病毒报道 |
| T-012 | Wired | https://www.wired.com/ | 深度报道 | 工具专题 |
| T-013 | Ars Technica | https://arstechnica.com/ | 技术分析 | 深度评测 |
| T-014 | VentureBeat | https://venturebeat.com/ | 行业动态 | 融资/发布 |

---

## 4. AI工具哨站 (AI Tool Outposts) - 9个

### Tier 1: 核心哨站 (每4小时)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| A-001 | There's An AI | https://theresanaiforthat.com/ | AI工具发现 | 新热门工具 |
| A-002 | Futurepedia | https://www.futurepedia.io/ | AI目录 | 周热门 |
| A-003 | AI Tool Hunt | https://www.aitoolhunt.com/ | AI工具评测 | 高评分新品 |

### Tier 2: 社区哨站 (每日)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| A-004 | Hugging Face | https://huggingface.co/spaces | 模型/空间热度 | Trending Space |
| A-005 | Papers with Code | https://paperswithcode.com/ | 研究热度 | 热门论文 |
| A-006 | AI Awesome | https://github.com/ai-collection/ai-collection | 精选列表 | 新增热门 |

### Tier 3: 补充哨站 (每周)

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| A-007 | TopAI.tools | https://topai.tools/ | AI排名 | 榜单变动 |
| A-008 | SaaS AI Tools | https://saasaitools.com/ | SaaS+AI | 新品发布 |
| A-009 | AI Tool Directory | https://aitoolsdirectory.com/ | 综合目录 | 月度热门 |

---

## 5. 通用哨站 (Universal Outposts) - 10个

### 搜索趋势哨站

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| U-001 | Google Trends | https://trends.google.com/trending?geo=US | 突破性查询 | Breakout状态 |
| U-002 | Google Trends | https://trends.google.com/trends/explore | 飙升查询 | +500%以上 |
| U-003 | Exploding Topics | https://explodingtopics.com/ | 新兴话题 | 爆发标记 |

### 社交媒体哨站

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| U-004 | YouTube | https://www.youtube.com/feed/trending?bp=4gIcGhpnYW1pbmdfY29ycHVzX21vc3RfcG9wdWxhcg%3D%3D | 游戏视频热度 | 24h>100k views |
| U-005 | TikTok | 手动搜索 #gaming #tools | 病毒内容 | >1M views |
| U-006 | Twitter/X | 搜索API | 话题热度 | >1000互动 |

### 知识库哨站

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| U-007 | Wikipedia | https://en.wikipedia.org/wiki/Special:RecentChanges | 新条目 | 游戏/工具新页 |
| U-008 | Know Your Meme | https://knowyourmeme.com/memes/new | 新meme | 游戏/工具相关 |
| U-009 | Fandom | https://www.fandom.com/ | Wiki创建 | 新游戏Wiki |
| U-010 | TV Tropes | https://tvtropes.org/ | 文化影响 | 新游戏条目 |

---

## 6. Reddit 哨站 (Reddit Outposts) - 15个

### 游戏类 (7个)

| 哨站ID | Subreddit | 成员数 | 采集频率 | 触发条件 |
|--------|-----------|--------|----------|----------|
| R-G01 | r/gaming | 39.6M | 每4小时 | >5k upvotes |
| R-G02 | r/Games | 3.5M | 每4小时 | >2k upvotes |
| R-G03 | r/pcgaming | 3.2M | 每4小时 | >1k upvotes |
| R-G04 | r/IndieGaming | 360k | 每6小时 | >500 upvotes |
| R-G05 | r/WebGames | 180k | 每6小时 | >200 upvotes |
| R-G06 | r/incremental_games | 150k | 每日 | >200 upvotes |
| R-G07 | r/freegames | 520k | 每日 | >500 upvotes |

### 工具类 (5个)

| 哨站ID | Subreddit | 成员数 | 采集频率 | 触发条件 |
|--------|-----------|--------|----------|----------|
| R-T01 | r/SideProject | 200k | 每4小时 | >100 upvotes |
| R-T02 | r/SaaS | 90k | 每6小时 | >100 upvotes |
| R-T03 | r/startups | 1.2M | 每6小时 | >300 upvotes |
| R-T04 | r/Entrepreneur | 2.1M | 每6小时 | >500 upvotes |
| R-T05 | r/InternetIsBeautiful | 17.5M | 每日 | >2k upvotes |

### AI类 (3个)

| 哨站ID | Subreddit | 成员数 | 采集频率 | 触发条件 |
|--------|-----------|--------|----------|----------|
| R-A01 | r/artificial | 900k | 每4小时 | >300 upvotes |
| R-A02 | r/MachineLearning | 2.8M | 每6小时 | >500 upvotes |
| R-A03 | r/ChatGPT | 5.5M | 每4小时 | >1k upvotes |

---

## 7. 情报专用哨站 (Intelligence Outposts) - 7个

### 法律风险哨站

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| I-001 | WIPO | https://www.wipo.int/portal/en/ | 商标注册 | 新注册公示 |
| I-002 | Google Patents | https://patents.google.com/ | 专利检索 | 相关专利 |

### 安全评估哨站

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| I-003 | VirusTotal | https://www.virustotal.com/ | 安全检测 | 下载类产品必查 |
| I-004 | Web of Trust | https://www.mywot.com/ | 网站信誉 | 官网信誉 |

### 实时舆情哨站

| 哨站ID | 平台 | URL | 监控目标 | 触发条件 |
|--------|------|-----|----------|----------|
| I-005 | Discord | 搜索公开服务器 | 社区活跃度 | 成员增长异常 |
| I-006 | Twitch | https://www.twitch.tv/directory | 直播热度 | 新游戏观众暴涨 |
| I-007 | Wayback Machine | https://web.archive.org/ | 历史验证 | 确认产品历史 |

---

## 8. 信号阈值矩阵

### 游戏类信号

| 指标 | 低 (黄色) | 中 (橙色) | 高 (红色) |
|------|-----------|-----------|-----------|
| Steam 评论 (24h) | 100+ | 500+ | 2000+ |
| Reddit upvotes | 500+ | 2k+ | 10k+ |
| YouTube views (24h) | 100k+ | 500k+ | 2M+ |
| TikTok 标签播放 | 1M+ | 10M+ | 100M+ |
| Twitch 观众 | 1k+ | 10k+ | 50k+ |
| Google Trends | 20+ | 50+ | 80+ |
| Scratch Trending | 前50 | 前20 | 前5 |

### 工具类信号

| 指标 | 低 (黄色) | 中 (橙色) | 高 (红色) |
|------|-----------|-----------|-----------|
| Product Hunt upvotes | 200+ | 500+ | 1000+ |
| Hacker News points | 50+ | 150+ | 400+ |
| GitHub stars (周增) | 100+ | 500+ | 2000+ |
| G2 reviews (月) | 10+ | 30+ | 100+ |
| Reddit mentions | 50+ | 200+ | 500+ |
| Google Trends | 10+ | 30+ | 60+ |

### AI工具信号

| 指标 | 低 (黄色) | 中 (橙色) | 高 (红色) |
|------|-----------|-----------|-----------|
| There's An AI ranking | 前100 | 前20 | 前5 |
| Hugging Face likes | 100+ | 500+ | 2000+ |
| r/ChatGPT mentions | 50+ | 200+ | 1000+ |
| GitHub stars | 500+ | 2000+ | 10000+ |
| Twitter mentions | 100+ | 500+ | 2000+ |

---

## 9. 巡逻日程表 v2.0

### 实时巡逻 (每2小时)
```
□ Steam New Releases
□ Product Hunt 首页
□ Hacker News 首页
□ GitHub Trending
```

### 高频巡逻 (每4小时)
```
□ Reddit 核心板块 (gaming, Games, SideProject)
□ Scratch Explore
□ YouTube Gaming Trending
□ There's An AI 首页
□ Discord 热门服务器
□ Twitch 游戏目录
```

### 每日巡逻 (10分钟)
```
□ 06:00 - Product Hunt 昨日冠军
□ 08:00 - Hacker News 首页
□ 10:00 - G2/Capterra 新品
□ 12:00 - Steam 趋势
□ 14:00 - Reddit 全板块扫描
□ 18:00 - YouTube 24h热门
□ 20:00 - AI工具目录更新
□ 22:00 - Google Trends Breakout
```

### 每周巡逻 (30分钟)
```
□ 周一 - Scratch/itch.io/GameJolt 全扫描
□ 周二 - 评测站 (Metacritic/IGN/G2) 新品
□ 周三 - GitHub/Hugging Face 周榜
□ 周四 - 媒体站 (TechCrunch/Polygon) 热文
□ 周五 - 全平台信号汇总 + 趋势分析
□ 周末 - 案例库更新 + 阈值校准 + 效能评估
```

---

## 10. 中继站过滤规则

```yaml
filter_rules:
  # 去重规则
  dedup:
    - same_keyword_within_24h: ignore
    - same_keyword_different_source: merge_and_boost
    - similar_keyword_fuzzy: group_for_review

  # 相关性规则
  relevance:
    must_be: [game, tool, mod, app, ai_tool, browser_game]
    must_not_be: [news_event, celebrity, sports, politics]
    language: [en]  # 仅英文
    exclude_brands: [microsoft, google, apple, meta, amazon]  # 大厂产品单独处理

  # 强度规则
  strength:
    min_signal_strength: 3/10
    multi_source_boost: +2  # 多源信号加成
    velocity_boost: +1      # 快速增长加成
    negative_sentiment_penalty: -2  # 负面情绪减分

  # 时效规则
  timing:
    ignore_if_peak_passed: true
    prefer_early_stage: true
    max_age_for_flash: 7d   # 超过7天需要完整评估

  # 风险预警
  risk_flags:
    copyright_concern: [fan_mod, clone, unofficial]
    creator_risk: [young_creator, anonymous, no_history]
    legal_risk: [gambling, adult, violence]
```

---

## 11. 信号传递格式

### 边境哨站 → 中继站

```json
{
  "outpost_signal": {
    "outpost_id": "G-007",
    "platform": "Scratch",
    "timestamp": "2024-09-01T08:00:00Z",
    "keyword": "Sprunki",
    "raw_metric": "trending #5",
    "metric_delta": "+15 positions in 24h",
    "url": "https://scratch.mit.edu/projects/...",
    "signal_strength": 7
  }
}
```

### 中继站 → 分析营

```json
{
  "relay_signal": {
    "signal_id": "uuid",
    "timestamp": "2024-09-01T08:30:00Z",
    "keyword": "Sprunki",
    "type_guess": "game",
    "sources": [
      {"outpost_id": "G-007", "platform": "Scratch", "strength": 7},
      {"outpost_id": "R-G05", "platform": "r/WebGames", "strength": 5}
    ],
    "aggregated_strength": 8,
    "dedup_status": "new",
    "relevance_score": 9,
    "risk_flags": ["fan_mod", "young_creator"],
    "recommendation": "forward_to_analysis",
    "urgency": "high"
  }
}
```

### 分析营 → 大汗帐

```json
{
  "analysis_result": {
    "signal_id": "uuid",
    "keyword": "Sprunki",
    "analysis_timestamp": "2024-09-01T09:00:00Z",
    "flash_assessment": {
      "five_factors": {
        "dao": 9, "tian": 10, "di": 9, "jiang": 4, "fa": 8
      },
      "battle_index": 8.0,
      "confidence": "medium"
    },
    "risk_assessment": {
      "legal_risk": "high",
      "creator_risk": "high",
      "overall_risk": "medium-high"
    },
    "alert_level": "orange",
    "recommended_response_level": "L3",
    "forward_to": "command",
    "notes": "高机会但需注意法律风险"
  }
}
```

---

## 12. Playwright 监控脚本

### 批量平台检查

```javascript
// Steam + Product Hunt + Hacker News 并行检查
async function dailyQuickScan() {
  const pages = [
    { url: "https://store.steampowered.com/explore/new/", signal: "steam" },
    { url: "https://www.producthunt.com/", signal: "ph" },
    { url: "https://news.ycombinator.com/", signal: "hn" }
  ];

  for (const page of pages) {
    await mcp__playwright__browser_navigate({ url: page.url });
    await mcp__playwright__browser_wait_for({ time: 2 });
    const snapshot = await mcp__playwright__browser_snapshot();
    // 分析 snapshot 提取信号
  }
}
```

### Scratch Trending 检查

```javascript
// Scratch 热门项目扫描
async function scratchTrendingScan() {
  await mcp__playwright__browser_navigate({
    url: "https://scratch.mit.edu/explore/projects/all"
  });
  await mcp__playwright__browser_wait_for({ time: 3 });
  await mcp__playwright__browser_snapshot();
  // 查找: 新项目进入前20，非已知IP
}
```

### AI工具目录检查

```javascript
// There's An AI 扫描
async function aiToolsScan() {
  await mcp__playwright__browser_navigate({
    url: "https://theresanaiforthat.com/"
  });
  await mcp__playwright__browser_wait_for({ time: 2 });
  await mcp__playwright__browser_snapshot();
  // 查找: 新上榜工具，评分>4.0
}
```

---

## 13. 哨站效能评估

### 月度评估模板

```markdown
## 哨站月度报告 - {YYYY-MM}

### 信号统计
| 哨站类型 | 总信号 | 有效信号 | 误报率 | 漏报数 |
|----------|--------|----------|--------|--------|
| 游戏类   | ___    | ___      | ___% | ___    |
| 工具类   | ___    | ___      | ___% | ___    |
| AI类     | ___    | ___      | ___% | ___    |
| Reddit   | ___    | ___      | ___% | ___    |
| 情报专用 | ___    | ___      | ___% | ___    |

### 最佳发现 Top 3
1. 哨站:___ 关键词:___ 提前天数:___
2. 哨站:___ 关键词:___ 提前天数:___
3. 哨站:___ 关键词:___ 提前天数:___

### 漏报案例分析
- 案例:___ 应由哨站:___ 发现
- 漏报原因:___
- 改进措施:___

### 阈值调整建议
- 调高:___
- 调低:___
- 新增哨站:___
- 移除哨站:___
```

---

## 14. Sprunki 回溯分析 (更新版)

如果 v2.0 哨站网络在 2024-08 就位：

| 日期 | 哨站 | 应发现信号 | 预期触发 |
|------|------|------------|----------|
| 2024-08-24 | G-007 (Scratch) | 新项目上传 | 监控启动 |
| 2024-08-28 | G-007 (Scratch) | 进入前50 | 黄色关注 |
| 2024-09-01 | G-007 (Scratch) | 进入前20 | 橙色预警 |
| 2024-09-01 | R-G05 (r/WebGames) | 首个热帖 | 信号合并 |
| 2024-09-02 | 中继站 | 多源信号 | 转分析营 |
| 2024-09-02 | 分析营 | Flash Report | Battle Index 8+ |
| 2024-09-02 | I-001 (WIPO) | 无商标注册 | 法律风险标记 |
| 2024-09-03 | 大汗帐 | L3/L4 响应 | 行动指令下达 |

**结论**: v2.0 网络可在 **2024-09-02** 触发响应，比峰值 (2024-12-29) 提前 **118天**。

---

## 15. 与关键词挖掘系统集成

哨站网络与关键词挖掘系统 (`/Users/suifeng/python/2026/seo-tools/关键词挖掘`) 共享配置：

```yaml
# 配置文件位置
platforms_config: /关键词挖掘/config/platforms.yaml
subreddits_config: /关键词挖掘/config/subreddits.yaml

# 采集脚本复用
scraper_module: /关键词挖掘/src/keyword_miner/scrapers/

# 数据共享
keyword_db: /关键词挖掘/data/keywords.db
signals_output: /资料收集/signals/
```

---

*Hotspot monitoring reference for market-intel-collector skill*
*Enhanced by: Genghis Khan (怯薛军前哨网络)*
*Integrated with: Keyword Mining System v3.12*
*Version: 2.0.0*

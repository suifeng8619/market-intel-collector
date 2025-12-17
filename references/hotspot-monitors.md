# Hotspot Monitoring Sources (热点监控源)

Real-time and periodic sources to detect trending keywords before they peak.

---

## Gaming Hotspots

### Tier 1: Must Monitor (Every Hour)

| Source | URL | What to Watch | Signal |
|--------|-----|---------------|--------|
| Steam New & Trending | https://store.steampowered.com/explore/new/ | New releases gaining traction | >1000 reviews in 24h |
| Steam Top Sellers | https://store.steampowered.com/search/?filter=topsellers | Sudden rank jumps | New entry in top 20 |
| itch.io Popular | https://itch.io/games/top-rated | Indie games going viral | >100 ratings spike |

### Tier 2: Daily Check

| Source | URL | What to Watch | Signal |
|--------|-----|---------------|--------|
| Reddit r/gaming Hot | https://reddit.com/r/gaming/hot | Games being discussed | >5k upvotes |
| Reddit r/indiegaming | https://reddit.com/r/indiegaming/hot | Indie discoveries | >500 upvotes |
| TikTok Gaming | Manual search #gaming | Viral game clips | >1M views |
| YouTube Gaming Trending | https://youtube.com/gaming | Trending game videos | Unusual spike |

### Tier 3: Weekly Scan

| Source | URL | What to Watch | Signal |
|--------|-----|---------------|--------|
| Metacritic Upcoming | https://www.metacritic.com/browse/game/ | High anticipation | >50 critic reviews day 1 |
| IGN Reviews | https://www.ign.com/reviews/games | Breakout scores | 9+ score |
| Kotaku/Polygon | News sections | Viral coverage | Multiple articles |

---

## Tool/SaaS Hotspots

### Tier 1: Must Monitor (Daily)

| Source | URL | What to Watch | Signal |
|--------|-----|---------------|--------|
| Product Hunt Daily | https://www.producthunt.com/ | Top launches | >500 upvotes day 1 |
| Hacker News | https://news.ycombinator.com/ | Tech tool discussions | Front page + >100 comments |
| Reddit r/SaaS | https://reddit.com/r/SaaS/hot | New tool mentions | >100 upvotes |

### Tier 2: Weekly Check

| Source | URL | What to Watch | Signal |
|--------|-----|---------------|--------|
| G2 New Products | https://www.g2.com/products/new | New listings with reviews | >20 reviews in first month |
| TechCrunch | https://techcrunch.com/ | Funding announcements | Series A+ with buzz |
| ProductHunt Weekly | Email digest | Top products of week | Weekly rank #1-5 |

### Tier 3: Monthly Scan

| Source | URL | What to Watch | Signal |
|--------|-----|---------------|--------|
| Crunchbase Trending | https://www.crunchbase.com/ | Funding trends | Hot sector signal |
| G2 Grid Reports | Category reports | Market movers | New leaders/challengers |

---

## Universal Signals (All Categories)

### Google Trends Breakout Detection

```
URL: https://trends.google.com/trending?geo=US
Check: Daily trending searches
Signal: Keyword appears in "Breakout" category
```

### Twitter/X Monitoring

```
Search: "{category} launched" OR "{category} alternative"
Signal: >1000 engagements on announcement
```

### YouTube Velocity

```
Search: Sort by upload date, filter last 24h
Signal: New video with >100k views in <24h
```

---

## Playwright Monitoring Scripts

### Steam New Releases Check

```javascript
// Navigate to Steam new releases
mcp__playwright__browser_navigate({ url: "https://store.steampowered.com/explore/new/" })
mcp__playwright__browser_wait_for({ time: 3 })
mcp__playwright__browser_snapshot()
// Look for: games with "Very Positive" and high review counts
```

### Product Hunt Today Check

```javascript
// Navigate to Product Hunt
mcp__playwright__browser_navigate({ url: "https://www.producthunt.com/" })
mcp__playwright__browser_wait_for({ time: 2 })
mcp__playwright__browser_snapshot()
// Look for: products with >300 upvotes before noon
```

### Google Trends Breakout Check

```javascript
// Navigate to trending searches
mcp__playwright__browser_navigate({ url: "https://trends.google.com/trending?geo=US" })
mcp__playwright__browser_wait_for({ time: 3 })
mcp__playwright__browser_snapshot()
// Look for: gaming/tool related breakouts
```

---

## Alert Thresholds

### Gaming Keywords

| Metric | Low | Medium | High |
|--------|-----|--------|------|
| Steam reviews (24h) | 100+ | 500+ | 2000+ |
| Reddit upvotes | 500+ | 2k+ | 10k+ |
| YouTube views (24h) | 100k+ | 500k+ | 2M+ |
| TikTok hashtag views | 1M+ | 10M+ | 100M+ |
| Google Trends index | 20+ | 50+ | 80+ |

### Tool Keywords

| Metric | Low | Medium | High |
|--------|-----|--------|------|
| Product Hunt upvotes | 200+ | 500+ | 1000+ |
| Hacker News points | 50+ | 150+ | 400+ |
| G2 reviews (month) | 10+ | 30+ | 100+ |
| Reddit mentions | 50+ | 200+ | 500+ |
| Google Trends index | 10+ | 30+ | 60+ |

---

## Recommended Monitoring Schedule

### Daily (5 minutes)
1. Product Hunt front page
2. Hacker News front page
3. Steam New & Trending

### Every 3 Days (10 minutes)
1. Reddit gaming/SaaS hot posts
2. YouTube trending in category
3. Google Trends breakouts

### Weekly (20 minutes)
1. G2/Capterra new entries
2. TechCrunch funding news
3. Full Google Trends category scan

---

## Integration with Flash Reports

When a hotspot is detected:

1. **Trigger Flash Report** immediately
2. Check if Battle Index > 5
3. If yes, queue for Full Report
4. If no, log and monitor for changes

```
Hotspot Detected → Flash Report (5 min) → GO? → Full Report (30 min)
                                        → NO-GO? → Log & Monitor
```

---

## Keshik Scout Network (怯薛军前哨网络) - New in v1.1.0

基于成吉思汗的"怯薛军"原则：情报网络必须覆盖广阔、反应迅速、层层传递。

### 前哨站分类

```
┌─────────────────────────────────────────────────────────────────┐
│                     怯薛军前哨网络架构                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐   │
│  │ 第一环  │     │ 第二环  │     │ 第三环  │     │ 指挥部  │   │
│  │ 边境哨  │ ──→ │ 中继站  │ ──→ │ 分析营  │ ──→ │ 大汗帐  │   │
│  │         │     │         │     │         │     │         │   │
│  │ 原始信号 │     │ 初筛过滤 │     │ 深度评估 │     │ 决策执行 │   │
│  └─────────┘     └─────────┘     └─────────┘     └─────────┘   │
│                                                                 │
│  Scratch         热度>阈值?      Flash Report    Response Level │
│  itch.io         去重?           五事评估        资源调度       │
│  Reddit          相关性?         Battle Index    行动指令       │
│  TikTok                                                         │
└─────────────────────────────────────────────────────────────────┘
```

---

### 第一环：边境哨站 (信号采集)

**任务**：24/7 监控，发现任何异常立即上报

#### 游戏类前哨

| 哨站ID | 平台 | 监控目标 | 采集频率 | 触发条件 |
|--------|------|----------|----------|----------|
| G-001 | Scratch Explore | 新项目 trending | 每4小时 | 进入前20 |
| G-002 | itch.io New | 新游戏评论 | 每6小时 | 24h评论>50 |
| G-003 | Steam New | 新发布游戏 | 每2小时 | 24h评论>500 |
| G-004 | Reddit r/WebGames | 热帖 | 每4小时 | >200 upvotes |
| G-005 | TikTok #indiegame | 视频热度 | 每日 | >500k views |
| G-006 | YouTube Gaming | 新视频 | 每4小时 | 24h>100k views |

#### 工具类前哨

| 哨站ID | 平台 | 监控目标 | 采集频率 | 触发条件 |
|--------|------|----------|----------|----------|
| T-001 | Product Hunt | 当日发布 | 每2小时 | >300 upvotes |
| T-002 | Hacker News | 首页帖子 | 每小时 | >100 points |
| T-003 | Reddit r/SideProject | 热帖 | 每4小时 | >100 upvotes |
| T-004 | GitHub Trending | 新仓库 | 每日 | 进入日榜 |
| T-005 | Twitter/X | #buildinpublic | 每4小时 | >500 likes |

#### 通用前哨

| 哨站ID | 平台 | 监控目标 | 采集频率 | 触发条件 |
|--------|------|----------|----------|----------|
| U-001 | Google Trends | 突破性查询 | 每日 | Breakout 状态 |
| U-002 | Google Trends | 飙升查询 | 每日 | +500% 以上 |
| U-003 | Know Your Meme | 新条目 | 每周 | 游戏/工具相关 |

---

### 第二环：中继站 (信号过滤)

**任务**：接收边境哨信号，过滤噪音，初步判断价值

#### 过滤规则

```yaml
filter_rules:
  # 去重规则
  dedup:
    - same_keyword_within_24h: ignore
    - same_keyword_different_source: merge_and_boost

  # 相关性规则
  relevance:
    - must_be: [game, tool, mod, app]
    - must_not_be: [news_event, celebrity, sports]
    - language: [en, zh] # 仅英文和中文

  # 强度规则
  strength:
    - min_signal_strength: 3/10
    - if_multiple_sources: boost_priority
    - if_rising_fast: boost_priority

  # 时效规则
  timing:
    - ignore_if_peak_passed: true
    - prefer_early_stage: true
```

#### 中继站输出格式

```json
{
  "relay_signal": {
    "signal_id": "uuid",
    "timestamp": "ISO8601",
    "keyword": "string",
    "type_guess": "game | tool | unknown",

    "sources": [
      {
        "outpost_id": "G-001",
        "platform": "Scratch",
        "raw_metric": "trending #5",
        "signal_strength": "7/10"
      }
    ],

    "aggregated_strength": "7/10",
    "dedup_status": "new | seen | merged",
    "relevance_score": "8/10",

    "recommendation": "forward_to_analysis | monitor | ignore",
    "urgency": "high | medium | low"
  }
}
```

---

### 第三环：分析营 (深度评估)

**任务**：接收中继站信号，执行 Flash Report，输出 Battle Index

#### 分析流程

```
收到信号
    ↓
执行 60秒超速预警
    ↓
五事快速评估
    ↓
计算 Battle Index
    ↓
    ├── Index ≥ 7 → 红色警报 → 立即上报大汗帐
    ├── Index 5-7 → 橙色预警 → 排队等待完整评估
    ├── Index 3-5 → 黄色关注 → 加入监控列表
    └── Index < 3 → 存档 → 记录到案例库
```

#### 分析营输出格式

```json
{
  "analysis_result": {
    "signal_id": "uuid (from relay)",
    "keyword": "string",
    "analysis_timestamp": "ISO8601",

    "flash_assessment": {
      "five_factors": {
        "dao": "X/10",
        "tian": "X/10",
        "di": "X/10",
        "jiang": "X/10",
        "fa": "X/10"
      },
      "battle_index": "X/10",
      "confidence": "high | medium | low"
    },

    "alert_level": "red | orange | yellow | none",
    "recommended_response_level": "L1 | L2 | L3 | L4 | L5",

    "forward_to": "command | queue | monitor | archive",
    "notes": "string"
  }
}
```

---

### 指挥部：大汗帐 (决策执行)

**任务**：接收分析营报告，分配资源，下达行动指令

#### 决策矩阵

| 警报等级 | 响应等级 | 决策时限 | 决策者 |
|----------|----------|----------|--------|
| 🔴 红色 | L4-L5 | 2小时内 | 需确认 |
| 🟠 橙色 | L3-L4 | 24小时内 | 自动 |
| 🟡 黄色 | L2-L3 | 持续监控 | 自动 |
| ⚪ 无 | L1 | 存档 | 自动 |

#### 行动指令格式

```json
{
  "action_order": {
    "order_id": "uuid",
    "issued_at": "ISO8601",
    "keyword": "string",

    "response_level": "L5",
    "resource_allocation": {
      "person_days": 10,
      "priority": "P0",
      "deadline": "ISO8601"
    },

    "action_items": [
      {
        "action": "execute_full_report",
        "deadline": "T+4h",
        "assigned_to": "auto"
      },
      {
        "action": "domain_check",
        "deadline": "T+2h",
        "assigned_to": "auto"
      },
      {
        "action": "longtail_blitz_tier1",
        "deadline": "T+8h",
        "assigned_to": "content_team"
      }
    ],

    "success_criteria": ["string"],
    "abort_conditions": ["string"]
  }
}
```

---

### 哨站巡逻日程

#### 每日巡逻 (10分钟)

```
□ 06:00 - 检查 Product Hunt 昨日冠军
□ 08:00 - 检查 Hacker News 首页
□ 12:00 - 检查 Steam New & Trending
□ 18:00 - 检查 Reddit 热帖
□ 22:00 - 检查 Google Trends Breakout
```

#### 每周巡逻 (30分钟)

```
□ 周一 - Scratch Explore 全扫描
□ 周二 - itch.io 新游戏扫描
□ 周三 - GitHub Trending 周榜
□ 周四 - YouTube Gaming 趋势
□ 周五 - 全平台信号汇总
□ 周末 - 案例库更新 + 阈值调整
```

---

### Sprunki 回溯：哨站本应发现什么？

| 日期 | 哨站 | 应发现信号 | 实际情况 |
|------|------|------------|----------|
| 2024-08-24 | G-001 (Scratch) | 新项目上传 | 未监控 |
| 2024-09-01 | G-001 (Scratch) | 进入 trending | 未监控 |
| 2024-09-15 | U-001 (Trends) | 搜索量飙升 | 未监控 |
| 2024-10-01 | G-006 (YouTube) | 视频爆发 | 未监控 |
| 2024-10-15 | U-002 (Trends) | Breakout 状态 | 未监控 |
| 2024-12-17 | - | 我们发现时 | 已过峰值 |

**教训**：如果 G-001 哨站在 2024-09-01 触发，我们有 3 个月的先发优势。

---

### 哨站效能评估

每月评估各哨站表现：

```markdown
## 哨站月度报告

### 信号统计
- 总信号数：___
- 有效信号数：___
- 误报率：___%
- 漏报案例：___

### 最佳发现
- 哨站：___
- 关键词：___
- 提前发现天数：___

### 需要调整
- 阈值调整：___
- 新增哨站：___
- 移除哨站：___
```

---

*Hotspot monitoring reference for market-intel-collector skill*
*Enhanced by: Genghis Khan (怯薛军前哨网络)*
*Version: 1.1.0*

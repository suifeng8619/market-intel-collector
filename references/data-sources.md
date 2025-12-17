# Data Sources Reference v2.0

> 整合自关键词挖掘系统 (32平台 + 15 subreddits) + 情报专用源
> 最后更新: 2025-12-17

---

## 一、数据源架构

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        市场情报数据源架构 v2.0                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    第一层：关键词挖掘系统共享源                        │   │
│  │                    (32 平台 + 15 Subreddits)                        │   │
│  │                                                                      │   │
│  │   游戏 (10)    工具 (9)     AI (6)      通用 (7)     Reddit (15)   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    第二层：情报专用补充源                              │   │
│  │                                                                      │   │
│  │   法律风险 (3)   竞争情报 (3)   安全评估 (2)   实时舆情 (4)          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  共计: 47 平台 + 15 Subreddits = 62 数据源                                 │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 二、游戏类平台 (10个)

> 来源: 关键词挖掘系统 `config/platforms.yaml`

### Tier 1: 必采 (P0)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **Steam** | `steam` | store.steampowered.com | 新品、热销、愿望单、评论 | Web爬虫 |
| **itch.io** | `itch_io` | itch.io | 独立游戏、评分、新品 | Web爬虫 |
| **CrazyGames** | `crazygames` | crazygames.com | 网页游戏、热门趋势 | Web爬虫 |

### Tier 2: 应采 (P1)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **GOG** | `gog` | gog.com | DRM-Free 游戏趋势 | Web爬虫 |
| **Poki** | `poki` | poki.com | 网页游戏热度 | Web爬虫 |
| **Game Jolt** | `gamejolt` | gamejolt.com | 独立游戏、社区热度 | API |
| **Metacritic** | `metacritic` | metacritic.com | 媒体/用户评分 | Web爬虫 |
| **OpenCritic** | `opencritic` | opencritic.com | 聚合评分 | Web爬虫 |

### Tier 3: 选采 (P2)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **Newgrounds** | `newgrounds` | newgrounds.com | Flash/HTML5 游戏 | Web爬虫 |
| **Humble Bundle** | `humblebundle` | humblebundle.com | 游戏包、趋势 | Web爬虫 |

### WebFetch Prompts

**Steam Store**
```
URL: https://store.steampowered.com/app/{app_id}/
Extract: game title, description, release date, developer, publisher,
price, review sentiment, review percentage, total reviews, tags,
platforms, languages
Fallback: Playwright (age gate)
```

**itch.io**
```
URL: https://itch.io/search?q={keyword}
Extract: game name, author, ratings count, avg rating, downloads,
price, tags, last updated
Fallback: WebSearch (Cloudflare block)
```

**Metacritic**
```
URL: https://www.metacritic.com/game/{game_name}/
Extract: metascore, user score, critic reviews count, user reviews count,
summary, critic quotes (top 3)
```

---

## 三、工具类平台 (9个)

> 来源: 关键词挖掘系统 `config/platforms.yaml`

### Tier 1: 必采 (P0)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **Product Hunt** | `producthunt` | producthunt.com | 新品、投票、评论 | API |
| **AlternativeTo** | `alternativeto` | alternativeto.net | 替代品需求、热门 | Web爬虫 |

### Tier 2: 应采 (P1)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **SaaSHub** | `saashub` | saashub.com | SaaS 趋势 | Web爬虫 |
| **Chrome Web Store** | `chrome_webstore` | chromewebstore.google.com | 扩展热门、新品 | Web爬虫 |
| **Firefox Add-ons** | `firefox_addons` | addons.mozilla.org | 扩展趋势 | Web爬虫 |
| **G2** | `g2` | g2.com | 软件评测 | Web爬虫 |

### Tier 3: 选采 (P2)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **Capterra** | `capterra` | capterra.com | 软件评测 | Web爬虫 |
| **Slant** | `slant` | slant.co | 工具对比 | Web爬虫 |
| **StackShare** | `stackshare` | stackshare.io | 技术栈趋势 | Web爬虫 |

### WebFetch Prompts

**Product Hunt**
```
URL: https://www.producthunt.com/products/{tool_name}
Extract: tagline, description, upvotes, launch date, maker info,
reviews summary, topics, related products
```

**G2**
```
URL: https://www.g2.com/products/{tool_name}/reviews
Extract: overall rating, total reviews, ease of use, support quality,
top likes, top dislikes, best for categories, pricing tier
```

**AlternativeTo**
```
URL: https://alternativeto.net/software/{tool_name}/
Extract: description, likes, alternatives list, tags, platforms,
user comments summary
```

---

## 四、AI 工具类平台 (6个)

> 来源: 关键词挖掘系统 `config/platforms.yaml`
> **重要**: 市场情报 Skill 原版完全缺失此类别

### Tier 1: 必采 (P0)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **There's An AI For That** | `theresanaiforthat` | theresanaiforthat.com | AI 工具目录、热门 | Web爬虫 |
| **Hugging Face** | `huggingface` | huggingface.co | Spaces + Models 热度 | API |
| **GitHub Trending** | `github_trending` | github.com/trending | 开源 AI 项目 | Web爬虫 |
| **Hacker News** | `hackernews` | news.ycombinator.com | AI 相关讨论 | API |

### Tier 2: 应采 (P1)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **Futurepedia** | `futurepedia` | futurepedia.io | AI 工具目录 | Web爬虫 |
| **Papers With Code** | `paperswithcode` | paperswithcode.com | AI 论文、代码 | API |

### WebFetch Prompts

**There's An AI For That**
```
URL: https://theresanaiforthat.com/ai/{tool_name}/
Extract: description, category, pricing, saves count, launch date,
features list, alternatives, user reviews
```

**Hugging Face**
```
API: https://huggingface.co/api/spaces?sort=likes
Extract: space name, author, likes, description, tags,
last updated, runtime, sdk
```

**GitHub Trending**
```
URL: https://github.com/trending?since=daily
Extract: repo name, description, stars, forks, language,
stars today, contributors
```

---

## 五、通用平台 (7个)

> 来源: 关键词挖掘系统 `config/platforms.yaml`

### Tier 1: 必采 (P0)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **Google Trends** | `google_trends` | trends.google.com | 搜索趋势、暴涨词 | pytrends |
| **YouTube** | `youtube` | youtube.com | 视频热度、搜索建议 | API |
| **Google Suggest** | `google_suggest` | suggestqueries.google.com | 搜索建议 | Web请求 |
| **Reddit** | `reddit` | reddit.com | 多 subreddit 聚合 | API |

### Tier 2: 应采 (P1)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **IndieHackers** | `indiehackers` | indiehackers.com | 产品讨论、创业者视角 | Web爬虫 |

### Tier 3: 选采 (P2)

| 平台 | ID | URL | 采集内容 | 方式 |
|------|-----|-----|----------|------|
| **Quora** | `quora` | quora.com | 问题趋势 | Web爬虫 |
| **Wikipedia** | `wikipedia` | wikipedia.org | 页面浏览量、词条信息 | API |

### WebFetch Prompts

**Google Trends**
```python
# 使用 pytrends 库
from pytrends.request import TrendReq
pytrends = TrendReq(hl='en-US', tz=360)
pytrends.build_payload([keyword], timeframe='today 12-m')

# 获取数据
interest_over_time = pytrends.interest_over_time()
related_queries = pytrends.related_queries()
interest_by_region = pytrends.interest_by_region()
```

**YouTube Data API**
```
Endpoint: https://www.googleapis.com/youtube/v3/search
Parameters: q={keyword}, type=video, order=viewCount, maxResults=50
Extract: video title, channel, views, likes, published date, description
```

---

## 六、Reddit Subreddits (15个)

> 来源: 关键词挖掘系统 `config/subreddits.yaml`

### 游戏类 (7个)

| Subreddit | 成员数 | 关注内容 | 采集量 |
|-----------|--------|----------|--------|
| r/gaming | 3500万 | 通用游戏讨论 | hot:50, new:30 |
| r/indiegaming | 50万 | 独立游戏 | hot:50, new:30 |
| r/WebGames | 30万 | 网页游戏 | hot:50, new:30 |
| r/pcgaming | 300万 | PC 游戏 | hot:50, new:30 |
| r/Games | 350万 | 游戏新闻讨论 | hot:50, new:30 |
| r/incremental_games | 20万 | 放置/增量游戏 | hot:30, new:20 |
| r/gamedev | 100万 | 游戏开发 | hot:30, new:20 |

### 工具类 (5个)

| Subreddit | 成员数 | 关注内容 | 采集量 |
|-----------|--------|----------|--------|
| r/InternetIsBeautiful | 1700万 | 优质网站/工具 | hot:50, new:30 |
| r/webapps | 10万 | 网页应用 | hot:50, new:30 |
| r/SideProject | 15万 | 个人项目 | hot:50, new:30 |
| r/selfhosted | 30万 | 自托管工具 | hot:50, new:30 |
| r/Productivity | 200万 | 效率工具 | hot:50, new:30 |

### AI 类 (6个)

| Subreddit | 成员数 | 关注内容 | 采集量 |
|-----------|--------|----------|--------|
| r/ChatGPT | 500万 | ChatGPT 使用 | hot:50, new:30 |
| r/StableDiffusion | 100万 | 图像生成 AI | hot:50, new:30 |
| r/LocalLLaMA | 50万 | 本地 LLM | hot:50, new:30 |
| r/artificial | 200万 | 通用 AI | hot:50, new:30 |
| r/MachineLearning | 300万 | 机器学习 | hot:30, new:20 |
| r/singularity | 100万 | AI 未来 | hot:30, new:20 |

### Reddit API 配置

```python
# 需要 OAuth credentials
# Env: REDDIT_CLIENT_ID, REDDIT_CLIENT_SECRET

import praw
reddit = praw.Reddit(
    client_id=os.getenv('REDDIT_CLIENT_ID'),
    client_secret=os.getenv('REDDIT_CLIENT_SECRET'),
    user_agent='market-intel-collector/1.0'
)

# 获取 subreddit 热帖
subreddit = reddit.subreddit('gaming')
for post in subreddit.hot(limit=50):
    # post.title, post.score, post.num_comments, post.url
```

---

## 七、情报专用源 (新增 12个)

> **关键词挖掘系统没有，情报 Skill 必须补充**

### 7.1 法律风险源 (3个) 🔴 关键

| 平台 | ID | URL | 用途 | 方式 |
|------|-----|-----|------|------|
| **Lumen Database** | `lumen` | lumendatabase.org | DMCA takedown 记录 | Web搜索 |
| **版权方官网** | `ip_holder` | 各 IP 持有方 | 官方立场、法律声明 | WebFetch |
| **法律新闻** | `legal_news` | Google News | 诉讼动态 | WebSearch |

**Lumen Database 查询**
```
URL: https://lumendatabase.org/notices/search?term={keyword}
Extract: notice count, sender names, takedown dates, URLs affected
用途: 判断 IP 是否有侵权历史，评估法律风险
```

**版权方态度调查模板**
```
Search: "{original_product}" official "fan content" OR "fan mod" policy
Search: "{company_name}" DMCA OR takedown OR copyright
Extract: 官方对 fan content 的立场 (允许/禁止/灰色地带)
```

### 7.2 竞争情报源 (3个) 🔴 关键

| 平台 | ID | URL | 用途 | 方式 |
|------|-----|-----|------|------|
| **SimilarWeb** | `similarweb` | similarweb.com | 竞争对手流量 | Web/API |
| **Ahrefs** | `ahrefs` | ahrefs.com | 关键词难度、外链 | 手动 |
| **Wayback Machine** | `wayback` | web.archive.org | 竞争对手历史 | API |

**SimilarWeb 查询**
```
URL: https://www.similarweb.com/website/{domain}/
Extract: monthly visits, traffic sources, top countries,
bounce rate, pages per visit, avg visit duration,
top referring sites, top destination sites
限制: 免费版数据有限，可用 WebSearch 获取部分数据
```

**Wayback Machine API**
```
API: https://web.archive.org/cdx/search/cdx?url={domain}&output=json
用途: 查看竞争对手何时进入、页面历史变化
```

### 7.3 安全评估源 (2个)

| 平台 | ID | URL | 用途 | 方式 |
|------|-----|-----|------|------|
| **Common Sense Media** | `commonsense` | commonsensemedia.org | 儿童内容安全评级 | WebFetch |
| **eSafety Guide** | `esafety` | esafety.gov.au | 澳洲政府安全评估 | WebFetch |

**Common Sense Media 查询**
```
URL: https://www.commonsensemedia.org/search/{keyword}
Extract: age rating, parent rating, kid rating,
educational value, violence level, language concerns,
parent review quotes, detailed breakdown
用途: "is X safe for kids" 类内容的权威来源
```

### 7.4 实时舆情源 (4个) 🟡 重要

| 平台 | ID | URL | 用途 | 方式 |
|------|-----|-----|------|------|
| **Twitter/X** | `twitter` | twitter.com | 实时讨论、创作者动态 | WebSearch |
| **TikTok** | `tiktok` | tiktok.com | 病毒传播源头 | WebSearch |
| **Discord** | `discord` | discord.com | 核心社区动态 | 手动 |
| **Twitch** | `twitch` | twitch.tv | 游戏直播热度 | API |

**Twitter/X 搜索**
```
Search: "{keyword}" -is:retweet lang:en since:2025-01-01
Search: from:{creator_handle} "{keyword}"
Extract: tweet content, engagement (likes/retweets/replies),
sentiment, key influencers discussing
限制: API 受限，主要通过 WebSearch
```

**TikTok 搜索**
```
Search: site:tiktok.com "{keyword}"
Search: tiktok "{keyword}" viral
Extract: 估算视频数、热门创作者、标签使用量
用途: Sprunki 案例证明 TikTok 是病毒传播关键节点
```

**Discord 调查**
```
Search: "{keyword}" discord server
Search: site:discord.gg "{keyword}"
用途: 找到官方/粉丝 Discord，评估社区规模和活跃度
限制: 需要手动加入服务器获取详细数据
```

---

## 八、数据源优先级矩阵

### 按报告类型

| 源类型 | Flash Report | Full Report | 竞品分析 |
|--------|--------------|-------------|----------|
| 官网 | 必采 | 必采 | 必采 |
| 游戏/工具平台 | 必采 | 必采 | 必采 |
| Google Trends | 必采 | 必采 | 选采 |
| YouTube | 选采 | 必采 | 选采 |
| Reddit | 选采 | 必采 | 应采 |
| 法律风险源 | - | 应采 | 必采 |
| 竞争情报源 | - | 选采 | 必采 |
| 安全评估源 | - | 按需 | - |
| 实时舆情源 | 选采 | 应采 | 选采 |

### 按关键词类型

| 源类型 | 游戏类 | 工具类 | AI类 |
|--------|--------|--------|------|
| Steam/itch.io | 必采 | - | - |
| Metacritic/OpenCritic | 应采 | - | - |
| Product Hunt | - | 必采 | 应采 |
| G2/Capterra | - | 必采 | 选采 |
| TAAFT/Futurepedia | - | - | 必采 |
| HuggingFace | - | - | 必采 |
| Common Sense Media | 按需 | - | - |

---

## 九、采集脚本位置

### 关键词挖掘系统 (可复用)

```
/Users/suifeng/python/2026/seo-tools/关键词挖掘/
├── src/collectors/
│   ├── gaming/          # 游戏类 10 个采集器
│   │   ├── steam.py
│   │   ├── itch_io.py
│   │   └── ...
│   ├── tools/           # 工具类 9 个采集器
│   │   ├── producthunt.py
│   │   ├── alternativeto.py
│   │   └── ...
│   ├── ai/              # AI类 6 个采集器
│   │   ├── theresanai.py
│   │   ├── huggingface.py
│   │   └── ...
│   └── general/         # 通用 7 个采集器
│       ├── google_trends.py
│       ├── youtube.py
│       ├── reddit.py
│       └── ...
└── config/
    ├── platforms.yaml   # 平台配置
    └── subreddits.yaml  # Reddit 配置
```

### API Keys 配置

```
/Users/suifeng/python/2026/seo-tools/关键词挖掘/.env

YOUTUBE_API_KEY=xxx
REDDIT_CLIENT_ID=xxx
REDDIT_CLIENT_SECRET=xxx
TWITCH_CLIENT_ID=xxx
TWITCH_CLIENT_SECRET=xxx
```

---

## 十、失败处理策略

### 降级链

```
WebFetch 失败
    ↓
等待 3 秒重试
    ↓
仍失败 → Playwright
    ↓
仍失败 → 替代源
    ↓
无替代 → 标记 [BLOCKED] 或 [NOT_FOUND]
```

### 替代源映射

| 失败源 | 替代方案 |
|--------|----------|
| Steam | Epic Games Store, GOG, 官网 |
| itch.io | Game Jolt, Newgrounds, WebSearch |
| Metacritic | OpenCritic, IGN |
| G2 | Capterra, TrustRadius |
| Product Hunt | Crunchbase, 官网 |
| Reddit | Twitter/X, Discord, 官方论坛 |
| Google Trends | Exploding Topics, 手动趋势图 |

### 错误标记规范

```markdown
[NOT_FOUND: {具体原因}]      # 数据不存在
[FETCH_FAILED: {URL}]        # 采集失败
[LOGIN_REQUIRED: {URL}]      # 需要登录
[BLOCKED: {URL}]             # 被反爬阻止
[RATE_LIMITED: {平台}]       # 触发限流
[ESTIMATE: {估算依据}]       # 估算数据
```

---

## 十一、数据源统计

### 总计

| 类别 | 平台数 | 来源 |
|------|--------|------|
| 游戏平台 | 10 | 关键词系统 |
| 工具平台 | 9 | 关键词系统 |
| AI平台 | 6 | 关键词系统 |
| 通用平台 | 7 | 关键词系统 |
| Reddit Subreddits | 15 | 关键词系统 |
| 法律风险源 | 3 | 情报专用 (新增) |
| 竞争情报源 | 3 | 情报专用 (新增) |
| 安全评估源 | 2 | 情报专用 (新增) |
| 实时舆情源 | 4 | 情报专用 (新增) |
| **总计** | **59 + 15** | |

### 覆盖率提升

| 维度 | v1.0 | v2.0 | 提升 |
|------|------|------|------|
| 游戏平台 | 3 | 10 | +233% |
| 工具平台 | 4 | 9 | +125% |
| AI平台 | 0 | 6 | 从0到6 |
| Reddit 结构化 | 0 | 15 | 从0到15 |
| 法律风险 | 0 | 3 | 从0到3 |
| 竞争情报 | 0 | 3 | 从0到3 |
| **综合覆盖率** | ~25% | ~75% | +200% |

---

*数据源参考文档 - market-intel-collector skill*
*版本: 2.0.0*
*整合自: 关键词挖掘系统 v3.12*

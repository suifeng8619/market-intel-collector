---
name: market-intel-collector
description: Collect market intelligence for games and tools in international (English) markets. Use when user asks to "collect info about", "research", "investigate", or "gather data on" a game or tool product. Outputs AI-friendly structured Markdown reports for SEO analysis and content creation.
---

# Market Intelligence Collector

Collect comprehensive market information for games and tools targeting international English-speaking markets.

## When to Use

- User mentions "collect", "research", "investigate", "gather" + product name
- User asks about market information, user reviews, or competitive intelligence
- User needs SEO content material or competitive analysis data
- User provides a product name and wants to understand market reception

## Input Requirements

1. **Keyword** (required): Product name, e.g., "Minecraft", "Notion"
2. **Type** (required): `game` or `tool`
3. **Background Info** (optional): Official URL, category, target users, etc.

## Workflow

### Phase 0: Confirm Input

If type is not provided, ask:
```
Is this a game or a tool? This determines which data sources I'll use.
```

If keyword is ambiguous (common name), ask for clarification:
```
There are multiple products named "{keyword}". Can you provide more context or the official website?
```

### Phase 1: Core Information Collection (Required)

**1.1 Search for Official Sources**

Run these WebSearch queries in parallel:

For **games**:
```
WebSearch: "{keyword} official site"
WebSearch: "{keyword} steam"
WebSearch: "{keyword} metacritic"
WebSearch: "site:reddit.com {keyword} review"
WebSearch: "{keyword} ign review"
```

For **tools**:
```
WebSearch: "{keyword} official site"
WebSearch: "{keyword} g2 reviews"
WebSearch: "{keyword} product hunt"
WebSearch: "site:reddit.com {keyword}"
WebSearch: "{keyword} vs alternatives"
```

**1.2 Fetch Key Pages**

Use WebFetch to extract detailed information from discovered URLs.

For each URL, use appropriate extraction prompts:

**Official Website:**
```
WebFetch prompt: "Extract: product name, tagline, main description,
key features (list), pricing information, supported platforms,
target audience, company/developer name"
```

**Steam (games):**
```
WebFetch prompt: "Extract: game title, description, release date,
developer, publisher, price, overall review sentiment and percentage,
total reviews count, recent review sentiment, tags/genres, platforms"
```

**G2/Capterra (tools):**
```
WebFetch prompt: "Extract: product name, overall rating, total reviews,
pros mentioned by users (top 5), cons mentioned by users (top 5),
best for (user types), pricing tier"
```

**Metacritic (games):**
```
WebFetch prompt: "Extract: metascore, user score, critic reviews count,
user reviews count, summary of critical reception"
```

**Product Hunt (tools):**
```
WebFetch prompt: "Extract: product tagline, description, upvotes count,
reviews count, maker information, launch date, featured comments"
```

### Phase 2: User Sentiment Collection

**2.1 Reddit Discussions**

From Reddit search results, fetch 2-3 most relevant discussion threads:
```
WebFetch prompt: "Extract: main topic, top comments (summarize sentiment),
common praise points, common complaints, use cases mentioned"
```

**2.2 Review Aggregation**

Compile ratings from multiple sources into a comparison table.

### Phase 3: Media Coverage (Games) / Professional Reviews (Tools)

**For games:**
```
WebSearch: "{keyword} review ign"
WebSearch: "{keyword} review gamespot"
WebSearch: "{keyword} review polygon"
```

**For tools:**
```
WebSearch: "{keyword} review techcrunch"
WebSearch: "{keyword} review {category} best"
```

Extract: publication name, rating/score, key quote, reviewer's verdict.

### Phase 4: Visual Resources

```
WebSearch: "{keyword} official trailer youtube" (games)
WebSearch: "{keyword} demo video youtube" (tools)
WebSearch: "{keyword} screenshots official"
```

Extract: YouTube video URLs, official image URLs when available.

### Phase 5: Generate Structured Report

Use the appropriate template:
- Games: `templates/game-report.md`
- Tools: `templates/tool-report.md`

Fill in all sections. For missing data, use format:
```
[NOT FOUND: {reason}]
```

## Fallback Strategies

### WebFetch Failures

If WebFetch fails for a URL:

1. **First attempt**: Retry once after 3 seconds
2. **Second attempt**: Use Playwright MCP
   ```
   mcp__playwright__browser_navigate(url)
   mcp__playwright__browser_snapshot()
   ```
3. **Third attempt**: Mark as `[FETCH_FAILED: {URL}]` and continue

### Data Not Found

| Situation | Action |
|-----------|--------|
| Game not on Steam | Search Epic Games Store, GOG |
| Tool not on G2 | Search Capterra, TrustRadius |
| No Reddit discussions | Note "Limited community discussion found" |
| No media reviews | May indicate niche product, note this |

### Disambiguation

If search results show multiple products with same name:
- Add type qualifier: `"{keyword}" game` or `"{keyword}" software`
- Add developer/company name if known
- Ask user for clarification if still ambiguous

## Output Format

### Structure Requirements

1. **All data must have source URLs** - Every piece of information needs a clickable source
2. **Use tables for structured data** - Ratings, features, comparisons
3. **Use blockquotes for direct quotes** - Official descriptions, user reviews
4. **Mark missing data explicitly** - `[NOT FOUND]` with reason
5. **Include collection timestamp** - Data may become outdated

### Language

- Collect data in English (original)
- Keep quotes and descriptions in English
- Output report in English for AI processing

## Key Principles

1. **Accuracy over completeness** - Don't fabricate data; mark gaps clearly
2. **Source everything** - Every claim needs a URL
3. **Respect rate limits** - Don't spam requests to same domain
4. **Handle failures gracefully** - One failed source shouldn't stop the whole process
5. **Prioritize official sources** - Official > user reviews > media > forums

## Advanced Data Collection

### Using Existing Scripts (关键词挖掘项目)

Located at: `/Users/suifeng/python/2026/seo-tools/关键词挖掘/`

**Scripts that DON'T need API keys** (can be used directly):

| Script | Path | Data Type | How to Use |
|--------|------|-----------|------------|
| Google Trends | `src/collectors/general/google_trends.py` | Interest over time, related queries | Uses `pytrends` library |
| Google Suggest | `src/collectors/general/google_suggest.py` | Autocomplete suggestions | Web scraping |
| Steam | `src/collectors/gaming/steam.py` | Concurrent players, reviews | Web scraping |
| itch.io | `src/collectors/gaming/itch_io.py` | Games, ratings, downloads | Web scraping |

**Scripts that NEED API keys** (check `.env` first):

| Script | Path | API Required | Env Variable |
|--------|------|--------------|--------------|
| YouTube | `src/collectors/general/youtube.py` | YouTube Data API v3 | `YOUTUBE_API_KEY` |
| Reddit | `src/collectors/general/reddit.py` | Reddit API | `REDDIT_CLIENT_ID`, `REDDIT_CLIENT_SECRET` |
| Twitch | `src/collectors/gaming/twitch.py` | Twitch API | `TWITCH_CLIENT_ID`, `TWITCH_CLIENT_SECRET` |

**Usage Pattern:**
```bash
# Check if API keys are configured
cat /Users/suifeng/python/2026/seo-tools/关键词挖掘/.env

# Run a specific collector (example)
cd /Users/suifeng/python/2026/seo-tools/关键词挖掘
python -m src.collectors.gaming.steam --keyword "{keyword}"
```

### Using Playwright for Direct Platform Access

When WebFetch fails or you need real-time data, use Playwright MCP:

**Google Trends (Essential for timeline data):**
```
mcp__playwright__browser_navigate({ url: "https://trends.google.com/trends/explore?q={keyword}&hl=en" })
mcp__playwright__browser_wait_for({ time: 3 })
mcp__playwright__browser_snapshot()
```

Extract from snapshot: interest over time values, geographic breakdown, related queries.

**itch.io (For indie/web games):**
```
mcp__playwright__browser_navigate({ url: "https://itch.io/search?q={keyword}" })
mcp__playwright__browser_wait_for({ time: 2 })
mcp__playwright__browser_snapshot()
```

Extract: game titles, ratings, number of ratings, authors.

**Steam Store (When WebFetch fails):**
```
mcp__playwright__browser_navigate({ url: "https://store.steampowered.com/search/?term={keyword}" })
mcp__playwright__browser_wait_for({ time: 2 })
mcp__playwright__browser_snapshot()
```

**Always close browser after collection:**
```
mcp__playwright__browser_close()
```

### Platform-Specific Notes

| Product Type | Key Platforms | Notes |
|--------------|---------------|-------|
| Steam Games | Steam, Metacritic, Reddit, YouTube | Use `steam.py` script for player counts |
| Web/Browser Games | itch.io, CrazyGames, Newgrounds | Playwright preferred for these |
| Mobile Games | App Store, Google Play, TapTap | WebFetch usually works |
| SaaS Tools | G2, Capterra, Product Hunt | API access limited, use WebFetch |
| Open Source | GitHub, npm, PyPI | GitHub API available in scripts |

### Viral/Trend Analysis (Games)

For understanding HOW a game went viral:

1. **Google Trends** - When did interest spike?
2. **Know Your Meme** - Search `site:knowyourmeme.com "{keyword}"`
3. **Fandom Wiki** - Version history, timeline
4. **TikTok** - Manual search (no API), note top hashtag views
5. **YouTube** - Sort by upload date + view count to find first viral videos

## Integration with Other Skills

After collection, user may want to:
- Use `seo-page-auditor` to audit the official website
- Use `seo-performance-analyzer` for deeper SEO analysis
- Use `web-performance` to check site performance

## Example Usage

**Simple:**
```
User: Collect market info for Hades, it's a game
Assistant: [Executes collection workflow, outputs game report]
```

**With context:**
```
User: Research Notion - it's a productivity tool, official site is notion.so
Assistant: [Executes collection with known URL, outputs tool report]
```

**Ambiguous:**
```
User: Collect info on Spark
Assistant: There are multiple products named "Spark" - could you specify
which one? For example: Spark email app, Apache Spark, Spark AR, etc.
```

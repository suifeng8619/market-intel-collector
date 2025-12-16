# Data Sources Reference

Quick reference for data sources, URL patterns, and extraction strategies.

---

## Game Data Sources

### Steam Store
- **URL Pattern**: `https://store.steampowered.com/app/{app_id}/{game_name}/`
- **Finding URL**: Search `"{game_name}" site:store.steampowered.com`
- **WebFetch Prompt**:
  ```
  Extract: game title, short description, long description, release date,
  developer, publisher, current price, original price (if on sale),
  overall review sentiment (e.g., "Very Positive"), overall review percentage,
  total review count, recent review sentiment, recent review percentage,
  user-defined tags (all), supported platforms (Windows/Mac/Linux),
  supported languages, system requirements summary
  ```
- **Fallback**: Use Playwright if age gate appears

### Metacritic
- **URL Pattern**: `https://www.metacritic.com/game/{game_name}/`
- **Finding URL**: Search `"{game_name}" site:metacritic.com game`
- **WebFetch Prompt**:
  ```
  Extract: metascore (critic score out of 100), user score (out of 10),
  critic reviews count, user reviews count, summary text,
  positive critic quotes (2-3), mixed/negative critic quotes (1-2)
  ```

### IGN
- **URL Pattern**: `https://www.ign.com/articles/{game_name}-review`
- **Finding URL**: Search `"{game_name}" review site:ign.com`
- **WebFetch Prompt**:
  ```
  Extract: score (out of 10), verdict/summary, pros list, cons list,
  reviewer name, review date, key quote
  ```

### OpenCritic
- **URL Pattern**: `https://opencritic.com/game/{id}/{game_name}`
- **Finding URL**: Search `"{game_name}" site:opencritic.com`
- **WebFetch Prompt**:
  ```
  Extract: top critic average, critics recommend percentage,
  tier (e.g., "Mighty"), number of reviews, summary
  ```

### Reddit (Games)
- **Search Pattern**: `site:reddit.com "{game_name}" (review OR "worth it" OR opinions)`
- **Subreddit Pattern**: `r/{game_name}` or `r/gaming` or genre-specific
- **WebFetch Prompt**:
  ```
  Extract: post title, upvotes, top 5 comments (summarize sentiment),
  common themes in discussion, specific praise points, specific complaints
  ```

### YouTube (Game Trailers/Reviews)
- **Search Pattern**: `"{game_name}" official trailer site:youtube.com`
- **Search Pattern**: `"{game_name}" review site:youtube.com`
- **Extract**: Video title, channel name, view count, URL

---

## Tool Data Sources

### G2
- **URL Pattern**: `https://www.g2.com/products/{tool_name}/reviews`
- **Finding URL**: Search `"{tool_name}" site:g2.com reviews`
- **WebFetch Prompt**:
  ```
  Extract: overall rating (out of 5), total reviews count,
  ease of use rating, quality of support rating, ease of setup rating,
  top 5 "what users like" themes, top 5 "what users dislike" themes,
  most helpful positive review quote, most helpful negative review quote,
  "best for" categories, company size breakdown of reviewers
  ```

### Capterra
- **URL Pattern**: `https://www.capterra.com/p/{id}/{tool_name}/`
- **Finding URL**: Search `"{tool_name}" site:capterra.com reviews`
- **WebFetch Prompt**:
  ```
  Extract: overall rating (out of 5), total reviews count,
  ease of use rating, customer service rating, value for money rating,
  likelihood to recommend percentage, pros summary, cons summary,
  reviewer industries breakdown
  ```

### Product Hunt
- **URL Pattern**: `https://www.producthunt.com/products/{tool_name}`
- **Finding URL**: Search `"{tool_name}" site:producthunt.com`
- **WebFetch Prompt**:
  ```
  Extract: tagline, description, total upvotes, launch date,
  maker/team information, featured reviews, topics/categories,
  related products, awards/badges if any
  ```

### TrustRadius
- **URL Pattern**: `https://www.trustradius.com/products/{tool_name}/reviews`
- **Finding URL**: Search `"{tool_name}" site:trustradius.com`
- **WebFetch Prompt**:
  ```
  Extract: trScore (out of 10), total reviews, pros themes,
  cons themes, best for use cases, alternatives mentioned
  ```

### GitHub (Open Source Tools)
- **URL Pattern**: `https://github.com/{org}/{repo}`
- **Finding URL**: Search `"{tool_name}" site:github.com`
- **WebFetch Prompt**:
  ```
  Extract: stars count, forks count, open issues count,
  last commit date, license, primary language,
  README summary (first 500 chars), contributors count
  ```

### Reddit (Tools)
- **Search Pattern**: `site:reddit.com "{tool_name}" (review OR alternative OR vs)`
- **Relevant Subreddits**: r/SaaS, r/startups, r/productivity, r/webdev, etc.
- **WebFetch Prompt**:
  ```
  Extract: post title, upvotes, key discussion points,
  mentioned alternatives, common use cases, complaints
  ```

---

## General Sources

### Official Website
- **Finding URL**: Search `"{product_name}" official site` or `"{product_name}" official website`
- **WebFetch Prompt**:
  ```
  Extract: company/product name, tagline, main description,
  key features (list all mentioned), pricing information,
  supported platforms, target audience description,
  social proof (customer logos, testimonials), contact info
  ```

### Wikipedia
- **URL Pattern**: `https://en.wikipedia.org/wiki/{Product_Name}`
- **Finding URL**: Search `"{product_name}" site:wikipedia.org`
- **WebFetch Prompt**:
  ```
  Extract: summary paragraph, developer/company, release date,
  genre/category, platform, reception summary, awards
  ```

### Crunchbase (Company Info)
- **URL Pattern**: `https://www.crunchbase.com/organization/{company_name}`
- **WebFetch Prompt**:
  ```
  Extract: company description, founding date, headquarters,
  funding total, latest funding round, employee count, founders
  ```

---

## Fallback Strategies

### When WebFetch Fails

**Step 1: Retry**
```
Wait 3 seconds, retry WebFetch with same parameters
```

**Step 2: Playwright Fallback**
```javascript
// Navigate to page
mcp__playwright__browser_navigate({ url: targetUrl })

// Wait for content to load
mcp__playwright__browser_wait_for({ time: 3 })

// Get page snapshot
mcp__playwright__browser_snapshot()

// Extract data from snapshot manually
```

**Step 3: Alternative Sources**

| Failed Source | Alternative |
|---------------|-------------|
| Steam | Epic Games Store, GOG, official site |
| Metacritic | OpenCritic, IGN aggregate |
| G2 | Capterra, TrustRadius |
| Product Hunt | Crunchbase, official site |
| Reddit | Twitter/X, official forums |

### Common Failure Reasons

| Error | Likely Cause | Solution |
|-------|--------------|----------|
| 403 Forbidden | Anti-bot protection | Use Playwright |
| 404 Not Found | Wrong URL | Re-search with different query |
| Timeout | Slow page/heavy JS | Increase timeout, use Playwright |
| Cloudflare | Bot detection | Mark as [BLOCKED], try alternative |
| Age Gate | Content restriction | Use Playwright to handle |
| Login Required | Gated content | Mark as [LOGIN_REQUIRED] |

---

## Search Query Templates

### Game Queries
```
"{game_name}" official site
"{game_name}" steam store
"{game_name}" metacritic
"{game_name}" review ign
"{game_name}" review gamespot
site:reddit.com "{game_name}" worth buying
"{game_name}" trailer youtube
"{game_name}" gameplay
```

### Tool Queries
```
"{tool_name}" official website
"{tool_name}" g2 reviews
"{tool_name}" capterra
"{tool_name}" product hunt
"{tool_name}" pricing
"{tool_name}" vs alternatives
"{tool_name}" vs {competitor}
site:reddit.com "{tool_name}" review
"{tool_name}" tutorial youtube
"{tool_name}" github
```

### Disambiguation Queries
```
"{product_name}" game 2024
"{product_name}" software tool
"{product_name}" by {company}
"{product_name}" {specific_platform}
```

---

## Rate Limiting Guidelines

- **Same domain**: Wait 2 seconds between requests
- **WebFetch failures**: Wait 5 seconds before retry
- **Playwright**: Close browser after each session
- **Maximum per source**: 3 pages per domain per collection

---

## Local Scripts (关键词挖掘项目)

Scripts located at: `/Users/suifeng/python/2026/seo-tools/关键词挖掘/`

### No API Key Required

**Google Trends (`src/collectors/general/google_trends.py`)**
```python
# Uses pytrends library
# Returns: interest over time, related queries, geographic data
from pytrends.request import TrendReq
pytrends = TrendReq(hl='en-US', tz=360)
pytrends.build_payload([keyword], cat=0, timeframe='today 12-m')
interest_over_time = pytrends.interest_over_time()
```

**itch.io (`src/collectors/gaming/itch_io.py`)**
```python
# Web scraping approach
# Returns: games list, ratings, downloads, authors
# URL pattern: https://itch.io/search?q={keyword}
```

**Steam (`src/collectors/gaming/steam.py`)**
```python
# Uses Steam Web API (public endpoints)
# Returns: concurrent players, review scores, tags
# Key endpoints:
#   - https://store.steampowered.com/api/appdetails?appids={app_id}
#   - https://steamcharts.com/app/{app_id} (for player history)
```

**Google Suggest (`src/collectors/general/google_suggest.py`)**
```python
# Scrapes Google autocomplete
# Returns: suggested queries for keyword
# URL: http://suggestqueries.google.com/complete/search?client=firefox&q={keyword}
```

### API Key Required

Check `/Users/suifeng/python/2026/seo-tools/关键词挖掘/.env` for configured keys.

**YouTube (`src/collectors/general/youtube.py`)**
- Env: `YOUTUBE_API_KEY`
- Returns: video search results, view counts, channel info

**Reddit (`src/collectors/general/reddit.py`)**
- Env: `REDDIT_CLIENT_ID`, `REDDIT_CLIENT_SECRET`
- Returns: posts, comments, subreddit stats

**Twitch (`src/collectors/gaming/twitch.py`)**
- Env: `TWITCH_CLIENT_ID`, `TWITCH_CLIENT_SECRET`
- Returns: streams, clips, game categories

---

## Output Quality Checks

Before finalizing report:

1. [ ] All ratings have source URLs
2. [ ] Quotes are in quotation marks with attribution
3. [ ] Missing data marked as `[NOT FOUND: reason]`
4. [ ] No placeholder text remaining
5. [ ] Collection date is accurate
6. [ ] All URLs are valid (not 404)

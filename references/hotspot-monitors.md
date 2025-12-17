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

*Hotspot monitoring reference for market-intel-collector skill*

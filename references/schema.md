# Input/Output Schema (输入输出规范)

Standard data contract for market-intel-collector to integrate with upstream and downstream systems.

---

## Input Schema

### Request Format

```json
{
  "keyword": "string (required)",
  "type": "game | tool (required)",
  "mode": "flash | full (default: full)",
  "priority": "P0 | P1 | P2 | P3 (default: P1)",
  "deadline": "ISO8601 datetime (optional)",
  "context": {
    "official_url": "string (optional)",
    "category": "string (optional)",
    "known_competitors": ["string"] (optional),
    "target_regions": ["string"] (optional)
  }
}
```

### Example Inputs

**Minimal:**
```json
{
  "keyword": "Sprunki",
  "type": "game"
}
```

**With Context:**
```json
{
  "keyword": "Notion",
  "type": "tool",
  "mode": "full",
  "priority": "P0",
  "context": {
    "official_url": "https://notion.so",
    "category": "productivity",
    "known_competitors": ["Obsidian", "Roam Research", "Coda"]
  }
}
```

**Flash Mode:**
```json
{
  "keyword": "Palworld",
  "type": "game",
  "mode": "flash",
  "priority": "P0",
  "deadline": "2024-01-20T12:00:00Z"
}
```

---

## Output Schema

### Report Metadata

```json
{
  "meta": {
    "keyword": "string",
    "type": "game | tool",
    "mode": "flash | full",
    "collection_date": "ISO8601 datetime",
    "collection_duration_seconds": "number",
    "data_completeness": "0-100%",
    "sources_used": "number",
    "sources_failed": "number"
  }
}
```

### Battle Assessment Output

```json
{
  "battle_assessment": {
    "factors": {
      "dao_user_pain": {
        "score": "1-10",
        "evidence": "string"
      },
      "tian_timing": {
        "score": "1-10",
        "evidence": "string"
      },
      "di_competition": {
        "score": "1-10",
        "evidence": "string"
      },
      "jiang_team": {
        "score": "1-10",
        "evidence": "string"
      },
      "fa_product": {
        "score": "1-10",
        "evidence": "string"
      }
    },
    "battle_index": {
      "score": "1-10 (weighted average)",
      "status": "GO | CAUTION | NO-GO"
    },
    "verdict": "string (one sentence)"
  }
}
```

### Executive Recommendations Output

```json
{
  "recommendations": {
    "opportunity_window": "open | narrowing | closed",
    "competition_intensity": "low | medium | high",
    "content_gaps": ["string"],
    "actions": {
      "immediate": ["string"],
      "short_term": ["string"],
      "hold_conditions": ["string"]
    },
    "risks": [
      {
        "description": "string",
        "probability": "high | medium | low",
        "impact": "high | medium | low",
        "mitigation": "string"
      }
    ],
    "roi_estimate": {
      "monthly_search_volume": "number | null",
      "competition_level": "low | medium | high",
      "expected_traffic_month3": "number | null",
      "content_investment_hours": "number",
      "priority_rating": "P0 | P1 | P2 | P3"
    },
    "final_verdict": {
      "decision": "PROCEED | PROCEED_WITH_CAUTION | SKIP",
      "summary": "string (one paragraph)"
    }
  }
}
```

### Full Report Structure (Sections)

```json
{
  "sections": {
    "basic_info": { /* ... */ },
    "official_description": { /* ... */ },
    "ratings_summary": { /* ... */ },
    "user_sentiment": {
      "positive": [{ "theme": "", "frequency": "", "quote": "", "source_url": "" }],
      "negative": [{ "theme": "", "frequency": "", "quote": "", "source_url": "" }]
    },
    "media_reviews": [{ "publication": "", "score": "", "verdict": "", "quote": "", "url": "" }],
    "visual_resources": {
      "trailers": [{ "title": "", "views": "", "url": "" }],
      "screenshots": [{ "description": "", "url": "" }]
    },
    "community": {
      "reddit": { "subreddit": "", "subscribers": "", "activity": "" },
      "hot_topics": [{ "title": "", "upvotes": "", "url": "" }]
    },
    "trend_data": {
      "google_trends": {
        "peak_date": "",
        "peak_index": 100,
        "current_index": "",
        "trend_direction": "rising | stable | declining",
        "top_regions": [""],
        "related_queries": [{ "query": "", "change": "" }]
      }
    },
    "competitive_landscape": {
      "similar_products": [{ "name": "", "similarity": "", "difference": "" }],
      "market_position": ""
    },
    "seo_insights": {
      "informational_queries": [""],
      "transactional_queries": [""],
      "navigational_queries": [""],
      "content_angles": [""],
      "longtail_ideas": [""]
    },
    "data_gaps": [{ "item": "", "status": "NOT_FOUND", "reason": "" }],
    "battle_assessment": { /* see above */ },
    "recommendations": { /* see above */ }
  }
}
```

---

## Source Attribution Format

Every data point must include source URL using `<source>` tags:

```markdown
<source url="https://example.com/page">
Content extracted from this source goes here.
Multiple lines are fine.
</source>
```

For JSON output:
```json
{
  "data": "value",
  "source_url": "https://example.com/page"
}
```

---

## Integration Protocols

### Upstream: Keyword Discovery System

**Expected handoff:**
```json
{
  "discovered_keywords": [
    {
      "keyword": "string",
      "type": "game | tool",
      "discovery_source": "steam | producthunt | trends | etc",
      "discovery_signal": "string",
      "suggested_priority": "P0 | P1 | P2 | P3"
    }
  ]
}
```

### Downstream: Content Creation AI

**Report consumption pattern:**
1. Parse report markdown
2. Extract `<source url="...">` blocks
3. Use content for article generation
4. Preserve source URLs for citations

**Expected interface:**
```json
{
  "report_file": "path/to/report.md",
  "target_content_type": "review | guide | comparison | listicle",
  "word_count_target": 2000,
  "citation_style": "inline | footnote | endnotes"
}
```

---

## Error Handling

### Error Response Format

```json
{
  "error": {
    "code": "FETCH_FAILED | NOT_FOUND | RATE_LIMITED | INVALID_INPUT",
    "message": "Human readable error message",
    "source": "URL that failed (if applicable)",
    "recoverable": true | false,
    "fallback_used": true | false,
    "fallback_source": "Alternative URL used (if applicable)"
  }
}
```

### Data Gap Notation

In markdown reports, missing data uses:
```
[NOT FOUND: {specific reason}]
[FETCH_FAILED: {URL}]
[LOGIN_REQUIRED: {URL}]
[BLOCKED: {URL}]
```

---

## Versioning

Current Schema Version: `1.0.0`

Include in all outputs:
```json
{
  "schema_version": "1.0.0"
}
```

Or in markdown:
```markdown
<!-- schema_version: 1.0.0 -->
```

---

*Schema reference for market-intel-collector skill*

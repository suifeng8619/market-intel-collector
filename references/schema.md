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

## Predictive Signals (New in v1.1.0)

基于凯撒"高卢战记"原则：记录不仅是历史，更是预测的基础。

### Predictive Assessment Output

```json
{
  "predictive_signals": {
    "virality_score": {
      "value": "0-100",
      "calculation": "based on engagement velocity + platform spread",
      "confidence": "high | medium | low"
    },
    "time_to_peak_estimate": {
      "days": "number",
      "basis": "similar case comparison",
      "confidence": "high | medium | low"
    },
    "competition_window": {
      "hours_until_saturated": "number",
      "current_serp_quality": "low | medium | high",
      "major_players_entering": "boolean"
    },
    "recommended_entry_point": {
      "decision": "now | wait | skip",
      "reasoning": "string",
      "wait_for_signal": "string (if wait)"
    },
    "decay_prediction": {
      "estimated_half_life_days": "number",
      "pattern_match": "explosive_decay | gradual_decline | sustained | revival_potential"
    }
  }
}
```

### Historical Comparison Output

```json
{
  "historical_comparison": {
    "similar_cases": [
      {
        "case_id": "string",
        "keyword": "string",
        "similarity_score": "0-100",
        "similarity_factors": ["string"],
        "outcome": "success | partial | failure",
        "lessons": ["string"]
      }
    ],
    "pattern_match_score": "0-100",
    "predicted_trajectory": "explosive | steady | niche | flash_in_pan",
    "trajectory_confidence": "high | medium | low"
  }
}
```

---

## Early Warning Integration (New in v1.1.0)

### Warning Signal Input

从 early-warning-system 接收的信号格式：

```json
{
  "warning_signal": {
    "trigger_time": "ISO8601 datetime",
    "signal_type": "dao | tian | di | jiang | fa",
    "signal_strength": "1-10",
    "source": "string",
    "raw_data": { },
    "auto_actions_taken": ["string"]
  }
}
```

### Response Level Output

向 response-levels 系统输出的格式：

```json
{
  "response_recommendation": {
    "level": "L1 | L2 | L3 | L4 | L5",
    "confidence": "high | medium | low",
    "five_factors_score": "X/5",
    "battle_index": "1-10",
    "time_sensitivity": "critical | high | medium | low",
    "resource_estimate": {
      "person_days": "number",
      "skills_required": ["string"]
    }
  }
}
```

---

## Long-tail Blitz Integration (New in v1.1.0)

### Keyword Opportunity Matrix

```json
{
  "longtail_opportunities": {
    "tier_1_blitz": [
      {
        "keyword": "string",
        "status": "breakout | rising | stable",
        "serp_results": "number",
        "competition_score": "1-10",
        "recommended_action": "immediate | watch | skip",
        "content_template": "string"
      }
    ],
    "tier_2_surround": [
      {
        "keyword": "string",
        "search_intent": "informational | transactional | navigational",
        "paa_source": "boolean",
        "recommended_content_type": "guide | faq | comparison"
      }
    ],
    "tier_3_longterm": [
      {
        "keyword": "string",
        "monthly_volume_estimate": "number",
        "competition": "none | low | medium",
        "batch_production": "boolean"
      }
    ]
  }
}
```

---

## Quality Gates (New in v1.1.0)

基于罗马军团的纪律原则，每份报告必须通过四道质量关卡。

### Gate Definitions

```json
{
  "quality_gates": {
    "gate_1_completeness": {
      "status": "pass | fail",
      "checks": [
        {"field": "basic_info", "required": true, "present": "boolean"},
        {"field": "ratings_summary", "required": true, "present": "boolean"},
        {"field": "trend_data", "required": true, "present": "boolean"},
        {"field": "battle_assessment", "required": true, "present": "boolean"},
        {"field": "recommendations", "required": true, "present": "boolean"}
      ],
      "missing_required": ["string"],
      "auto_check": true
    },
    "gate_2_consistency": {
      "status": "pass | fail",
      "checks": [
        {"rule": "battle_index_matches_factors", "valid": "boolean"},
        {"rule": "verdict_matches_index", "valid": "boolean"},
        {"rule": "sources_all_valid_urls", "valid": "boolean"},
        {"rule": "dates_are_consistent", "valid": "boolean"}
      ],
      "inconsistencies": ["string"],
      "auto_check": true
    },
    "gate_3_actionability": {
      "status": "pass | fail",
      "checks": [
        {"rule": "has_clear_recommendation", "valid": "boolean"},
        {"rule": "actions_are_specific", "valid": "boolean"},
        {"rule": "risks_have_mitigations", "valid": "boolean"},
        {"rule": "roi_estimate_provided", "valid": "boolean"}
      ],
      "issues": ["string"],
      "auto_check": false,
      "reviewer": "human"
    },
    "gate_4_freshness": {
      "status": "pass | fail | stale",
      "collection_timestamp": "ISO8601 datetime",
      "hours_since_collection": "number",
      "freshness_threshold_hours": 24,
      "refresh_recommended": "boolean",
      "auto_check": true
    }
  }
}
```

### Gate Status Summary

```json
{
  "quality_summary": {
    "overall_status": "approved | needs_review | rejected",
    "gates_passed": "X/4",
    "blocking_issues": ["string"],
    "warnings": ["string"],
    "approved_at": "ISO8601 datetime | null",
    "approved_by": "auto | human"
  }
}
```

---

## Case Library Integration (New in v1.1.0)

### Case Card Format

每个处理过的关键词生成案例卡片：

```json
{
  "case_card": {
    "case_id": "uuid",
    "keyword": "string",
    "type": "game | tool",
    "collection_date": "ISO8601 datetime",

    "timeline": {
      "first_signal_date": "ISO8601 date",
      "viral_start_date": "ISO8601 date",
      "peak_date": "ISO8601 date",
      "our_discovery_date": "ISO8601 date",
      "days_late": "number"
    },

    "metrics_at_peak": {
      "google_trends_index": 100,
      "youtube_videos": "number",
      "platform_rating": "string",
      "platform_reviews": "number"
    },

    "metrics_current": {
      "google_trends_index": "number",
      "decline_percentage": "number"
    },

    "viral_factors": {
      "primary_driver": "string",
      "platforms": ["string"],
      "audience": "string",
      "unique_hook": "string"
    },

    "our_action": {
      "decision": "entered | skipped | watched",
      "response_level": "L1-L5",
      "content_created": ["string"],
      "results": "string"
    },

    "lessons_learned": ["string"],

    "tags": ["string"],

    "similar_to": ["case_id"]
  }
}
```

---

## Versioning

Current Schema Version: `1.1.0`

### Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-12 | Initial schema |
| 1.1.0 | 2025-12 | Added: Predictive Signals, Quality Gates, Case Library, Early Warning Integration, Long-tail Blitz Integration |

Include in all outputs:
```json
{
  "schema_version": "1.1.0"
}
```

Or in markdown:
```markdown
<!-- schema_version: 1.1.0 -->
```

---

*Schema reference for market-intel-collector skill*
*Expanded by: Julius Caesar*

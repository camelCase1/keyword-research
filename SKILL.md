---
name: keyword-research
description: >
  Scrape Google Autocomplete, Reddit, and Google Trends for niche-specific tier list keywords.
  Analyzes demand signals and outputs a ranked report with estimated search volume, keyword difficulty,
  and suggested article titles. Optionally pass custom seed keywords.
  Triggers on: "keyword research", "find keywords", "seo keywords", "article ideas".
allowed-tools:
  - Bash
  - Read
  - Write
  - Agent
---

# Keyword Research — Niche Tier List SEO Keywords

Scrape multiple free sources for niche-specific tier list keyword opportunities, then analyze and rank them by opportunity score.

## Usage

```
/keyword-research
/keyword-research naruto, dragon ball
/keyword-research "fast food, snacks, candy"
/keyword-research --only "beauty, skincare"    # ONLY these seeds, skip defaults
```

## Instructions

When invoked, follow these steps:

### Step 1: Run the scraper

Run the keyword research script. If the user provided custom seeds, pass them via `--seeds` (prepends to defaults) or `--only` (replaces all defaults):

```bash
# No args — use all default seeds (gaming, anime, food, beauty, tech, travel, drinks, etc.)
node scripts/keyword-research.js

# Add custom seeds alongside defaults
node scripts/keyword-research.js --seeds "naruto tier list, dragon ball tier list"

# ONLY research specific seeds (skip all defaults)
node scripts/keyword-research.js --only "naruto tier list, dragon ball tier list"
```

If the user passes seed topics WITHOUT "tier list" suffix, automatically append "tier list" to each seed. For example, if the user says `/keyword-research naruto, dragon ball`, run:

```bash
node scripts/keyword-research.js --seeds "naruto tier list, dragon ball tier list"
```

The script scrapes:
- Google Autocomplete API (real-time search suggestions + alphabet expansion)
- YouTube Autocomplete (video-heavy keyword signals)
- Google Related Searches
- Reddit trending posts
- Google Trends suggestions + **rising queries** (breakout/emerging topics)
- Wikipedia Pageviews API (spike detection)

It outputs:
- Raw keywords JSON: `scripts/keyword-research-raw-YYYY-MM-DD.json`
- Scored keywords JSON: `scripts/keyword-research-scored-YYYY-MM-DD.json`
- Console output with top 50 keywords by demand signal

### Step 2: Analyze the results

After the script runs, read the scored keywords JSON and the raw keywords JSON. Then provide the user with a comprehensive analysis:

1. **Top 10 Article Opportunities** — a table with:
   - Target keyword
   - Use the KD and volume values from the scored JSON. Only override if the value is -1 (failed scrape)
   - Search intent (informational, investigational, transactional)
   - Category (gaming, anime, food, sports, movies, music, beauty, tech, travel, drinks, tabletop, reality-tv, general)
   - Trend velocity (trending up/down/stable) and seasonal timing notes
   - RPM flag (⚠ if low-RPM kids audience like Roblox)

2. **Detailed breakdown** for each of the top 10:
   - Suggested article title (SEO-optimized, under 60 chars)
   - Why this keyword is a good opportunity
   - Specific content strategy (what to cover, how to structure, where to promote)
   - **SERP features**: Note which features are present (featured snippet, video carousel, PAA, image pack) and recommend content format accordingly

3. **Quick wins** — keywords with KD under 25 that could rank in 2-4 months with proper internal linking

4. **Content cluster strategy** — how to group related keywords into topic clusters with internal linking. **Check for cannibalization**: if multiple keywords target the same intent, group them under one URL

5. **Seasonality calendar** — flag keywords with seasonal patterns and recommend optimal publish timing (e.g., NFL content in July-August before season starts)

6. **Recommended priority** — which articles to write first based on effort vs. impact, factoring in RPM potential (beauty, tech, travel, drinks > gaming > anime > Roblox/kids)

### Step 3: Save the report

Save the full analysis as a markdown report at `scripts/keyword-research-YYYY-MM-DD.md`.

### Important Notes

- **TARGET US / T1 AUDIENCES**: All keyword analysis, search volume estimates, and content recommendations should be optimized for US audiences primarily, and Tier 1 English-speaking countries (US, UK, Canada, Australia) secondarily. Deprioritize keywords with demand mainly from non-English or non-T1 regions. When estimating search volume and difficulty, use US-centric data.
- EXCLUDE generic tool keywords like "tier list maker", "tier list template", "tier list creator", etc. Focus only on niche-specific content keywords.
- Keywords with more independent sources (higher source count) indicate stronger real-world demand.
- Prioritize keywords where TierBuddy has a natural advantage (interactive tier lists, user-generated content).
- Consider the user's existing content and categories when recommending opportunities.
- If the OpenRouter API key works, the script will do LLM-powered analysis automatically. If not, do the analysis yourself based on the raw data.

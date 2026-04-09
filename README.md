# Keyword Research

Scrape Google Autocomplete, Reddit, and Google Trends for niche-specific tier list keywords. Analyzes demand signals and outputs a ranked report with estimated search volume, keyword difficulty, and suggested article titles.

## Data Sources

- Google Autocomplete API (real-time suggestions + alphabet expansion)
- YouTube Autocomplete (video-heavy keyword signals)
- Google Related Searches
- Reddit trending posts
- Google Trends suggestions + rising queries (breakout/emerging topics)
- Wikipedia Pageviews API (spike detection)

## Usage

This project is used as a [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code). Invoke it from Claude Code:

```
/keyword-research
/keyword-research naruto, dragon ball
/keyword-research "fast food, snacks, candy"
/keyword-research --only "beauty, skincare"
```

- No args: uses all default seed categories (gaming, anime, food, beauty, tech, travel, drinks, etc.)
- With seeds: adds custom seeds alongside defaults
- With `--only`: replaces all defaults with the specified seeds

## Output

Each run produces:

- `scripts/keyword-research-raw-YYYY-MM-DD.json` — raw scraped keywords
- `scripts/keyword-research-scored-YYYY-MM-DD.json` — keywords scored by opportunity
- `scripts/keyword-research-YYYY-MM-DD.md` — full analysis report with top opportunities, quick wins, content clusters, and seasonality calendar

---
name: daily-ai-news-skill
description: "Generate a daily AI news briefing by aggregating, filtering, and deeply interpreting hot AI/LLM updates from multiple tech and finance sources. Use when users ask for daily AI news, AI morning briefings, LLM trend scans, AI startup/product updates, or a Chinese magazine-style AI report with links and actionable insights."
---

# Daily AI News Skill

Produce a high-signal daily AI news report in Simplified Chinese.

## Workflow

1. Parse user scope: time window, source preference, depth, and audience (engineer/product/investor).
2. Fetch candidate items from the configured sources (default: all major sources).
3. Expand broad keywords automatically:
   - `AI` -> `AI,LLM,GPT,Claude,Generative,Machine Learning,RAG,Agent`
   - `创业` -> `Startup,融资,VC,产品发布,增长`
   - `金融` -> `Finance,Stock,Market,Economy,Crypto,Macro`
4. Filter to AI-relevant items with high value signals (major release, SOTA, high heat, policy impact).
5. If requested time window returns fewer than 5 items, keep all in-window items first, then add marked supplementary hot items from a wider window.
6. Generate final report in markdown and save into `reports/` with timestamped filename.

## Source Strategy

Use these sources as defaults when no source is specified:

- Hacker News
- Product Hunt
- GitHub Trending
- 36Kr
- Tencent News (Tech)
- WallStreetCN
- V2EX
- Weibo Hot Search

### Global Scan Pattern

For full-market daily scan:

```bash
python3 scripts/fetch_news.py --source all --limit 15 --deep
```

Then apply semantic filtering to keep only AI-focused high-value items.

## Reporting Rules (Required)

- Language: Simplified Chinese.
- Tone: concise magazine/newsletter style.
- Include sections:
  - `今日 AI 头条` (3-5 items)
  - `技术与产品` (AI/LLM/product launches)
  - `资本与产业信号` (funding, policy, market)
- Each item must include:
  1. Markdown link title: `### 1. [标题](URL)`
  2. Metadata line: source + time/date + heat/score
  3. One-line why-it-matters summary
  4. 2-3 bullet deep interpretation points

### GitHub Trending Exception

For pure list-based GitHub Trending requests:

- Return all fetched top-N items directly.
- Do not perform smart-fill.
- For each item include:
  - `核心价值`
  - `启发思考`
  - `场景标签` (3-5 tags)

## Time Window Smart Fill

When user asks for a strict window (e.g. past 4 hours):

1. List strict in-window items first.
2. If total < 5, add high-value supplementary items from up to past 24h.
3. Mark supplementary items clearly, e.g. `⚠️ 18h ago` or `🔥 24h Hot`.

## Output

Always save report as:

```text
reports/ai_daily_YYYYMMDD_HHMM.md
```

Then present the full report content in chat.

## Interactive Menu Trigger

If user says `daily-ai-news-skill 如意如意` (or asks for menu/help):

1. Read `templates.md` in this skill folder.
2. Show the commands exactly as listed.
3. Ask user to pick a number or copy a command.

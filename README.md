# Newsletter-Skill

Newsletter-Skill is a Codex skill for collecting China-related coverage from subscribed international media and producing a source-linked Chinese HTML briefing.

The skill is designed for weekday morning and afternoon editions. It uses the user's authenticated Chrome session, verifies articles on the original publisher pages, merges reports about the same event, and preserves the source links used for every briefing item.

## Customization and forks

The media websites and news categories covered by this skill were selected for the author's requirements. Users are welcome to customize the source list, topic taxonomy, schedule, output format, and other instructions to fit their own workflows. Forks and community adaptations are welcome.

## What it does

- Visits Financial Times, The Wall Street Journal, The Economist, and Bloomberg in a fixed serial order.
- Uses each publisher's direct China/latest URL rather than navigating through site menus.
- Applies Beijing-time publication windows and includes the full weekend interval in the Monday morning edition.
- Filters coverage into four categories: US–China relations, macroeconomics, industry, and capital markets.
- Merges reporting about the same concrete event while retaining all verified source links.
- Translates headlines into Chinese and writes a faithful 100–200 Chinese-character summary without adding background analysis.
- Generates a self-contained, responsive HTML dashboard with a table of contents, coverage status, category counts, and linked sources.

## Editions and coverage windows

All times use Asia/Shanghai (UTC+8).

| Edition | Scheduled time | Coverage interval |
| --- | --- | --- |
| Monday morning | Monday 09:00 | Previous Friday 15:00 inclusive to Monday 09:00 exclusive (66 hours; includes Saturday and Sunday) |
| Tuesday–Friday morning | Weekdays 09:00 | Previous day 15:00 inclusive to the current day 09:00 exclusive (18 hours) |
| Afternoon | Weekdays 15:00 | Current day 09:00 inclusive to 15:00 exclusive (6 hours) |

The recurring schedule is configured by the Codex automation that invokes this skill. Manual runs may provide an explicit edition or time window.

## Sources

| Publisher | Direct start URL | Required route |
| --- | --- | --- |
| Financial Times (FT) | <https://www.ft.com/china> | Open the direct URL and inspect the China page |
| Wall Street Journal (WSJ) | <https://www.wsj.com/world/china?mod=nav_top_subsection> | Open the direct URL and inspect the China page |
| The Economist | <https://www.economist.com/topics/china> | Open the direct URL and inspect the China topic page |
| Bloomberg | <https://www.bloomberg.com/latest?utm_source=homepage&utm_medium=web&utm_campaign=latest> | Open Latest and repeatedly use `Load more` until the listing crosses the time-window boundary |

Sources are checked serially in this order: FT → WSJ → The Economist → Bloomberg. Article pages are opened and verified before an item is included.

## Editorial scope

The skill focuses on substantive China-related reporting in these categories:

1. **US–China relations:** trade, tariffs, technology competition, export controls, diplomacy, geopolitics, and US policy toward China.
2. **Macroeconomics:** GDP, deflation, domestic demand, property, local-government debt, growth-model transition, Political Bureau meetings, and macroeconomic policy.
3. **Industry:** AI, robotics, rare earths, China-threat narratives, capacity and overcapacity, subsidies, involution-style competition, and other strategically important industries.
4. **Capital markets:** foreign-investor access, US listings by Chinese companies, program and quantitative trading, outbound investment, corporate fraud, co-location or hosting restrictions, ROE, listed companies, securities regulation, and exchange policy.

Incidental mentions, general market roundups without a substantive China angle, and articles outside the exact publication interval are excluded.

## Output contract

Each item follows this structure:

```text
【媒体名称（主题分类）】：中文标题

【摘要】
100–200 字的忠实中文概括。

【新闻链接】
媒体名称：原文链接

【不确定信息】
仅在确有具体不确定点时出现。
```

The dashboard must:

- Be written in Chinese.
- Contain a linked table of contents in the order US–China relations, macroeconomics, industry, and capital markets.
- Show the edition, exact coverage interval, generation time, total count, category counts, and per-source coverage state.
- Use inline CSS and no external scripts, fonts, trackers, or remote assets.
- Be saved as a timestamped report and as `latest.html` under `/Users/solinzhang/Documents/Newsletter-Reports/` in the configured local environment.

## Browser and access behavior

- Use only visible pages in the user's authenticated Chrome session. Never inspect or export cookies, passwords, local storage, or subscription settings.
- If an anti-bot or human-verification challenge appears, stop immediately, do not retry or bypass it, notify the user, and wait for manual verification.
- If the first direct navigation fails for another reason, pause and tell the user the publisher, exact URL, and visible problem. The user may manually open the URL in Chrome; after confirmation, the skill takes over the existing tab and resumes without repeating completed publishers.
- Each publisher receives at most one manual-opening handoff. If takeover still fails, record `访问受限` or `部分完成`, explain why, and continue without looping.
- Bloomberg pagination is complete only after `Load more` has been clicked one step at a time and a visible item predates the window start. If pagination stops early, Bloomberg is marked `部分完成`.
- Do not use search snippets, aggregators, or unverified headlines to fill gaps.

## Installation

This repository is itself a skill package: `SKILL.md` is at the repository root.

Install it with the Codex skill installer using the repository root, or install from the repository URL in the Codex interface:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --url https://github.com/Solin-Shelly/Newsletter-Skill/tree/main \
  --name newsletter-skill
```

Because the repository is private, the installer needs an authenticated GitHub session or Git credentials with access to `Solin-Shelly/Newsletter-Skill`.

After installation, invoke the skill explicitly with `$newsletter-skill` or ask Codex to run the China-focused international-media briefing workflow.

## Repository layout

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── dashboard-template.html
└── references/
    └── newsletter-spec.md
```

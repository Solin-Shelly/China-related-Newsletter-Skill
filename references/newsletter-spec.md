# Newsletter specification

## Editions and time boundaries

Use Asia/Shanghai (UTC+8) throughout.

| Edition | Scheduled time | Coverage interval |
| --- | --- | --- |
| 周一上午版 | Monday 09:00 | Previous Friday 15:00 exclusive to Monday 09:00 exclusive (66 hours); includes all Saturday and Sunday reporting |
| 周二至周五上午版 | Tuesday–Friday 09:00 | Previous calendar day 15:00 exclusive to current day 09:00 exclusive (18 hours) |
| 下午版 | Weekdays 15:00 | Current day 09:00 inclusive to 15:00 inclusive (6 hours) |

Use the article's original publication time. Morning editions exclude both interval endpoints; afternoon editions include both endpoints. If only a date is visible or the original time cannot be verified, exclude the item from the main briefing and list it under access or verification notes.

## Sources and navigation

| Output label | Direct start URL | Required ego-lite route |
| --- | --- | --- |
| FT | https://www.ft.com/china | Open the direct URL; do not navigate through menus |
| WSJ | https://www.wsj.com/world/china?mod=nav_top_subsection | Open the direct URL; do not navigate through menus |
| 经济学人 | https://www.economist.com/topics/china | Open the direct URL; do not navigate through menus |
| 彭博 | https://www.bloomberg.com/latest?utm_source=homepage&utm_medium=web&utm_campaign=latest | Open the direct URL; repeatedly click `Load more` until visible entries predate the window start; do not navigate through menus |

Visit publishers serially in this exact order: **FT → WSJ → 经济学人 → 彭博**. Complete candidate discovery and article verification for one publisher before opening the next. Reuse or close tabs so the four publisher sites are not kept open simultaneously.

If the first direct attempt to open a publisher fails for a reason other than an anti-bot challenge—including a blank or error page, timeout, unavailable page, sign-in or subscription gate, or ego-lite navigation failure—pause the entire browsing sequence. Immediately tell the user the publisher, exact direct URL, and visible problem, hand off the ego-lite task space, and ask them to open that URL manually in their already authenticated ego-lite browser and confirm when it is ready. Do not repeatedly reload, navigate through menus, switch browsers, or continue to the next publisher. After explicit confirmation, take over the same task space and currently open ego-lite tab without navigating away first and resume at that publisher; do not repeat completed publishers. Permit one such handoff per publisher. If takeover still does not provide access, record `访问受限` (or `部分完成` when some content was verified), state the reason, continue with the remaining publishers, and do not request the same handoff repeatedly.

For Bloomberg, use `Load more` to extend the listing before treating source coverage as complete. Click once, wait until additional entries appear, then repeat; do not use a fixed number of clicks. Stop only after at least one visible entry has an original publication time earlier than the current window start. If a click produces no response or no new visible entries, or the button disappears or is disabled before reaching that boundary, pause at Bloomberg and do not mark it `部分完成`, skip it, continue to another source, or generate the dashboard. Hand off the ego-lite task space to the user, ask them to operate `Load more` or restore the page, and wait for explicit confirmation; then take over the same task space and resume from Bloomberg. If it still does not respond, remain paused and ask the user again. Generate the dashboard only after the Bloomberg boundary check is complete. An anti-bot challenge still triggers the immediate-stop rule below.

If an anti-bot or human-verification challenge appears at any point, stop browsing immediately and do not retry, bypass it, or continue to another publisher. Hand off the ego-lite task space, notify the user in the same task, identify the affected publisher and page, request manual verification in ego-lite, and wait for the user's explicit confirmation before taking over the same task space and resuming from that publisher. Already completed publishers do not need to be revisited. This immediate-verification rule takes priority over the initial-access handoff above.

The user has subscription sessions in ego-lite. Use only the visible authenticated pages in the ego-lite task space. Do not interact with account, billing, password, cookie, or subscription-management settings.

For each source, track one of these coverage states:

- `完成`: the prescribed route was checked through the time-window start and all plausible candidates were opened.
- `部分完成`: some verified content was collected but the route could not be checked completely.
- `访问受限`: sign-in, paywall, bot check, permission, or browser failure prevented verification.
- `无符合项`: the route was checked completely and no article met the scope.

## Inclusion taxonomy

Include articles in which a listed topic is substantively about China, Chinese policy, a Chinese company or market, or a direct China-related international action.

### 1. 中美关系

- Trade and tariffs
- Technology competition or controls
- Diplomatic relations and geopolitical developments
- United States policy toward China

### 2. 宏观经济

- GDP and major economic indicators
- Deflation, domestic demand, and consumption
- Property and housing
- Local-government debt
- Transition between old and new growth drivers
- CPC Central Committee Political Bureau meetings
- Macroeconomic policy

### 3. 产业

- Artificial intelligence and robotics
- Rare earths and critical minerals
- Narratives framed as a China threat
- Capacity, overcapacity, subsidies, and involution-style competition
- Other strategically important industry developments

### 4. 资本市场

- Foreign-investor market access
- US listings by Chinese companies
- Program trading and quantitative-trading regulation
- Outbound investment
- Corporate accounting fraud
- Co-location or hosting restrictions
- ROE and listed-company developments
- China Securities Regulatory Commission or Chinese-exchange policies

Exclude incidental China mentions, general global-market wrap-ups without a substantive China angle, opinion content that does not report a relevant development, and articles outside the exact publication interval.

## Classification and event merging

- Give every item exactly one primary category and one narrower topic label.
- If an item plausibly spans categories, choose the category that best describes the central event. Category ordering is for presentation, not a keyword-precedence rule.
- Merge multiple reports only when a reader would reasonably regard them as coverage of the same underlying event.
- A merged item must retain every verified canonical link. Use a short source-specific attribution when reports differ on numbers, timing, actors, or interpretation.
- There is no per-source or total item limit.

## Writing contract

Use this visible structure for each item:

```text
【媒体名称（主题分类）】：中文标题

【摘要】
100–200 字的忠实中文概括。

【新闻链接】
媒体名称：原文链接

【不确定信息】
仅在确有具体不确定点时出现；否则整段省略。
```

Rules:

- Translate the title without sensationalizing or softening it.
- Preserve the distinction between the media's framing, a quoted person's view, and a reported fact.
- Do not add background analysis or material learned elsewhere.
- Keep material numbers, dates, entities, and policy names when they are central to the report.
- For merged reporting, write one 100–200-character summary and provide labeled links for every contributing source.
- When an article cannot be opened or verified, do not infer a summary from its headline or listing snippet.

## Accepted style example

```text
【彭博（上市公司）】：中国月之暗面就500亿美元估值的IPO前融资展开谈判

【摘要】
月之暗面正准备于八月启动新一轮融资讨论，计划在香港上市前以最高500亿美元的估值进行募资。这家中国人工智能实验室本夏启动了一轮融资，估值为315亿美元，随后计划与潜在投资者展开后续资本筹集的洽谈。6月份，月之暗面实现了3亿美元的年度重复收入，自推出最新AI模型Kimi K3以来，其日均销售额已至少增长六倍。

【新闻链接】
彭博：https://www.bloomberg.com/news/articles/2026-07-21/china-s-moonshot-in-talks-on-pre-ipo-funds-at-50-billion-value
```

The example defines tone and structure only. Never reuse its claims unless that article is inside the current window and is opened again.

## Dashboard contract

- Start with a title, edition, exact interval, generated-at time, and total count.
- When revising, use `(edition date, edition type: morning or afternoon, exact coverage interval)` as the edition key. Re-running the same key must overwrite its existing dashboard file and `latest.html` in place. A morning edition and an afternoon edition are different keys and must never overwrite each other. Do not create or retain parallel superseded versions; create a new report file only when the key differs.
- Show a source-coverage panel for FT, WSJ, 经济学人, and 彭博.
- Generate a linked directory in this order: 中美关系、宏观经济、产业、资本市场.
- Show category and item counts even when zero.
- Each card must show sources, narrow topic, translated title, summary, labeled source links, and an uncertainty note only when required.
- If the run is partial, place a prominent warning near the top and name every incomplete source.
- Do not embed article text, paywalled screenshots, remote images, analytics, or trackers.

---
name: newsletter-skill
description: Gather and summarize China-related coverage from FT, WSJ, The Economist, and Bloomberg through the user's authenticated ego-lite browser session, merge reporting on the same event, and create a source-linked Chinese HTML briefing. Use for the weekday 09:00 or 15:00 newsletter editions and for manual runs of the same workflow. Do not use for general news research or summaries based only on search snippets.
---

# Newsletter Skill

Create a Chinese HTML briefing from articles that were actually opened and verified on the four subscribed media sites.

## Required resources

- Read [references/newsletter-spec.md](references/newsletter-spec.md) before collecting articles. It is the source of truth for editions, media routes, topic scope, merging, and wording.
- Use [assets/dashboard-template.html](assets/dashboard-template.html) as the output layout. Replace every `{{...}}` marker and remove all template-only sample blocks before delivery.
- Use the `ego-browser` skill and its ego-lite task space for all publisher navigation, interaction, and extraction because the workflow depends on the user's existing subscription sessions. Never inspect, export, or expose cookies, passwords, local storage, or other credentials.

## Workflow

1. Establish the edition and exact Asia/Shanghai coverage window.
   - Monday morning edition at 09:00: from the preceding Friday at 15:00 exclusive to Monday at 09:00 exclusive (66 hours), covering all Saturday and Sunday reporting without leaving a gap after Friday's afternoon edition.
   - Tuesday–Friday morning editions at 09:00: from 15:00 the prior day exclusive to 09:00 the current day exclusive (18 hours).
   - Afternoon edition at 15:00: from 09:00 inclusive to 15:00 inclusive (6 hours).
   - For a manual run, use an explicitly requested edition or time window. If neither is supplied, choose the most recently completed scheduled window and state it in the dashboard.
   - Convert each original publication timestamp to Asia/Shanghai before applying the edition-specific boundaries above. Morning editions exclude both endpoints; afternoon editions include both endpoints. Do not use an updated timestamp in place of the original publication time.

2. Discover candidates in ego-lite, one publisher at a time, in this exact order: FT, WSJ, The Economist, then Bloomberg.
   - Start directly from the prescribed source URL; do not navigate through a site's menus to reach the listing.
   - Finish checking and recording one publisher before opening the next. Reuse the current tab where practical and close article tabs after recording verified details; do not keep all four publisher sites open at once.
   - Continue pagination or lazy loading until entries are older than the window start; do not stop after the first screen.
   - On Bloomberg, repeatedly click the `Load more` button one time at a time and wait for new entries after each click. Continue until at least one visible entry predates the window start; the initial screen alone does not count as a complete check. If a click produces no response or no new visible entries before that boundary, pause at Bloomberg, hand off the ego-lite task space to the user, and do not continue to other sources or generate the dashboard. After explicit confirmation, take over the same task space and resume from Bloomberg; generate the dashboard only after the Bloomberg boundary check is completed.
   - Open every plausible candidate and verify its full article, canonical URL, headline, source, and original publication time.
   - Do not substitute web search, Chrome, another browser, an aggregator, or a snippet when subscribed content cannot be opened in ego-lite.

3. Filter and classify.
   - Include only articles whose substantive focus matches the topic taxonomy, not incidental mentions of China.
   - Assign one primary category in this order: 中美关系、宏观经济、产业、资本市场. Use the most central subject, not the first keyword encountered.
   - Record a narrower topic label such as 房地产、人工智能、量化交易监管, or 上市公司.

4. Merge reporting on the same event.
   - Merge only articles about the same concrete event, announcement, policy action, company development, or data release.
   - Keep all verified source links and list all contributing media. Attribute material differences instead of flattening them into one claim.
   - Do not merge unrelated articles merely because they share a broad topic.

5. Write the Chinese item.
   - Translate the headline faithfully and write a 100–200 Chinese-character summary, excluding the title, links, and uncertainty note.
   - Summarize only claims supported by the opened articles. Do not add background analysis, predictions, conclusions, or unstated causal explanations.
   - Use the labels `FT`, `WSJ`, `经济学人`, and `彭博`; join merged sources with `、`.
   - Add `不确定信息：...` only for a specific unresolved point. Omit the field when nothing is uncertain.

6. Build and save the dashboard.
   - Preserve the category order and generate a linked table of contents before the briefings.
   - Show the edition, exact coverage window, generation time, item counts, and per-source coverage status.
   - Keep the HTML self-contained and responsive: inline CSS, no external scripts, fonts, trackers, or remote assets.
   - Save timestamped reports under `/Users/solinzhang/Documents/Newsletter-Reports/` as `china-news-YYYYMMDD-HHMM.html`, and update `latest.html` with the same finished content.
   - Before saving, use `(edition date, edition type: morning or afternoon, exact coverage window)` as the report key. If the same edition is regenerated, overwrite its previous matching report in place and overwrite `latest.html`; do not create or retain a parallel superseded version. A morning edition never overwrites an afternoon edition, and vice versa. Create a new timestamped file only when this key differs.
   - Return clickable absolute links to both files. A desktop shortcut may point to `latest.html`, but the recurring task must write only inside the configured workspace.

## Failure and completion rules

- If any page shows an anti-bot or human-verification challenge, including a CAPTCHA, "verify you are human" prompt, automated-traffic warning, or browser challenge, stop all browsing immediately. Do not reload, retry, bypass the challenge, or continue to another source. Hand off the ego-lite task space to the user, notify them in the same task with the affected publisher and page, ask them to complete the verification in ego-lite, and wait for confirmation. After explicit confirmation, take over the same task space and resume from the blocked source without repeating already completed sources. This rule overrides the normal initial-access handoff below.
- If the initial direct navigation to a publisher fails for a reason other than an anti-bot challenge—including a blank or error page, timeout, unavailable page, sign-in or subscription gate, or ego-lite navigation failure—stop the browsing sequence immediately. Tell the user the publisher, exact direct URL, and visible problem; hand off the ego-lite task space and ask them to open that URL manually in their authenticated ego-lite browser and confirm when the page is ready. Do not repeatedly reload, use an alternate route, switch browsers, or continue to the next publisher while waiting.
- After explicit confirmation, take over the same ego-lite task space and already-open tab without navigating away first, then resume from the blocked publisher. Preserve verified work from completed publishers.
- Allow one manual-opening handoff per publisher. If the page remains unavailable after takeover, mark the publisher `访问受限` (or `部分完成` if some content was verified), state the exact reason, continue with the remaining publishers, and do not loop on repeated handoffs.
- If a Bloomberg `Load more` click produces no response or no new visible entries before the listing reaches the window start, or the button disappears or becomes disabled before that boundary, do not mark Bloomberg `部分完成`, skip it, continue to another source, or generate a partial dashboard. Hand off the ego-lite task space and ask the user to operate `Load more` or restore the page; after explicit confirmation, take over the same task space and resume from Bloomberg. If it still does not respond, remain paused and ask the user again. Generate the dashboard only after Bloomberg reaches the time-window boundary.
- If one source fails, produce a clearly labeled partial dashboard from verified sources and list the failed source in the coverage status. Never describe a partial run as complete.
- If no qualifying articles exist, still create the dashboard with zero counts and a clear `本时段无符合条件的新闻` message.
- Completion requires all four source routes to be checked through the start of the time window, every included article to have been opened, all links to be present, and no unresolved template markers to remain.

---
name: axel
description: Use for any question about the user's own business, before general knowledge. That covers their store, brand, products, customers, numbers, tests, past decisions, documents and work in progress. Typical asks are "what did we decide about...", "what are our brand rules", "what is our returns policy", "where is the report on...", and "pick up where a colleague left off". For tests, funnels, merchandising, search, ads, customers or competitors, the matching specialist skill also applies. Also use it when the user says "save this", "remember this", "note this for the team", or corrects an answer that came from Axel. Axel is the company's shared brain, department files, job threads, checks and read-only store, ads and analytics data.
---

# Axel

**Axel is your company's shared brain.** Your Claude reads from it and saves to it, and so does everyone else's on your team.
Your Digital Media (YDM), your CRO agency, runs it and works from the same brain.

The server enforces every rule that matters. This skill makes the right behaviour the default.
Axel knows your company from your login. Never pass a company name or id to a tool.
The one exception is a `tenant_slug` and `job_id` from a job handover line (see "Jobs from the Axel app").

## What Axel holds

- **Brain:** knowledge you edit when things change. Processes, rules, decisions, brand and customer facts, test learnings.
  Its `memory` area holds short dated facts and corrections. A dated memory fact beats an older document.
- **Files:** documents that record a moment, filed by department. Reports, readouts, briefs, plans.
- **Jobs:** threads of work that anyone on the team can pick up.
- **Checks, findings and briefs:** what Axel's scheduled checks found, and the briefs built from them.
- **Data:** read-only queries on the sources the company has connected.

Everything is shared. Your colleagues and the YDM team see what you save.

## How to answer any business question

Do this loop for every question about the business. Do not answer from general knowledge as if it were company fact.

0. **Specialist area?** If the question is about tests, funnels or why sales moved, merchandising, search, ads, customers or competitors, call `playbook(name)` first and follow it inside this loop.
1. **Memory first.** `brain_lookup(query, partition="memory")`. Corrections and recent decisions live here.
2. **Search with the user's words.** `brain_lookup(query)` with no partition searches every area.
3. **If the results do not answer it, search again, differently.** Never answer "nothing found" after one search.
   - Use other words: the shopper's words, a synonym, singular and plural, the product or topic name. Keywords do not match word forms well.
   - Or pick an area. `brain_map` lists the areas. Set `partition` to each area likely to hold the answer.
   - Raise `k` (up to 20) when results look thin or crowded by raw files.
   - Stop when two sources agree.
4. **Read before you quote.** A search hit is a fragment. `brain_read(path)` the best one to three documents.
5. **Numbers need live data.** Call `connections_list` first. It shows which sources are connected and which tool reads each. Then use that tool.
6. **Answer with sources.** Give the path of every source. Combine sources when the answer is spread out.
   If two facts conflict, the newer dated one wins. Say that you saw both.
   If the brain still lacks the answer, say plainly what you searched and what is missing. Do not guess.

Example: "What do customers say about our sizing?" Call `playbook("customer-insight")` first. Search memory for "sizing". Then search "too small", "runs small", "fit".
Read the top review and support documents. If a reviews or helpdesk source is connected, check it for counts. Answer with each path.

## Which tool for what

| The user wants | Use |
|---|---|
| What the company knows or decided | The loop above: `brain_lookup`, then `brain_read` |
| Who the company is, or how this brain is filed | `brain_read("00-client-profile.md")`, and `brain_read("_filing-rules.md")` if it exists. Not every brain has both. `brain_map("root")` shows them |
| To see what the brain holds, or where to look | `brain_map` (all areas), `brain_map(partition)` (paths in one area) |
| A report, brief or document a colleague saved | `files_list(department?)`, then `file_read(path)` |
| Method for a test, funnel, merchandising, search, ads, customers or competitors | `playbook(name)`. See "Specialist skills" |
| To fill up the brain from what Claude knows and their files | `playbook("brain-fill")` |
| Which data sources are connected | `connections_list` |
| Store orders, sales, products, stock, discounts | `shopify_query` |
| Site behaviour: traffic, funnels, events, experiments | `posthog_query` |
| Google Analytics traffic, channels, landing pages | `ga4_query` |
| Organic Google search: queries, clicks, position, sitemaps | `gsc_query` |
| Shopping feed: disapproved products, feed issues | `merchant_query` |
| Google Ads campaigns, keywords, spend | `google_ads_query` |
| Facebook and Instagram ad spend, ROAS, creatives | `meta_query` |
| Blended ROAS, MER, order attribution | `triplewhale_query` |
| Helpdesk volume, contact reasons, satisfaction, conversations | `commslayer_query` |
| Email or SMS (Klaviyo), product reviews (Judge.me), other sources with no own tool | `api_read` |
| Stored daily figures for a date, or what a department's dashboard tracks | `metrics_get(keys, date)`, `widgets_get(department)`. Keys: `sales`, `orders`, `sessions`, `meta_spend`, `google_spend`, `mer`. Stored figures only; for anything else use the data tool |
| What Axel's checks found, or whether a check ran | `findings_list`, `checks_list`, `check_runs_list`, then `check_run_get(run_id)` for the evidence |
| The published briefs | `briefs_list(department?, limit?)` |
| Work under way, or a colleague's thread | `job_list`, then `job_get(job_id)` |
| Jobs by department | `boards_get(department?)` |
| An action the YDM team set up, such as a report refresh | `run_action` (no arguments lists them). Ask the user before you start one |
| Axel usage this month, or what this person may do | `usage_get`, `permissions_get` |
| To keep something | `remember`, `propose_knowledge` or `file_save`. See "Saving" |
| What happened to a save | `save_status(saveId)` |

Departments for boards, checks, widgets and briefs are: growth, paid, search, ops, care, market, ceo.
You only see data tools for connected sources. If one is missing, say that source is not connected. Do not invent its numbers.
If a tool is refused for lack of permission, say so. `permissions_get` shows what the person may do.

## Specialist skills

Seven specialist skills build on this one. Use the matching one. Each loads its method with `playbook(name)`.

| Area | Skill and playbook name |
|---|---|
| Tests, hypotheses, readouts, what to test next | `experimentation` |
| Why sales or conversion moved, funnels, channel mix | `analytics-and-attribution` |
| Best sellers, variants, bundles, margin, stock, pricing | `merchandising` |
| SEO, Search Console, AI assistants recommending the brand | `search-and-answer-engines` |
| Meta, Google or TikTok ads, ROAS, CPA, scale or cut | `paid-acquisition` |
| Reviews, support themes, complaints, customer language | `customer-insight` |
| Competitors, their offers, pricing and checkout | `competitive-intelligence` |

## Asking for data

- **Data is read-only.** Axel cannot change the store, a product, a price, an ad or a test.
  If the user wants a change, say so. Suggest the Shopify admin, the ad platform, or the YDM team.
- **Ask for aggregates.** Totals, rates and counts by day, product, channel or market.
- **Never request shoppers' personal fields.** That means names, emails, phone numbers, addresses and IP addresses.
  The server strips them anyway, so asking only wastes a query.
- **State the date range, the time zone and the comparison period** with every number.
- **One call per connection.** If a source has more than one, pass each id from `connections_list` in the tool's own id field.
- **Prefer one well-built query to many small ones.** There are rate limits.
  If you hit one, the tool names the limit and when to retry.
- **Back every number with a source.** Label estimates, and say what data would settle them.
- **A result may have some text withheld by the server.** Do not guess what it said.

## Corrections

When the user corrects you, or says an answer is out of date, do this at once:

1. Say you will use the correction for the rest of this chat, until a save lands.
2. Offer `remember` with one or two dated sentences. Show the exact text.
3. If a brain document was wrong, also offer `propose_knowledge` to fix it.

Do not offer `remember` for one person, one order or a private matter.

## Saving

**Chatting never saves anything.** Only a save tool does.
Never say you saved or noted something unless a save tool returned success.

### Offer, then save on yes

Offer to save when:

- a decision is made, or a finding is confirmed and will matter later;
- a document is finished;
- the user corrects you, or the brain turns out to be wrong;
- a session ends with work still open.

Say in one line what you would save and where. Show the exact text. **Save only after the user says yes.**

### Which save tool

Ask one question: **when things change, would you edit this, or write a new one?**

| What it is | Tool | What happens |
|---|---|---|
| A short dated fact or correction. "Spring sale: 20% off sitewide, 1 to 14 October 2026." | `remember(fact)` | Lands at once in memory, dated. One or two sentences. |
| Knowledge you would edit: a process, how-to, rule, brand guideline, decision, standing fact | `propose_knowledge(partition, path, content, why)` | If the signed-in person is a company admin, their yes in chat approves it and it saves at once. For anyone else it waits for an admin, or lands unreviewed if the company auto-approves. The answer says which. |
| A document you would replace: a report, readout, brief, plan for a period | `file_save(department, name, content)` | Lands in that department's files. The name must end in `.md`. |

A report is never knowledge. For both, `file_save` the report, then `propose_knowledge` the lesson and link the file.

Guidance:

- **`propose_knowledge`:** read `_filing-rules.md` if it exists. Otherwise use `brain_map(partition)` to pick the partition and path. Search first so you do not duplicate a document.
  Write a clear `why`: what changed and the source. If it updates a document, name it.
- **`file_save`:** pick the department whose team would look for it. The standard set is Marketing, Ecommerce, CRO, Customer, Product and Operations.
  An admin may have added more. `files_list` shows the current list.
  Use a clear dated name, such as `2026-10-03-october-funnel-read.md`.
- **A new save is searchable after about 15 minutes.** Do not search for it straight away.

After every save, tell the user the result: applied, waiting for approval, or refused.
For a waiting save, give the `saveId`. Check it with `save_status`. States are applied, pending, rejected, undone and refused.

### Never save these

The server refuses them anyway. Do not try.

- **Secrets:** API keys, tokens, passwords.
- **Card or bank numbers.**
- **Company finances:** bank balances, cash position, profit and loss, salaries, loans, investors, tax.
- **Shoppers' personal details:** names, emails, addresses, phone numbers, individual orders.
- **Anyone's private matters:** health, HR, family.
- **Anything meant for another company's brain.**

Fine to save: store performance (revenue, AOV, conversion rate, ad spend, product margins), and your team named in their work role.
"The CRO lead approved the new cart design" is fine.

If a document contains a refused item, remove that part before saving. Tell the user what you removed.

### If a save is refused

- Show the user the reason the server gave, in plain words.
- Do not retry the same content or reword it to get around the rule.
- Offer to save a version without the refused part.

## Jobs: how work is tracked

**A job is a thread.** Anyone on the team can pick it up later.

1. **On any task with more than one step, or any work that may already be under way, start with `job_list`.**
   If an open job matches, `job_get` it and continue. Read the latest entry's next steps and start there. Say whose entry you are picking up.
2. **If none matches, open one.** Say the title, then call `job_create`.
   Set `specialist` if one applies. The key is the specialist skill's name, such as `experimentation`.
   Skip jobs only for a quick one-off question.
3. **Add one entry per session** with `job_add_entry`, at a natural stop. Show it in four short lines, then add it on yes:
   - **asked:** what the user wanted this session;
   - **found:** what you learned, with numbers and sources;
   - **docs:** brain paths used or produced;
   - **next:** next steps, and who owns each.
4. **Close it** with `job_close` when the work is done and the user agrees. The thread stays readable.

Job entries follow the same rules as saves. No secrets, finances, shopper details or private matters.

### Jobs from the Axel app

If the user gives you a job handover line from the Axel app, it carries a `tenant_slug` and a `job_id`. Copy them exactly. Never guess them.

1. `job_context` first. It shows the job, its open ask, decisions, file versions and sources.
2. `job_checkout` before you work. It takes the job so two people cannot work it at once. Pass the `expected_holder_version` from the context.
3. `job_file_version_save` to save a file to the job. Ask first. Use a fresh `idempotency_key` for each new save. Reuse the same key when you retry the same save.
4. `job_ask_answer` only after the person has confirmed that exact ask and option in Axel. Use only the `confirmation_id` Axel gave the person. Never make one up. It expires after ten minutes. Never answer an ask on a guess.
5. `job_hand_back` with notes when you stop. Pass `expected_holder_version` equal to the `job.holder_version` that `job_checkout` returned (or re-read `job_context`). `job_mark_done` only when the job has no open ask.

## Privacy: what is logged

- **Axel logs every call your Claude makes to it.** Who, when, which tool, the request text and the result.
- **Your company's admin can see that call log.** So can the YDM team. Request text is kept for 90 days.
- **Axel never sees your chat,** your other connectors, or anything not sent to it.

So:

- **Personal chats should have Axel switched off.** If a chat turns personal, remind the user once.
- **Keep private matters out of queries and saves.**

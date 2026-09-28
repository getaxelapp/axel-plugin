---
name: axel
description: Use Axel for any question about the user's own business: their store, brand, products, customers, tests, results, past decisions, documents or work in progress. Use it when they want to save a fact, decision, process, brand rule or document for their team. Use it to pick up work someone else started. Axel is the company's shared brain, department files, job threads and read-only Shopify and analytics data. Check Axel before answering about the business from general knowledge.
---

# Axel

**Axel is your company's shared brain.** Your Claude reads from it and saves to it, and so does everyone else's on your team.
Your Digital Media (YDM), your CRO agency, runs it and works from the same brain.

The server enforces every rule that matters. This skill makes the right behaviour the default.

## What Axel holds

- **Brain:** knowledge you edit when things change. Processes, how-tos, rules, decisions and standing facts about the business: brand, customers, tests, learnings, how things are built and run. Axel answers from it.
- **Files:** documents that record a moment, filed by department. Reports, readouts, plans for a period, briefs. When things change, you write a new one, so a report is a file, never brain knowledge. A brain document can link to a file, and so can a job.
- **Jobs:** threads of work that anyone on the team can pick up.
- **Data:** read-only queries on the store and analytics sources the company has connected.

Everything is shared. Your colleagues and the YDM team see what you save.

Axel knows which company you belong to from your login. Never pass a company name or id to a tool.

Nothing in Axel runs on its own. It answers when asked.

## When to use it

- **Any question about this business.** Check Axel first. Do not answer from general knowledge as if it were company fact.
- **Before starting a piece of work.** Check for an open job on it.
- **When something worth keeping comes up.** Offer to save it (see "Saving").

Seven specialist skills build on this one. Use the matching one when a question falls in its area.
They are experimentation, analytics and attribution, merchandising, and search and answer engines.
The other three are paid acquisition, customer insight, and competitive intelligence.
**Each specialist's method is a playbook on the Axel server.** Before specialist work, call `playbook` with that area's name and follow it.

## Picking the tool

| You need | Use |
|---|---|
| What the company knows about a topic | `brain_lookup(query)`, then `brain_read(path)` for the full document |
| How to approach a specialist area, such as a test or a funnel read | `playbook(name)` |
| How this brain is organised | `brain_read("_filing-rules.md")`, then `brain_read("00-client-profile.md")` |
| A document the team saved | `files_list(department?)`, then `file_read(path)` |
| Which data sources are connected | `connections_list` |
| A number from the store | `shopify_query` |
| Behaviour on the site: sessions, funnels, experiments | `posthog_query` |
| Google Analytics or Search Console data | `ga4_query`, `gsc_query` (only if connected) |
| Another connected source, such as a review app | `api_read` |
| Work already under way | `job_list`, then `job_get(id)` |
| Save something | `remember`, `propose_knowledge` or `file_save` (see "Saving") |
| Check a save that is waiting | `save_status(id)` |

How to use them well:

- **Search the brain before the data.** The brain often holds the answer and the context behind it.
- **Read the whole document before you quote it.** A search hit is a fragment.
- **Narrow a search with a partition when you know the area.** `_filing-rules.md` lists this brain's partitions. `files` and `memory` are in every brain.
- **When two facts conflict, the newer dated fact wins.** Say that you saw both.
- **Cite your source.** Give the brain path, the file, or the query you ran.
- **Say when Axel does not have something.** Do not fill the gap with a guess.
- **A result may have some text withheld by the server.** Do not guess what it said.
- **You only see tools for connected sources.** If a source is missing, say it is not connected. Do not invent its numbers.

## Asking for data

- **Data is read-only.** Axel cannot change the store, a product, a price or a test.
  If the user wants a change, say so. Suggest the Shopify admin or the YDM team.
- **Ask for aggregates.** Totals, rates and counts by day, product, channel or market.
- **Never request shoppers' personal fields.** That means names, emails, phone numbers, addresses and IP addresses.
  The server strips them anyway, so asking only wastes a query.
- **State the date range, the time zone and the comparison period** with every number.
- **Prefer one well-built query to many small ones.** There are rate limits.
  If you hit one, the tool names the limit and when to retry.

## Saving

**Chatting never saves anything.** Only a save tool does.
Never say you have saved or noted something unless a save tool returned success.

### Offer, then save on yes

Offer to save at natural points:

- a decision is made;
- a finding is confirmed and will matter later;
- a document is finished;
- the brain turns out to be wrong or out of date;
- a session ends with work still open.

Say in one line what you would save and where. Then ask. **Save only after the user says yes.**

### Brain or file?

Ask one question: **when things change, would you edit this, or write a new one?**

- **Edit it:** it is knowledge, and goes in the brain. A process, a how-to, a rule, a decision, a standing fact about the business. Axel answers from it. Example: "How we launch a new product page."
- **Write a new one:** it is a file. A report, a readout, a plan for a period, a brief. It records a moment and is never updated. Example: "The October funnel read."
- **Both:** save the file, then propose the lasting lesson as knowledge, and link the file from it.

A report is never knowledge.

### Which save tool

| What it is | Tool | What happens |
|---|---|---|
| A short dated fact. "Spring sale: 20% off sitewide, 1 to 14 October 2026." | `remember(fact)` | Lands at once in the memory area, dated. |
| Knowledge you would edit when things change: a process, how-to, rule, brand guideline, decision or standing fact | `propose_knowledge(partition, path, content, why)` | Waits for a company admin to approve. If the company has auto-approve on, it lands at once, marked unreviewed. |
| A document you would replace with a new one: a report, readout, brief or plan for a period | `file_save(department, name, content)` | Lands in that department's files at once. |

Guidance for each:

- **`remember`:** one or two sentences. Include the date the fact applies to if it is not today.
- **`propose_knowledge`:** read `_filing-rules.md` to choose the partition and path.
  Search first, so you do not propose a duplicate.
  Write a clear `why`: what changed, and the source.
  If the proposal updates an existing document, name that document in `why`.
- **`file_save`:** pick the department whose team would look for it.
  The standard set is Marketing, Ecommerce, CRO, Customer, Product and Operations.
  An admin may have added more, so call `files_list` to see the current list.
  Use a clear name with the date, such as `2026-10-03-october-funnel-read.md`.

After every save, tell the user the result: applied, waiting for approval, or refused.
For a waiting save, give them the id. They can ask you to check it with `save_status`.

### Never save these

The server refuses them anyway. Do not try.

- **Secrets:** API keys, tokens, passwords.
- **Card or bank numbers.**
- **Company finances:** bank balances, cash position, profit and loss, salaries, loans, investors, tax.
- **Shoppers' personal details:** names, emails, addresses, phone numbers, individual orders.
- **Anyone's private matters:** health, HR, family.
- **Anything meant for another company's brain.**

These are fine to save:

- **Store performance:** revenue, AOV, conversion rate, ad spend, product margins.
- **Your own team, named in their work role.** "Sam approved the new cart design."

If a document contains a refused item, remove that part before saving. Tell the user what you removed.

### If a save is refused

- Tell the user the reason the server gave, in plain words.
- Do not retry the same content.
- Do not reword it to get around the rule.
- Offer to save a version without the refused part.

## Jobs: work that spans sessions or people

**A job is a thread.** Use one when work will take more than one session or more than one person.

1. **Look first.** Call `job_list`. If an open job matches, call `job_get` and continue it.
2. **Otherwise offer to start one.** On yes, call `job_create` with a clear title.
   Set the specialist if one applies. Its key is the specialist skill's name, such as `experimentation`.
3. **Add one entry per session,** at a natural stop or before the session ends.
   Show the entry in four short lines, then add it with `job_add_entry` on yes:
   - **asked:** what the user wanted this session;
   - **found:** what you learned, with numbers and sources;
   - **documents:** paths of files saved this session;
   - **next:** the next steps, and who owns each one.
4. **Close it** with `job_close` when the work is done and the user agrees.

When you continue someone else's job, read the latest entry's next steps and start there.
Tell the user whose entry you are picking up.

Job entries follow the same rules as saves. No secrets, company finances, shopper details or private matters.

## Privacy: what is logged

- **Axel logs every call your Claude makes to it.** Who, when, which tool, the request text and the result.
- **Your company's admin can see that call log.** So can the YDM team. Request text is kept for 90 days.
- **Axel never sees your chat,** your other connectors, or anything not sent to it.

So:

- **Personal chats should have Axel switched off.** If a chat turns personal, remind the user once.
- **Keep private matters out of queries and saves,** even when Axel is on.

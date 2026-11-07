# Axel for Claude

**Axel is your company's shared brain.** This plugin connects your Claude to it.
Once connected, Claude can read your company's knowledge, files, jobs and store data.
It can also save to them when you say yes. Your whole team shares the same Axel.

Axel is run by Your Digital Media (YDM), your agency.

## Install it in Claude

You need a Claude account and a work email your company has added to Axel.

1. In Claude, open **Customize → Plugins**.
2. Click **Add marketplace** and enter `getaxelapp/axel-plugin`.
3. Find **Axel** in the list and install it.
4. Open the plugin's **Connectors** tab and click **Connect**.
5. Sign in with your work email. Axel emails you a sign-in link. Open it in the same browser.
6. Turn on **Sync automatically** for the Axel marketplace. Updates then arrive by themselves.

Start a new chat. Check that Axel is switched on in the tools menu. Then ask Claude something about your business.

**Added Axel by hand before?** Remove that copy under **Customize → Connectors** first. Two copies of Axel clash.

### For Team and Enterprise owners

An owner can add the marketplace once for the whole organisation. Axel can then be installed for everyone by default.
Each person still clicks **Connect** and signs in with their own work email.

### Using Claude Code

Run these two commands, then restart Claude Code:

```sh
claude plugin marketplace add getaxelapp/axel-plugin
claude plugin install axel@axel
```

To sign in, run `/mcp` in a session, pick the Axel server and follow the link.

## Make Axel your default

Three ways to make a chat work through Axel from the first message:

1. **A Claude Project (best).** Create a Project, for example "Our company · Axel", and paste this into its instructions:

   > Work through Axel for everything in this project. At the start of each chat, check Axel's open jobs and continue the one that fits, or offer to start one. Search Axel before answering from general knowledge. Before we stop, add a job entry. Ask before saving anything.

   Every chat in that Project then works through Axel.
2. **The start command.** Type `/axel:start` and what you're working on, for example `/axel:start October funnel review`.
3. **A one-line prompt** in any chat: "Use Axel for this session and run it as an Axel job."

## What is in the plugin

- **The connection** to Axel at `https://mcp.getaxel.app/mcp`. You sign in with your work email.
- **A start command,** `/axel:start`, that binds a chat to Axel and picks up or opens a job.
- **The core skill.** It teaches Claude how to use Axel well: how to search the brain, which tool to use for each kind of question, how to save safely, and how to run jobs.
- **Seven specialist skills:** experimentation, analytics and attribution, merchandising, and search and answer engines.
  The other three are paid acquisition, customer insight and competitive intelligence.
  Each one loads its playbook from Axel after you sign in.

## What changed in 0.4.0

- **Better brain search.** Claude now checks memory first, then searches again with other words or in other areas before it says it found nothing. It reads the best documents and cites each path.
- **The right tool without prompting.** The core skill has a table that maps what you ask to the Axel tool that answers it. That covers files, jobs, checks, briefs, metrics and every connected data source.
- **Corrections are offered for saving.** When you correct Claude, it offers to remember the fix for the whole team.
- **Jobs from the Axel app.** Claude knows how to read, check out, update and hand back a job from the app.
- **Clearer skill descriptions,** so Claude picks the right specialist from the way you naturally ask.

## Your privacy

- **Axel logs every call your Claude makes to it.** Your company's admin and your YDM team can see that log.
- **Axel never sees your chats,** your other connectors, or anything Claude does not send to it.
- **Switch Axel off for personal chats.** In a chat, click **+** in the message box, then **Connectors**, and turn Axel off.

## Need help?

Ask your YDM team.

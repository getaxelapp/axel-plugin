---
description: Start working through Axel for the rest of this chat. Picks up the matching job, or opens a new one.
argument-hint: <what we're working on>
---

# Start an Axel session

The user wants this whole chat to run through Axel. Topic: $ARGUMENTS

Do these in order:

1. **Work through Axel from now on.** Follow the core `axel` skill for everything in this chat.
   For any business question, run its search loop: memory first, then at least two searches with different words or areas, then read the best documents.
2. **Find the job.** Call `job_list`.
   - If an open job matches the topic, call `job_get`. Say whose entry you are picking up. Start from its next steps.
   - If none matches, call `job_create` with a clear title. Set the specialist if one fits, such as `experimentation`.
   - If there is no topic, list the open jobs and ask which one to continue, or what to start.
3. **Say how you will work,** in two or three short lines:
   - You search Axel several ways before answering from general knowledge, and cite each source.
   - You use the connected data sources for numbers.
   - You ask before saving anything, and offer to remember any correction.
   - You add a job entry before we stop.

If Axel is not connected or asks you to sign in, tell the user to open the Axel plugin's Connectors tab and choose Connect.

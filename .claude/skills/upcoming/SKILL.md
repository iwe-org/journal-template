---
name: upcoming
description: List upcoming birthdays and the people the user is overdue to be in touch with, from the people pages. Read-only. Use when the user asks "whose birthday is coming up", "who should I call", "who haven't I talked to" or "what's coming up".
---

# What is coming up

This skill reads the graph and changes nothing.

## Steps

1. **Birthdays.** Read every person's birthday.

   ``` bash
   iwe find --filter 'birthday: {$exists: true}' --project 'key=$key,title=$title,birthday,deceased' -f yaml
   ```

   Skip anyone with `deceased`. A birthday is `YYYY-MM-DD` or `MM-DD`;
   compare only month and day with today and list those in the next 30 days
   (or the window the user asked for), soonest first. Give the age they turn
   when the year is known.

2. **Gift ideas.** For each upcoming birthday, read the person's
   `## Gifts` section and mention the unchecked ideas.

3. **Overdue to be in touch.** Read everyone with a `stay_in_touch` rhythm.

   ``` bash
   iwe find --filter 'stay_in_touch: {$exists: true}' --project 'key=$key,title=$title,stay_in_touch,last_contacted' -f yaml
   ```

   A person is overdue when `last_contacted` is more than 7 days ago for
   `weekly`, 31 for `monthly`, 92 for `quarterly` or 365 for `yearly`, or is
   missing. List them most overdue first, with the date of the last entry
   that mentions them:

   ``` bash
   iwe find --references people/<slug> --filter 'type: entry' --sort date:-1 -l 1 -f keys
   ```

4. **Open loans and to-dos.** Optionally, when the user asked what is coming
   up in general, list unchecked items under `## Loans` and `## To do` on the
   people pages.

Answer with short lists linking each person's page. Do not edit any document;
if the user wants to act on something, use `log-day`, `add-person` or
`log-event`.

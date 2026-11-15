---
name: log-day
description: Write a journal entry from what the user tells you about their day, link the people in it and note when the user last saw them. Use when the user says "log today", "journal entry", "write about my day", "today I ..." or dictates what happened.
---

# Log a day

## Steps

1. **Find or create the entry.** The key is `journal/<date>`, the date being
   the day the user is writing about (today unless they say otherwise).

   ``` bash
   iwe find -k journal/2026-11-15 -f keys
   ```

   If it exists, append to it. If it does not, create
   `graph/journal/2026-11-15.md`:

   ``` markdown
   ---
   type: entry
   date: 2026-11-15
   mood: good
   ---

   # 2026-11-15

   Coffee with [Mira](../people/mira-kovalenko.md) before work. ...
   ```

   Write in the first person, in the user's words. Tidy grammar, but do not
   add events, feelings or details they did not give. Set `mood` only when
   the user says how the day felt (`awesome`, `good`, `meh`, `bad`, `awful`),
   and `tags` only for topics they name.

2. **Link every person mentioned** inside the sentence that mentions them.

   ``` bash
   iwe find --fuzzy "Mira" --filter 'type: person' -f keys
   ```

   When a name matches no one, ask whether to add the person; if yes, follow
   the `add-person` skill with what the user knows, and link the new page.
   When a name matches more than one person, ask which.

3. **Update `last_contacted`** on each person the user saw or spoke to, when
   the entry's date is later than the value there.

   ``` bash
   iwe update -k people/mira-kovalenko --set last_contacted=2026-11-15
   ```

4. **List a new entry in the Journal hub.** In `graph/journal.md`, under the
   `## YYYY-MM` heading for its month (add the heading above the older months
   if it is missing), put a line of its own at the top:

   ``` markdown
   [2026-11-15](journal/2026-11-15.md)
   ```

5. **Offer follow-ups the entry implies**, without doing them unasked: a life
   event (`log-event`), a gift idea, a loan, or a to-do on a person's page.

6. **Check.**

   ``` bash
   iwe schema validate
   iwe normalize
   ```

---
name: log-event
description: Record something that happened in someone's life — a move, a new job, an engagement, a birth, a loss — as an event that links everyone it happened to. Use when the user says "Anna got a new job", "log that ...", "they had a baby" or "add a life event".
---

# Log a life event

## Steps

1. **Confirm the date.** An event needs the day it happened. If the user gave
   only a month or a season, ask; do not guess.

2. **Create the event** as `graph/events/<YYYY-MM-DD>-<slug>.md`, the slug a
   few words on what happened.

   ``` markdown
   ---
   type: event
   date: 2026-09-13
   category: love
   emotion: positive
   ---

   # Oleh and Mira got engaged

   [Oleh](../people/oleh-bondar.md) proposed to [Mira](../people/mira-kovalenko.md) at the top of the climbing wall.
   ```

   `category` is one of `love`, `family`, `friends`, `work`, `education`,
   `home`, `health`, `travel`, `milestone`, `loss`, `other`. Set `emotion`
   (`positive`, `neutral`, `negative`) only when it is clear from what the
   user said. Link every person it happened to inside a sentence; add a
   person who has no page with the `add-person` skill, after asking.

3. **List it in the Events hub.** In `graph/events.md`, under the `## YYYY`
   heading for its year (add it above older years if missing), put a line of
   its own in date order, newest first:

   ``` markdown
   [Oleh and Mira got engaged](events/2026-09-13-oleh-and-mira-engaged.md)
   ```

4. **Update the people it changes**, when the user's words say so: a new
   relationship on both pages (an engagement, a marriage, a birth), a new
   `## Work` line, a new `address`, or `deceased` for a loss. Ask before
   removing anything.

5. **Chapters.** If the event belongs to a chapter of the user's life in
   `chapters/`, offer to add its link there in date order.

6. **Check.**

   ``` bash
   iwe schema validate
   iwe normalize
   ```

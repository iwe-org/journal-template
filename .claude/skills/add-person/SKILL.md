---
name: add-person
description: Add a person to the journal — a page with what the user knows about them, their relationships written on both sides, and a place on the People hub. Use when the user says "add <name>", "I met <name>", "remember that <name> is ..." or mentions someone who has no page yet.
---

# Add a person

## Steps

1. **Check they are not already here.**

   ``` bash
   iwe find --fuzzy "<name>" --filter 'type: person' -f keys
   ```

2. **Create the page** as `graph/people/<slug>.md`, the slug their name in
   lowercase words joined by hyphens.

   ``` markdown
   ---
   type: person
   created: 2026-11-15
   birthday: 05-14
   ---

   # Anna Novak

   Met at the climbing gym in 2019.

   ## Relationships

   ### Family

   - Sister of [Kate Novak](kate-novak.md)
   ```

   `created` is today. Add only the fields and sections the user gave you; a
   page with a name and one sentence is complete. Frontmatter holds facts to
   filter on (`birthday`, `pronouns`, `labels`, `emails`, `phones`,
   `socials`, `address`, `stay_in_touch`); everything else goes in the
   sections, in the order the person schema lists them.

3. **Write each relationship on both pages.** Add the line under the right
   `### Love`, `### Family`, `### Friends` or `### Work` heading on the new
   page, and the reverse line on the other person's page, named from that
   side: `Parent of` here, `Child of` there.

4. **Place them on the People hub.** Ask how the user knows them if it is not
   clear, then add a line of its own to `graph/people.md` under
   `## Family`, `## Friends`, `## Work` or `## Acquaintances`:

   ``` markdown
   [Anna Novak](people/anna-novak.md)
   ```

5. **Groups and work.** If the user says the person belongs to a group, add a
   line of its own with their link to the group's page. If the user names
   where they work, link the organization from `## Work`, creating
   `graph/organizations/<slug>.md` and listing it in `organizations.md` when
   it is new.

6. **Check.**

   ``` bash
   iwe schema validate
   iwe normalize
   ```

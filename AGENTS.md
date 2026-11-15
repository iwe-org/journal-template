# Agent guide

This workspace is the user's private journal and a record of the people in
their life. You help them write journal entries, keep a page for each person,
record what happens in those people's lives, and remind them of birthdays and
people they have not been in touch with. Documents live in `graph/`; a
document's key is its path under `graph/` without `.md`.

Everything here is personal. Write only what the user tells you, in their
words. Never invent a fact about a person, guess a date, or fill a field you
were not given; leave it out instead.

## What is in the graph

| Kind | Key | Frontmatter |
| --- | --- | --- |
| entry | `journal/YYYY-MM-DD` | `type: entry`, `date`, optional `mood`, `tags` |
| person | `people/<slug>` | `type: person`, `created`, optional `birthday`, `pronouns`, `labels`, `stay_in_touch`, `last_contacted` and others |
| event | `events/YYYY-MM-DD-<slug>` | `type: event`, `date`, `category`, optional `emotion` |
| group | `groups/<slug>` | `type: group`, `kind`, `created` |
| organization | `organizations/<slug>` | `type: organization`, `created`, optional `website` |
| chapter | `chapters/<slug>` | `type: chapter`, `start`, optional `end` |
| hub | `index`, `people`, `groups`, `organizations`, `events`, `journal`, `chapters` | `type: hub` |

Each kind has a schema in `.iwe/schemas/` whose description is the full rule;
read it before writing a kind for the first time.

- **Entries.** One per day, keyed by its date and titled with the date or a
  short title the user gives. A second entry on the same day is
  `journal/YYYY-MM-DD-<slug>`. `mood` is `awesome`, `good`, `meh`, `bad` or
  `awful`.
- **People.** One `# Name` heading with a short paragraph, then optional
  sections in this order: `## Relationships` (under `### Love`, `### Family`,
  `### Friends`, `### Work`), `## Work`, `## Important dates`, `## Pets`,
  `## Notes`, `## Gifts`, `## Loans`, `## To do`. `birthday` is `YYYY-MM-DD`,
  or `MM-DD` when the year is unknown.
- **Events.** Something that happened to one or more people: a move, a new
  job, an engagement, a birth, a loss. Every person it happened to is linked
  inside a sentence.
- **Groups.** A family, couple, circle of friends or club. Its members are
  linked one per line, so each person is a child of the group.
- **Chapters.** A period of the user's own life. Its entries and events are
  linked one per line, in the order they happened.

## How documents connect

- **A person's history is their back-links.** Entries and events link the
  people in them; nothing is copied onto the person's page. To see everything
  about a person:

  ``` bash
  iwe find --references people/anna-novak -f keys
  ```

- **Relationships are written on both pages.** When Anna is Kate's sister, the
  line `- Sister of [Kate Novak](kate-novak.md)` goes under `### Family` on
  Anna's page and `- Sister of [Anna Novak](anna-novak.md)` on Kate's. For a
  relationship with two names, each page uses the name from its own side:
  `Parent of` on one, `Child of` on the other. The usual names are:
  - Love: partner, spouse, fiancé or fiancée, dating, ex-partner
  - Family: parent and child, sibling, grandparent and grandchild, aunt or
    uncle and niece or nephew, cousin, godparent and godchild
  - Friends: friend, best friend, climbing partner or any shared activity
  - Work: colleague, manager and report, mentor and mentee, client
- **The user's own relationship to a person** is where the person sits on the
  People hub: `## Family`, `## Friends`, `## Work` or `## Acquaintances`. A
  person can be listed under more than one.
- **Links.** Markdown links only, relative to the document,
  `[Anna Novak](../people/anna-novak.md)`; never wiki links. A link on a line
  of its own makes the target a child of the document; a link inside a
  sentence or a list item is a reference.
- **Every document is listed in its hub**: entries in `journal.md` under a
  `## YYYY-MM` heading, newest first; events in `events.md` under a `## YYYY`
  heading, newest first; people in `people.md`; groups, organizations and
  chapters in theirs.

## Keeping it current

- When an entry or an event mentions a person, set that person's
  `last_contacted` to the entry's date if it is later than the one there.
- When a gift is given or a loan returned, check its item; do not delete it.
- When a person dies, set `deceased` and keep the page as it is.

## Skills

`log-day`, `add-person`, `log-event` and `upcoming` hold the steps for the
common requests.

## The examples

Documents marked `example: true` are samples that show the shape of each
kind. When the user asks to remove the examples, delete each of them with
`iwe delete`, which also removes the links to them from the hubs:

``` bash
iwe find --filter 'example: true' -f keys
iwe delete -k <key>
```

Then, in `graph/index.md`, remove the two `## Start here` steps that open an
example and the paragraph about the examples, keeping the steps that ask the
agent for something. Do not delete the hubs, and do not delete an example the
user has edited without asking.

Run `iwe schema validate` and `iwe normalize` after a batch of edits.

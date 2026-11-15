# Journal

[![GitHub](https://img.shields.io/badge/GitHub-iwe--org%2Fjournal--template-blue)](https://github.com/iwe-org/journal-template)

A private journal and personal CRM as one knowledge graph: daily entries, a page for each person in your life, how they are related to each other, and what happened to them. It is an [iWe](https://iwe.md) workspace template: the iWe app installs it into an empty folder, and your agent works from `AGENTS.md` and the skills in `.claude/skills/`.

The idea and much of the model come from [Monica](https://github.com/monicahq/monica), the open-source personal relationship manager. See [Credits](#credits).

## What is in it

Documents live under `graph/`:

| Kind | Folder | What it is |
| --- | --- | --- |
| Entries | `graph/journal/` | One journal entry per day, keyed by its date, with a `mood` and `tags` |
| People | `graph/people/` | One page per person: relationships, work, important dates, pets, notes, gifts, loans and to-dos |
| Events | `graph/events/` | Things that happened in people's lives: moves, new jobs, engagements, births, losses |
| Groups | `graph/groups/` | Families, couples, circles of friends and clubs, which list their members |
| Organizations | `graph/organizations/` | Where people work or study |
| Chapters | `graph/chapters/` | Periods of your own life, which list their entries and events in order |
| Hubs | `graph/` | The start page and one hub per kind |

It ships with a small set of linked example documents, all marked `example: true`, that show each kind. Ask your agent to remove the examples when you start; the hubs stay.

## How it fits together

- **A person's history is their back-links.** Journal entries and events link the people in them, and nothing is copied onto the person's page. Open a person and their back-links list every day you saw them and everything that happened to them.
- **Relationships are written on both pages,** grouped under Love, Family, Friends and Work, each side named from its own point of view: `Parent of` on one page, `Child of` on the other.
- **Facts you filter on live in frontmatter:** birthdays, labels, how often you want to stay in touch, and when you were last in touch. Everything else is prose in sections.
- **Groups and chapters include their members** with links on a line of their own, so a person is a child of each group they belong to, and an entry is a child of its chapter.

A person (`graph/people/anna-novak.md`):

``` markdown
---
type: person
created: 2026-11-01
pronouns: she/her
birthday: 1991-05-14
labels:
- climbing
stay_in_touch: monthly
last_contacted: 2026-11-08
---

# Anna Novak

Met at the climbing gym in 2019.

## Relationships

### Family

- Sister of [Kate Novak](kate-novak.md)

## Gifts

- [ ] A book on azulejo tiles (birthday)

## Loans

- [ ] Lent the two-person tent (2026-10-02)
```

A journal entry (`graph/journal/2026-11-08.md`):

``` markdown
---
type: entry
date: 2026-11-08
mood: awesome
---

# 2026-11-08

First session back on the wall. [Anna](../people/anna-novak.md) was in town and came along with [Mira](../people/mira-kovalenko.md).
```

Each kind has a schema in `.iwe/schemas/` and a UI sidecar that tells the iWe app which fields to show beside a link to the document.

## Working with your agent

The skills in `.claude/skills/` cover the common requests:

| Skill | Ask for | What it does |
| --- | --- | --- |
| `log-day` | "log today: had coffee with Mira…" | Writes the entry in your words, links the people in it and updates when you last saw them |
| `add-person` | "add Kate, Anna's sister" | Creates the page, writes the relationship on both sides and places the person on the People hub |
| `log-event` | "Oleh and Mira got engaged in September" | Records the event, links everyone in it and updates their relationships |
| `upcoming` | "whose birthday is coming up?" | Lists birthdays with gift ideas and the people you are overdue to be in touch with; changes nothing |

The graph is queryable with the `iwe` CLI:

``` bash
iwe find --references people/anna-novak -f keys                  # everything that mentions Anna
iwe find --filter 'type: entry, mood: awful' --sort date:-1       # hard days, newest first
iwe find --filter 'labels: climbing' -f keys                      # everyone you climb with
iwe find --filter 'type: event, category: work' --sort date:-1    # career moves
iwe find --included-by groups/novak-family -f keys                # members of a group
```

## Credits

This template is inspired by [Monica](https://github.com/monicahq/monica), the open-source personal relationship management system that helps people "document their life", created by [Regis Freyd (djaiss)](https://github.com/djaiss) and [Alexis Saettler (asbiin)](https://github.com/asbiin) with the Monica community. The kinds of documents here and the vocabularies in their schemas follow Monica's model: contacts with relationships grouped as love, family, friends and work, each with a name for both sides; groups such as families, couples and clubs; life events with an emotion; important dates, pets, gifts, loans and tasks on a contact; a journal with a mood per day; and slices of life, here called chapters.

No code or content is taken from Monica; this template re-expresses its ideas as plain markdown in a knowledge graph. If you want a hosted, multi-user personal CRM with reminders by email, use Monica itself: [monicahq.com](https://www.monicahq.com).

## Checks

From the template folder:

``` bash
iwe-template check .
iwe schema validate
iwe normalize && git diff --exit-code
```

## License

MIT. See [LICENSE.md](LICENSE.md).

## Related projects

- [IWE](https://iwe.md): the CLI and documentation
- [Monica](https://github.com/monicahq/monica): the personal CRM this template is inspired by

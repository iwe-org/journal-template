# Changelog

All notable changes to this template are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [1.0.0]

### Added

- Seven kinds of document, each with a schema and a UI sidecar bound in
  `.iwe/config.toml`: `entry` (`journal/`), `person` (`people/`), `event`
  (`events/`), `group` (`groups/`), `organization` (`organizations/`),
  `chapter` (`chapters/`) and `hub`.
- A start page and one hub per kind.
- A linked set of example documents marked `example: true`: four people, two
  groups, an organization, two events, two journal entries and a chapter.
- An "Add to Today's Entry" action that links the content under the cursor
  into `journal/<today>`.
- `AGENTS.md` and four skills: `log-day`, `add-person`, `log-event` and
  `upcoming`.

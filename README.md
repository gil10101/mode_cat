# mode_cat

The catalogue the Mode app's Marketplace page reads. Mode fetches `index.json`
from this repository over HTTPS (the `marketplace.index_url` setting, by default
`https://raw.githubusercontent.com/gil10101/mode_cat/main/index.json`) and the
pack files it points to; it sends nothing back. No account, no payment, no
telemetry. A pack URL in the index must stay on this host.

## Layout

| Path | What it is |
|---|---|
| `index.json` | The catalogue: `kind: "mode-catalogue"`, `format: 1`, a name, and one entry per pack (slug, name, description, author, version, `url` relative to this file, the counts, tags) |
| `packs/<slug>.mode-pack.json` | One pack per file, in the format the app writes with **Export pack** on an agent or automation row and **Create pack…** on the Marketplace page |

A pack (`kind: "mode-pack"`, `format: 1`) carries up to three lists: `agents`
(an agent profile without anything of one machine: no workspace, path to
`claude`, provider or memories), `automations` (name, schedule, action; a
`start_session` names its agent by the pack-local `profile_key` and keeps its
folder only as a hint, which the install asks about when the folder is not on
the user's computer) and `themes` (theme pack documents as the Appearance page
exports them). The app validates every item before anything is installed and
installs automations switched off.

## Adding a pack

1. In Mode, open Integrations → Marketplace → **Create pack…** (or **Export
   pack** on a row); the file lands in Downloads.
2. Copy it to `packs/`, keeping the `<slug>.mode-pack.json` name.
3. Add an entry to `index.json` with the slug, name, description, author,
   version, the relative `url` and the counts.
4. Open a pull request.

## The starter packs

| Slug | Contents |
|---|---|
| `reviewer-starter` | A pull-request reviewer agent and a weekday night session that runs it |
| `daily-plan` | A weekday note with the day's plan |
| `ocean` | A cool light-and-dark theme pack with a warm accent |

The app's browser mock (`ui/src/app/mock-v1.ts` in the Mode repository, used by
its tests and the dev server) mirrors these three entries, so the Marketplace
page shows the same catalogue offline as it does here.

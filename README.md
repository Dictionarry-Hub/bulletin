# Profilarr Bulletin

Announcement feed and release manifest for Profilarr clients.

Every running Profilarr instance fetches two JSON files from this repo over
GitHub's raw content CDN on a schedule:

- **`versions.json`** - bot-maintained. Holds the current stable release, the
  stable release history, and the latest `develop` commit. Drives the version
  badge and Settings > About page.
- **`announcements.json`** - human-maintained via PR. Holds the list of active
  announcements shown in-app (breaking changes, deprecations, feature
  spotlights).

They are separate files for a reason: a mistake in an announcement PR must not
break the version check for every user, and the version sync bot must never
stomp on an in-flight announcement PR.

Full architecture lives in the profilarr repo:
[`docs/backend/announcements.md`](https://github.com/Dictionarry-Hub/profilarr/blob/main/docs/backend/announcements.md).

## Layout

```
bulletin/
├── versions.json                    # bot-owned (do not hand-edit)
├── announcements.json               # human-owned via PR
├── announcements/
│   └── <ulid>.md                    # body for each announcement
├── schema/
│   ├── versions.schema.json
│   └── announcements.schema.json
└── .github/
    └── workflows/
        ├── validate.yml             # PR validation
        └── sync.yml                 # hourly versions.json sync
```

## Publishing an announcement

1. Generate a ULID ([ulidgenerator.com](https://ulidgenerator.com/) or similar).
2. Create `announcements/<ulid>.md` with the body. Markdown is supported.
3. Add an entry to `announcements.json`'s `announcements` array:

   ```json
   {
     "id": "01J0000000000000000000000A",
     "title": "Short headline",
     "severity": "info",
     "published_at": "2026-04-20T10:00:00Z",
     "expires_at": null,
     "min_version": null,
     "max_version": null,
     "link": null
   }
   ```

4. Open a PR. CI validates the schema, required fields, ULID uniqueness, and
   that every announcement has a matching body file (and vice versa).

Once merged, Profilarr instances pick up the announcement on the next hourly
fetch or on restart.

### Fields

| Field                         | Notes                                                                        |
| ----------------------------- | ---------------------------------------------------------------------------- |
| `id`                          | ULID, 26 chars Crockford Base32. Immutable once published.                   |
| `title`                       | Short headline, 1-200 chars.                                                 |
| `severity`                    | `info`, `warning`, `critical`.                                               |
| `published_at`                | ISO-8601 UTC. Drives sort order.                                             |
| `expires_at`                  | Optional ISO-8601 UTC. After this, clients hide the entry.                   |
| `min_version` / `max_version` | Optional SemVer bounds for targeting. Absent or null = no bound on that end. |
| `link`                        | Optional external URL surfaced as "Read more" in the UI.                     |

## Withdrawing an announcement

Remove the entry from `announcements.json` and delete the body file. Clients
reconcile missing ids as `withdrawn` and hide them locally on the next sync.
The git history preserves the content.

## Versions manifest

Do not edit `versions.json` manually. The `sync.yml` workflow runs hourly,
paginates the GitHub Releases API for Profilarr (filtering drafts and
prereleases), fetches the latest `develop` commit, and overwrites
`versions.json` with the current state. It commits only if something actually
changed.

Trigger a manual sync from the Actions tab
(`Sync versions manifest` > `Run workflow`) for an immediate refresh.

The full-overwrite approach means a yanked release disappears from the
manifest on the next sync, and edits to release metadata on GitHub propagate
without manual intervention. The bulletin repo mirrors GitHub; it doesn't
maintain a parallel log.

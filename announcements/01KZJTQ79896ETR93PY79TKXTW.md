Profilarr v2.1.0 is now available! Here are the highlights:

# Database Sync Priority

You can now sync quality profiles from multiple databases to the same Arr instance.

- Drag the database cards into the order you want; the topmost database has the highest priority.
- Priority only matters when multiple databases provide a quality profile or referenced custom format with the same name. The higher-priority database's version wins. If there are no overlapping names, changing the order has no effect.

# Preview Upgrade Filters

Upgrade filters can now be previewed before a run, letting you see what will be selected and fine-tune your setup without triggering any searches.

- The preview shows which library items are selected, which remain eligible, which are on cooldown, and which were filtered out.

# Smaller Improvements

- Search across all fields when filtering custom formats and other entities.
- Disable the sync prompt shown after saving configuration changes.
- Manage the API key through an environment variable.
- Use decimal size values in custom format conditions.
- Automatically select a condition pattern when its name matches.
- Prioritise upgrades by size, with new largest-first and smallest-first selectors.
- Improved Sonarr upgrade scoring using episode file data.
- Cleaner and more consistent Profilarr and Arr log views.

As usual, please post any bugs and feature requests to [GitHub](https://github.com/Dictionarry-Hub/profilarr/issues).

Profilarr v2.3.0 is now available! Here are the highlights:

# Experimental Sonarr v5 Support

Sonarr v5 stores quality size limits on each quality profile. On a v5 instance, choose the quality definition on the Sync page, for all profiles or per profile. v5 instances show an Experimental label.

# Scoped API Keys

Create named API keys that only reach the areas a script needs, with read or write access and an optional expiry.

# Password and SSO Login Together

Password login and SSO can now be enabled at the same time. With `AUTH=on`, set all three `OIDC_*` settings to show the SSO button.

# Basic Auth for Arr Instances

Arr instances behind a reverse proxy login, such as Authelia, Traefik `basicAuth` or nginx `auth_basic`, can now store Basic Auth credentials. If a proxy login blocks the connection test, Profilarr now tells you.

# Smaller Improvements

- Lower memory use and a smaller image.
- Show delay profile delays in minutes, hours or days.
- Choose the page Profilarr opens on.
- Run renames by hand while the rename schedule is off.
- See and open the regex behind each custom format condition.
- Library sort order and download status filter are remembered.
- Outbound proxy settings are logged at startup.

# Upgrade Notes

- The local authentication bypass is gone.
- Any existing API keys become full access "default" keys.
- `AUTH=oidc` is deprecated. Use `AUTH=on` with all three `OIDC_*` settings.

# Security

This release fixes three security issues. Read each advisory for details and any steps to take.

- [GHSA-w2w8-9gr2-cg28](https://github.com/Dictionarry-Hub/profilarr/security/advisories/GHSA-w2w8-9gr2-cg28): sanitized backup downloads.
- [GHSA-6wpc-hrgx-553v](https://github.com/Dictionarry-Hub/profilarr/security/advisories/GHSA-6wpc-hrgx-553v): markdown from linked databases.
- [GHSA-xhjf-xjhw-69mh](https://github.com/Dictionarry-Hub/profilarr/security/advisories/GHSA-xhjf-xjhw-69mh): SQL in linked databases.

As usual, please post any bugs and feature requests to [GitHub](https://github.com/Dictionarry-Hub/profilarr/issues).

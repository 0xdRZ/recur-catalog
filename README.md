# recur-catalog

The remote provider catalog for [Recur](https://github.com/0xdRZ/recur-app) —
subscription brand names, logos, and metadata. The app ships a bundled snapshot and
syncs this repo through the jsDelivr CDN, so **adding a brand here reaches every user
without an App Store release**.

Releases are **immutable git tags**. The app resolves the newest tag via jsDelivr's
version API and fetches that tag's files from a `@<version>` URL that is cached
forever and can never go stale. (The mutable `@main` ref was unreliable — jsDelivr
caches a separate copy per compression and a purge did not clear every variant.)

## Adding a brand

1. Add a square logo PNG to `logos/<key>.png` (transparent background, ≥240×240).
   Optional dark-mode variant: `logos/<key>@dark.png`.
2. Append an entry to `providers` in `catalog.json` (keep the array alphabetized):

```json
{ "key": "disneyplus", "name": "Disney+", "logo": "logos/disneyplus.png" }
```

Optional fields: `"darkLogo"`, `"color"` (hex brand color, e.g. `"#E50914"` — overrides
the wash the app computes from the logo, needed for monochrome marks like ChatGPT),
`"cancel"` (direct link to the service's cancellation page — powers the app's
cancellation assistant), `"overridesBundled": true` (the catalog logo REPLACES the
copy bundled inside the app — use for rebrands of bundled marks so the new art
reaches users without an App Store release; point `"logo"` at a NEW filename, e.g.
`logos/<key>-v2.png`, so the immutable CDN can't serve a stale copy).

3. **Bump the top-level `version` integer** (human-readable; increment by one).
4. Commit, then **tag the release and push both**:

```sh
git add -A && git commit -m "Add <brand> (catalog vN)"
git tag vN.0.0        # N = the catalog version integer, semver so jsDelivr sorts it
git push origin main vN.0.0
```

No CDN purge needed — the tag is immutable. jsDelivr indexes a new tag within a few
minutes, after which the app resolves it on its next check.

## Consumed by the app as

```
# newest release tag (sorted newest-first):
https://data.jsdelivr.com/v1/packages/gh/<owner>/recur-catalog        ->  versions[0]
# then that tag's immutable files:
https://cdn.jsdelivr.net/gh/<owner>/recur-catalog@<version>/catalog.json
https://cdn.jsdelivr.net/gh/<owner>/recur-catalog@<version>/logos/<key>.png
```

`schemaVersion` marks breaking format changes; clients ignore catalogs with a
schemaVersion newer than they understand.

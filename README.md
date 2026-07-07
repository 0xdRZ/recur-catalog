# recur-catalog

The remote provider catalog for [Recur](https://github.com/yogasaikrishna/recur-app) —
subscription brand names, logos, and metadata. The app ships a bundled snapshot and
syncs this repo through the jsDelivr CDN, so **adding a brand here reaches every user
without an App Store release**.

## Adding a brand

1. Add a square logo PNG to `logos/<key>.png` (transparent background, ≥240×240).
   Optional dark-mode variant: `logos/<key>@dark.png`.
2. Append an entry to `providers` in `catalog.json` (keep the array alphabetized):

```json
{ "key": "disneyplus", "name": "Disney+", "logo": "logos/disneyplus.png" }
```

Optional fields: `"darkLogo"`, `"color"` (hex brand color, e.g. `"#E50914"`),
`"category"` (default category name), `"cancelURL"` (direct link to the service's
cancellation page — powers the app's cancellation assistant).

3. **Bump the top-level `version` integer** — clients only re-sync when it increases.
4. Commit to `main`. Done — jsDelivr serves it worldwide within minutes.

## Consumed by the app as

```
https://cdn.jsdelivr.net/gh/<owner>/recur-catalog@main/catalog.json
https://cdn.jsdelivr.net/gh/<owner>/recur-catalog@main/logos/<key>.png
```

`schemaVersion` marks breaking format changes; clients ignore catalogs with a
schemaVersion newer than they understand.

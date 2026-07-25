# Modern Dev Tools - Community Known Issues

A community database of RimWorld bugs and their fixes, in a simple JSON file. The
[Modern Dev Tools](https://steamcommunity.com/) mod reads this file so that when a
player selects an error in the debug log, they see a plain-language explanation and a
fix, contributed by the community, without waiting for a mod update.

Anyone can add an entry: mod authors documenting a known conflict, or players who found
a fix. See CONTRIBUTING.md.

## How the mod uses it

- In Modern Dev Tools, the player turns on **Use community databases** (Modules window)
  and presses **Update info**. The mod downloads this `known-issues.json` on a background
  thread and caches it. It works offline afterward from the cache.
- When the player selects an error, the mod matches it against every entry (by exception
  type, message keywords or regex, stack namespaces, or the implicated mod's packageId).
  A match shows the entry's title, explanation, fix, and link, and marks the error as a
  "Known issue".
- Nothing here can run code. It is pure data: text and match rules. It is safe.

## File

- `known-issues.json` - the database (this is what the mod downloads).
- `schema.json` - a JSON Schema for validation.
- `CONTRIBUTING.md` - how to add an entry, field by field.

## Raw URL

The mod fetches:

```
https://raw.githubusercontent.com/Astryls/ModernDevTools-KnownIssues/main/known-issues.json
```

If you fork this under a different name, update the URL in the mod (or open an issue).

## License

The data in `known-issues.json` is released under CC0 (public domain) so it can be
bundled, mirrored, and reused freely.

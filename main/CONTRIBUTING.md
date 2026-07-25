# Contributing a known issue

Thank you for helping other players. You do not need to know any code. You are just
adding a block of text and some match rules to a JSON file.

## Quick steps

1. Fork this repository.
2. Open `known-issues.json`.
3. Copy an existing entry in the `issues` array and edit the fields (below).
4. Give it a new, unique `id`.
5. Commit and open a pull request. Keep one issue per entry.

Validate your JSON before submitting (any JSON linter, or `python -m json.tool known-issues.json`).

## Entry fields

Each entry is one object in the `issues` array.

| Field | Required | What it is |
|---|---|---|
| `id` | yes | A unique, lowercase, dash-separated id, e.g. `foomod-nullref-on-load`. |
| `title` | yes | One short line naming the problem. Sentence case. |
| `explanation` | yes | Plain language: what the error means and why it happens. |
| `fix` | yes | Plain language: what the player should do. |
| `severity` | no | One of: `info`, `low`, `moderate`, `high`, `critical`. |
| `url` | no | A link to the mod page, an issue, or a patch. |
| `match` | yes | How to recognise the error. At least one matcher must be set (below). |
| `reportedBy` | no | Your name or handle, for credit. |
| `verified` | no | `true` if the fix is confirmed, else `false`. |

### The `match` object

An error matches the entry if ANY of these hit. More specific matchers score higher, so
prefer keywords or a namespace over a bare exception type.

| Matcher | What it tests |
|---|---|
| `exceptionTypes` | Short exception names in the message, e.g. `["NullReferenceException"]`. |
| `keywords` | Case-insensitive substrings of the message, e.g. a class name `["FooMod.BarComp"]`. |
| `regexes` | .NET regular expressions tested against the message. |
| `namespaces` | Namespace prefixes that appear in the stack trace, e.g. `["FooMod"]`. |
| `packageIds` | packageIds of mods implicated by the stack trace, e.g. `["author.foomod"]`. |

Leave a matcher as an empty array `[]` if you do not use it.

## Writing a good entry

- Match tightly. A bare `NullReferenceException` matches thousands of errors; add a
  keyword (a class name from the stack) or the mod's namespace so it only fires for the
  real bug.
- Copy the exact class or message text from a real log. In Modern Dev Tools, select the
  error and press **Copy report** to get the message, likely source, and stack trace.
- Write the fix for a player: what to update, what setting to change, or which mod to
  remove. Avoid jargon.
- Keep `title`, `explanation`, and `fix` in plain sentence case. No AI-style dashes; use
  commas, colons, or short sentences.

## Example

```json
{
  "id": "foomod-nullref-on-load",
  "title": "NullReferenceException from Foo Mod when loading a save",
  "explanation": "Foo Mod assumes every pawn has a job when a save loads, which is not always true, so it throws on load.",
  "fix": "Update Foo Mod to 2.1 or later. If you cannot, load the save with Foo Mod's 'auto-assign' option turned off, then turn it back on.",
  "severity": "high",
  "url": "https://steamcommunity.com/sharedfiles/filedetails/?id=123456789",
  "match": {
    "exceptionTypes": ["NullReferenceException"],
    "keywords": ["FooMod.JobLoader"],
    "regexes": [],
    "namespaces": ["FooMod"],
    "packageIds": ["author.foomod"]
  },
  "reportedBy": "yourname",
  "verified": true
}
```

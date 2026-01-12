```yaml
number: 16381
title: "Avoid indexing the project if `configurationPreference` is `editorOnly`"
type: pull_request
state: merged
author: dcarrier
labels:
  - server
assignees: []
merged: true
base: main
head: issue-16267
created_at: 2025-02-25T18:58:55Z
updated_at: 2025-02-27T02:19:49Z
url: https://github.com/astral-sh/ruff/pull/16381
synced_at: 2026-01-12T15:55:54Z
```

# Avoid indexing the project if `configurationPreference` is `editorOnly`

---

_@dcarrier_

## Summary

Closes: https://github.com/astral-sh/ruff/issues/16267

This change skips building the `index` in RuffSettingsIndex when the configuration preference, in the editor settings, is set to `editorOnly`. This is appropriate due to the fact that the indexes will go unused as long as the configuration preference persists.

## Test Plan

I have tested this in VSCode and can confirm that we skip indexing when `editorOnly` is set. Upon switching back to `editorFirst` or `filesystemFirst` we index the settings as normal.

I don't seen any unit tests for setting indexing at the moment, but I am happy to give it a shot if that is something we want.


---

_Comment by @github-actions[bot] on 2025-02-25 19:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `server` added by @ntBre on 2025-02-26 03:19_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-02-26 07:35_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:115 on 2025-02-26 08:17_

I think the `fallback` should only use the default configuration. If you notice in `RuffSettings::fallback` it will first try to get the user settings via `find_user_settings_toml` (e.g., `~/.config/ruff/ruff.toml`) which should be ignored for this change.

What I would do is:
* Separate the default logic from `RuffSettings::fallback` (inside `unwrap_or_else`) as a separate method which could be named `editor_only`.
* Use this new method in `RuffSettings::fallback` and when configuration preference is `EditorOnly`
* (Optional) Document both this new method and `fallback` method

---

_@dhruvmanila requested changes on 2025-02-26 08:19_

Thanks for taking a look at this. I think we should avoid considering the user settings and only use the default configuration.

---

_@dcarrier reviewed on 2025-02-26 21:22_

---

_Review comment by @dcarrier on `crates/ruff_server/src/session/index/ruff_settings.rs`:115 on 2025-02-26 21:22_

Thank you for the review. I have implemented the three changes that you have recommended.

Curious on your thoughts!

---

_@dhruvmanila approved on 2025-02-27 02:13_

Thank you! I made couple of tweaks:

* Changed the debug message to include the workspace path
* Used `.ok().map(...)` instead of explicit `match` in `fallback`

---

_Renamed from "Skip RuffSettings indexing during editorOnly" to "Avoid settings indexing if `configurationPreference` is `editorOnly`" by @dhruvmanila on 2025-02-27 02:13_

---

_Renamed from "Avoid settings indexing if `configurationPreference` is `editorOnly`" to "Avoid indexing the project if `configurationPreference` is `editorOnly`" by @dhruvmanila on 2025-02-27 02:14_

---

_Merged by @dhruvmanila on 2025-02-27 02:16_

---

_Closed by @dhruvmanila on 2025-02-27 02:16_

---

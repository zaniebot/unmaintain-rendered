```yaml
number: 12362
title: Use fallback settings when indexing the project
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/fallback
created_at: 2024-07-17T12:51:31Z
updated_at: 2024-07-18T03:46:47Z
url: https://github.com/astral-sh/ruff/pull/12362
synced_at: 2026-01-12T15:55:41Z
```

# Use fallback settings when indexing the project

---

_@dhruvmanila_

## Summary

This PR updates the settings index building logic in the language server to consider the fallback settings for applying ignore filters in `WalkBuilder` and the exclusion via `exclude` / `extend-exclude`.

This flow matches the one in the `ruff` CLI where the root settings is built by (1) finding the workspace setting in the ancestor directory (2) finding the user configuration if that's missing and (3) fallback to using the default configuration.

Previously, the index building logic was being executed before (2) and (3). This PR reverses the logic so that the exclusion / `respect_gitignore` is being considered from the default settings if there's no workspace / user settings. This has the benefit that the server no longer enters the `.git` directory or any other excluded directory when a user opens a file in the home directory.

Related to #11366

## Test plan

Opened a test file from the home directory and confirmed with the debug trace (removed in #12360) that the server excludes the `.git` directory when indexing.

---

_Label `server` added by @dhruvmanila on 2024-07-17 12:51_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-17 12:53_

---

_Comment by @github-actions[bot] on 2024-07-17 13:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-07-17 14:36_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/index/ruff_settings.rs`:170 on 2024-07-17 14:36_

Are these clones necessary?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:170 on 2024-07-17 15:09_

I think these are all `Arc<RuffSettings>` so this should be cheap. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:175 on 2024-07-17 15:11_

I'm a bit surprised why we return `Continue` but the comment at the top says: *If the directory is excluded from the workspace, skip it*

---

_@MichaReiser reviewed on 2024-07-17 15:11_

---

_@dhruvmanila reviewed on 2024-07-17 16:35_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:170 on 2024-07-17 16:35_

Yes, these are all `Arc`s

---

_@dhruvmanila reviewed on 2024-07-17 16:38_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:170 on 2024-07-17 16:38_

It otherwise throws "rustc: temporary value dropped while borrowed" if I use a reference

```rs
                    let settings = index
                        .read()
                        .unwrap()
                        .range(..directory.clone())
                        .rfind(|(path, _)| directory.starts_with(path))
                        .map(|(_, settings)| settings)
                        .unwrap_or(&fallback);
```

---

_@dhruvmanila reviewed on 2024-07-17 16:41_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:175 on 2024-07-17 16:41_

Oh my! Good catch. I think I confused `Skip` with `Continue` in https://github.com/astral-sh/ruff/pull/12299 🤦‍♂️ 

---

_@MichaReiser approved on 2024-07-17 20:40_

---

_Merged by @dhruvmanila on 2024-07-18 03:46_

---

_Closed by @dhruvmanila on 2024-07-18 03:46_

---

_Branch deleted on 2024-07-18 03:46_

---

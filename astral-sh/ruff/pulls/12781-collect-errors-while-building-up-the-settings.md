```yaml
number: 12781
title: Collect errors while building up the settings index
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/settings-index-error
created_at: 2024-08-09T12:08:39Z
updated_at: 2024-08-12T15:52:15Z
url: https://github.com/astral-sh/ruff/pull/12781
synced_at: 2026-01-12T15:55:42Z
```

# Collect errors while building up the settings index

---

_@dhruvmanila_

## Summary

Related to https://github.com/astral-sh/ruff-vscode/issues/571, this PR updates the settings index builder to trace all the errors it encountered. Without this, there's no way for user to know that something failed and some of the capability might not work as expected. For example, in the linked PR, the settings were invalid which means notebooks weren't included and there were no log messages for it.

## Test Plan

Create an invalid `ruff.toml` file:
```toml
[tool.ruff]
extend-exclude = ["*.ipynb"]
```

Logs:
```
2024-08-12 18:33:09.873 [info] [Trace - 6:33:09 PM]   12.217043000s ERROR ruff:main ruff_server::session::index::ruff_settings: Failed to parse /Users/dhruv/playground/ruff/pyproject.toml
```

Notification Preview:

<img width="483" alt="Screenshot 2024-08-12 at 18 33 20" src="https://github.com/user-attachments/assets/a4f303e5-f073-454f-bdcd-ba6af511e232">

Another way to trigger is to provide an invalid `cache-dir` value:
```toml
[tool.ruff]
cache-dir = "$UNKNOWN"
```

Same notification preview but different log message:
```
2024-08-12 18:41:37.571 [info] [Trace - 6:41:37 PM]   21.700112208s ERROR ThreadId(30) ruff_server::session::index::ruff_settings: Error while resolving settings from /Users/dhruv/playground/ruff/pyproject.toml: Invalid `cache-dir` value: error looking key 'UNKNOWN' up: environment variable not found
```

With multiple `pyproject.toml` file:
```
2024-08-12 18:41:15.887 [info] [Trace - 6:41:15 PM]    0.016636833s ERROR ThreadId(04) ruff_server::session::index::ruff_settings: Error while resolving settings from /Users/dhruv/playground/ruff/pyproject.toml: Invalid `cache-dir` value: error looking key 'UNKNOWN' up: environment variable not found

2024-08-12 18:41:15.888 [info] [Trace - 6:41:15 PM]    0.017378833s ERROR ThreadId(13) ruff_server::session::index::ruff_settings: Failed to parse /Users/dhruv/playground/ruff/tools/pyproject.toml
```

---

_Label `server` added by @dhruvmanila on 2024-08-09 12:08_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:249 on 2024-08-09 12:09_

I'm not sure if this is the way to go. We can't quit if we encounter an error as that would be a breaking change because currently we just continue to the next path.

---

_@dhruvmanila reviewed on 2024-08-09 12:09_

---

_Comment by @github-actions[bot] on 2024-08-09 12:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @dhruvmanila on 2024-08-09 13:09_

---

_Comment by @codspeed-hq[bot] on 2024-08-12 13:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/settings-index-error)

### Merging #12781 will **not alter performance**

<sub>Comparing <code>dhruv/settings-index-error</code> (2004c6a) with <code>main</code> (2ea7957)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Marked ready for review by @dhruvmanila on 2024-08-12 13:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-12 13:12_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:164 on 2024-08-12 14:10_

You can use an `AtomicBool` instead of a `std::sync::Mutex`. An `AtomicBool` is more lightweight than a `Mutex`. 

---

_@MichaReiser approved on 2024-08-12 14:12_

---

_Comment by @MichaReiser on 2024-08-12 14:13_

Thanks. This is a great addition!

---

_Merged by @dhruvmanila on 2024-08-12 15:42_

---

_Closed by @dhruvmanila on 2024-08-12 15:42_

---

_Branch deleted on 2024-08-12 15:42_

---

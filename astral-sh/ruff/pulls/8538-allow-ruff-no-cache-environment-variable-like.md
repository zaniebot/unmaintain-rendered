```yaml
number: 8538
title: Allow RUFF_NO_CACHE environment variable (like RUFF_CACHE_DIR)
type: pull_request
state: merged
author: akx
labels:
  - cli
assignees: []
merged: true
base: main
head: no-cache-envvar
created_at: 2023-11-07T12:24:42Z
updated_at: 2023-11-27T04:08:09Z
url: https://github.com/astral-sh/ruff/pull/8538
synced_at: 2026-01-12T15:55:26Z
```

# Allow RUFF_NO_CACHE environment variable (like RUFF_CACHE_DIR)

---

_@akx_

## Summary

Being able to set `--no-cache` without touching the command line makes comparing formatter speed with e.g. Hyperfine a lot easier; Black allows one to set `BLACK_CACHE_DIR=/dev/null`, but setting `RUFF_CACHE_DIR=/dev/null` has Ruff choke:

```
error: Failed to initialize cache at /dev/null: Not a directory (os error 20)
error: Failed to initialize cache at /dev/null: Not a directory (os error 20)
warning: Failed to open cache file '/dev/null/0.1.4/18160934645386409287': Not a directory (os error 20)
```

Alternately, we could make a `/dev/null` (or `nul` on Windows) cache directory imply `--no-cache`?

## Test Plan

None yet.

---

_Comment by @github-actions[bot] on 2023-11-07 13:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@konstin approved on 2023-11-07 13:21_

---

_Label `cli` added by @dhruvmanila on 2023-11-07 13:31_

---

_Merged by @zanieb on 2023-11-07 14:35_

---

_Closed by @zanieb on 2023-11-07 14:35_

---

_Comment by @MichaReiser on 2023-11-27 04:08_

You can use `--parameter-list` and set e.g. `CACHE` to `--no-cache` or an empty string. That's how I did cached/uncached comparisons in the past. I'm not sure if that's more or less convenient than what you use. 

I'm a bit hesitant from adding environment variables only for profiling. They are a source of confusion and introduce some complexity. 

---

```yaml
number: 15452
title: "Display context for `ruff.configuration` errors"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/display-err-context
created_at: 2025-01-13T09:48:16Z
updated_at: 2025-01-13T10:13:22Z
url: https://github.com/astral-sh/ruff/pull/15452
synced_at: 2026-01-12T15:55:51Z
```

# Display context for `ruff.configuration` errors

---

_@dhruvmanila_

## Summary

I noticed this while trying out https://github.com/astral-sh/ruff-vscode/issues/665 that we use the `Display` implementation to show the error which hides the context. This PR changes it to use the `Debug` implementation and adds the message as a context.

## Test Plan

**Before:**

```
   0.001228084s ERROR main ruff_server::session::index::ruff_settings: Unable to find editor-specified configuration file: Failed to parse /private/tmp/hatch-test/ruff.toml
```

**After:**

```
   0.002348750s ERROR main ruff_server::session::index::ruff_settings: Unable to load editor-specified configuration file

Caused by:
    0: Failed to parse /private/tmp/hatch-test/ruff.toml
    1: TOML parse error at line 2, column 18
         |
       2 | extend-select = ["ASYNC101"]
         |                  ^^^^^^^^^^
       Unknown rule selector: `ASYNC101`
```


---

_Label `server` added by @dhruvmanila on 2025-01-13 09:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-13 09:48_

---

_Comment by @github-actions[bot] on 2025-01-13 09:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-13 10:11_

For reference: https://docs.rs/anyhow/latest/anyhow/struct.Error.html#display-representations

---

_Merged by @dhruvmanila on 2025-01-13 10:13_

---

_Closed by @dhruvmanila on 2025-01-13 10:13_

---

_Branch deleted on 2025-01-13 10:13_

---

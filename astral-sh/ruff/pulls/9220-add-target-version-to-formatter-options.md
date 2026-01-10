```yaml
number: 9220
title: "Add `target_version` to formatter options"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - formatter
assignees: []
merged: true
base: main
head: add-target-version-to-formatter-options
created_at: 2023-12-21T02:48:33Z
updated_at: 2023-12-21T04:17:46Z
url: https://github.com/astral-sh/ruff/pull/9220
synced_at: 2026-01-10T23:07:18Z
```

# Add `target_version` to formatter options

---

_Pull request opened by @MichaReiser on 2023-12-21 02:48_

## Summary

This PR adds the `target_version` option to the formatter option and initializes it with the `target_version` specified in the `ruff.toml`.

The new configuration doesn't yet affect any formatting. 

## Test Plan

I ran ruff manually in my test project and added a `dbg` statement to `to_format_options`. Changes to `target-version` in the configuration were reflected in the debug printed `PyFormatOptions`. Verified that using `requires-python` works as intended.


---

_Label `configuration` added by @MichaReiser on 2023-12-21 02:48_

---

_Label `formatter` added by @MichaReiser on 2023-12-21 02:48_

---

_@MichaReiser reviewed on 2023-12-21 02:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:383 on 2023-12-21 02:50_

It would be nice if we could have a single `PythonVersion` definitions shared by the linter and formatter but there's currently no good crate where to place it. Creating one feels overkill for such a simple definition. 

Ultimately I think the definition should be in `ruff_python_ast` where it specifies the language features.

---

_Review requested from @dhruvmanila by @MichaReiser on 2023-12-21 02:52_

---

_Comment by @github-actions[bot] on 2023-12-21 03:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/options.rs`:380 on 2023-12-21 03:13_

Should we add a comment stating that the default version should match with the `PythonVersion` in the `ruff_linter` crate?

---

_@dhruvmanila approved on 2023-12-21 03:14_

---

_@MichaReiser reviewed on 2023-12-21 04:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:380 on 2023-12-21 04:02_

Great idea. I added a comment to both `PythonVersion`s

---

_Merged by @MichaReiser on 2023-12-21 04:05_

---

_Closed by @MichaReiser on 2023-12-21 04:05_

---

_Branch deleted on 2023-12-21 04:06_

---

```yaml
number: 12006
title: "Error when using the `tab-size` option"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - configuration
  - cli
assignees: []
merged: true
base: ruff-0.5
head: remove-deprecated-tab-size-option
created_at: 2024-06-24T07:00:20Z
updated_at: 2024-06-25T07:36:05Z
url: https://github.com/astral-sh/ruff/pull/12006
synced_at: 2026-01-10T21:56:00Z
```

# Error when using the `tab-size` option

---

_Pull request opened by @MichaReiser on 2024-06-24 07:00_

## Summary

This PR makes the warning when using the `tab-size` option an error. The `tab-size` option was deprecated in Ruff 0.1.2 (released October 2023).

Part of https://github.com/astral-sh/ruff/issues/7650

## Test Plan

* Verified that the option isn't used by the LSP (I searched for tab-size and tab_size)
* Verified that the option isn't exposed via the CLI (other than using `--config`)
* Verified that using the previously accepted `tab-size` option fails

```shell
❯ ../ruff/target/debug/ruff check test.py --config tab-size=2
ruff failed
  Cause: The `tab-size` option has been renamed to `indent-width` to emphasize that it configures the indentation used by the formatter as well as the tab width. Please update your `--config` CLI arguments to use `indent-width = <value>` instead.

❯ ../ruff/target/debug/ruff check test.py 
ruff failed
  Cause: The `tab-size` option has been renamed to `indent-width` to emphasize that it configures the indentation used by the formatter as well as the tab width. Please update `pyproject.toml` to use `indent-width = <value>` instead.
```

* Verified that using `indent-width` works

```shell
❯ ../ruff/target/debug/ruff check test.py
warning: Detected debug build without --no-cache.
```

---

_Added to milestone `v0.5.0` by @MichaReiser on 2024-06-24 07:02_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-06-24 07:04_

---

_Marked ready for review by @MichaReiser on 2024-06-24 07:06_

---

_Label `breaking` added by @MichaReiser on 2024-06-24 08:53_

---

_Label `configuration` added by @MichaReiser on 2024-06-24 08:53_

---

_Label `cli` added by @MichaReiser on 2024-06-24 08:53_

---

_Comment by @github-actions[bot] on 2024-06-24 10:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-06-24 10:51_

---

_Renamed from "Remove deprecated `tab-size` option" to "Error when using the `tab-size` option" by @MichaReiser on 2024-06-25 07:20_

---

_Merged by @MichaReiser on 2024-06-25 07:36_

---

_Closed by @MichaReiser on 2024-06-25 07:36_

---

_Branch deleted on 2024-06-25 07:36_

---

```yaml
number: 22101
title: "[`flake8_print`] better suggestion for basicConfig in `T201` docs"
type: pull_request
state: merged
author: amyreese
labels:
  - documentation
assignees: []
merged: true
base: main
head: amy/t201-basicconfig
created_at: 2025-12-19T23:49:48Z
updated_at: 2026-01-05T19:42:49Z
url: https://github.com/astral-sh/ruff/pull/22101
synced_at: 2026-01-12T15:57:41Z
```

# [`flake8_print`] better suggestion for basicConfig in `T201` docs

---

_@amyreese_


<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

`logging.basicConfig` should not be called at a global module scope,
as that produces a race condition to configure logging based on which
module gets imported first.  Logging should instead be initialized
in an entrypoint to the program, either in a `main()` or in the
typical `if __name__ == "__main__"` block.

## Test Plan

tbd


---

_Review requested from @ntBre by @amyreese on 2025-12-19 23:50_

---

_Review requested from @MichaReiser by @amyreese on 2025-12-19 23:50_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 00:03_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `documentation` added by @MichaReiser on 2025-12-22 11:00_

---

_@MichaReiser approved on 2025-12-22 11:00_

---

_@MichaReiser reviewed on 2025-12-22 11:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_print/rules/print_call.rs`:48 on 2025-12-22 11:02_

I would probably use `if __name __ == "__main__"` here because a `main` function is entirely optional



```suggestion
/// if __name__ == "__main__":
///     logging.basicConfig(level=logging.INFO)
```

---

_@MichaReiser approved on 2025-12-22 11:02_

---

_@ntBre approved on 2025-12-22 19:05_

---

_Merged by @amyreese on 2026-01-05 19:42_

---

_Closed by @amyreese on 2026-01-05 19:42_

---

_Branch deleted on 2026-01-05 19:42_

---

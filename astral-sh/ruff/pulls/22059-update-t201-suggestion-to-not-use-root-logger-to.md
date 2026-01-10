```yaml
number: 22059
title: " Update T201 suggestion to not use root logger to satisfy LOG015"
type: pull_request
state: merged
author: cbachhuber
labels:
  - documentation
assignees: []
merged: true
base: main
head: cbachhuber-patch-1
created_at: 2025-12-18T20:10:21Z
updated_at: 2025-12-19T19:08:13Z
url: https://github.com/astral-sh/ruff/pull/22059
synced_at: 2026-01-10T16:36:18Z
```

#  Update T201 suggestion to not use root logger to satisfy LOG015

---

_Pull request opened by @cbachhuber on 2025-12-18 20:10_

## Summary

Currently, the proposed fix for https://docs.astral.sh/ruff/rules/print/ violates https://docs.astral.sh/ruff/rules/root-logger-call/. Thus, let's change the proposal to make LOG015 happy as well.

## Test Plan

Test manually in a project that has both T201 and LOG015 enabled and run them over the previous and proposed code. Is there continuous testing of the code snippets from the docs?


---

_@cbachhuber reviewed on 2025-12-18 20:10_

---

_Review comment by @cbachhuber on `crates/ruff_linter/src/rules/flake8_print/rules/print_call.rs`:40 on 2025-12-18 20:10_

This will prepend the file and log level to any log message. If that's not ok, we can 


```suggestion
/// logging.basicConfig(level=logging.INFO, format="%(message)s")
/// logger = logging.getLogger(__name__)
```

---

_Marked ready for review by @cbachhuber on 2025-12-18 20:11_

---

_Label `documentation` added by @ntBre on 2025-12-18 20:13_

---

_Review requested from @amyreese by @ntBre on 2025-12-18 20:13_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 20:22_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@amyreese approved on 2025-12-19 01:15_

I know why it's there, but IMO the suggested fix shouldn't include `logging.basicConfig()` at the global module level due to the race condition around which call "wins" based on which module is imported first.

Otherwise, I whole-heartedly agree with this change.

---

_Merged by @amyreese on 2025-12-19 19:08_

---

_Closed by @amyreese on 2025-12-19 19:08_

---

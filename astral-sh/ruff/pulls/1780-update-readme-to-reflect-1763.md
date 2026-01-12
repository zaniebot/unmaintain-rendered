```yaml
number: 1780
title: "Update readme to reflect #1763"
type: pull_request
state: merged
author: Czaki
labels: []
assignees: []
merged: true
base: main
head: fix_readme
created_at: 2023-01-11T11:07:30Z
updated_at: 2023-01-11T23:47:26Z
url: https://github.com/astral-sh/ruff/pull/1780
synced_at: 2026-01-12T05:36:32Z
```

# Update readme to reflect #1763

---

_Pull request opened by @Czaki on 2023-01-11 11:07_

When checking changes in the 0.0.218 release I noticed that auto fixing PT004 and PT005 was disabled but this change was not reflected in README. So I create this small PR to do this. 

---

_Comment by @charliermarsh on 2023-01-11 15:40_

These docs are actually auto-generated! Do you mind changing the structs in `src/violations.rs`? (`MissingFixtureNameUnderscore` and `IncorrectFixtureNameUnderscore` should be changed from `AlwaysAutofixableViolation` to `Violation`, and the `autofix_title` method should be removed.)

---

_Merged by @charliermarsh on 2023-01-11 23:37_

---

_Closed by @charliermarsh on 2023-01-11 23:37_

---

_Comment by @charliermarsh on 2023-01-11 23:37_

All good, I got it :)

---

_Branch deleted on 2023-01-11 23:47_

---

```yaml
number: 8592
title: Support local and dynamic class- and static-method decorators
type: pull_request
state: merged
author: ThiefMaster
labels:
  - configuration
assignees: []
merged: true
base: main
head: n80x-decorators-whitelist
created_at: 2023-11-09T23:25:04Z
updated_at: 2023-11-10T08:27:22Z
url: https://github.com/astral-sh/ruff/pull/8592
synced_at: 2026-01-10T23:40:55Z
```

# Support local and dynamic class- and static-method decorators

---

_Pull request opened by @ThiefMaster on 2023-11-09 23:25_

## Summary

This brings ruff's behavior in line with what `pep8-naming` already does and thus closes #8397.

I had initially implemented this to look at the last segment of a dotted path only when the entry in the `*-decorators` setting started with a `.`, but in the end I thought it's better to remain consistent w/ `pep8-naming` and doing a match against the last segment of the decorator name in any case.

If you prefer to diverge from this in favor of less ambiguity in the configuration let me know and I'll change it so you would need to put e.g. `.expression` in the `classmethod-decorators` list.

## Test Plan

Tested against the file in the issue linked below, plus the new testcase added in this PR.


---

_Comment by @github-actions[bot] on 2023-11-09 23:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-11-10 02:00_

I went back and forth on this a bit but ultimately I think it makes sense in the interest of pragmatism and matching user expectation.


---

_@charliermarsh approved on 2023-11-10 02:01_

Thanks @ThiefMaster! I just modified the PR to break the logic out into standalone functions, and we also now always run the match checks rather than only if it's _not_ an import.

---

_Label `configuration` added by @charliermarsh on 2023-11-10 02:01_

---

_Renamed from "Fix {class,static}method-decorators for dotted names" to "Support local and dynamic class- and static-method decorators" by @charliermarsh on 2023-11-10 02:01_

---

_Merged by @charliermarsh on 2023-11-10 02:04_

---

_Closed by @charliermarsh on 2023-11-10 02:04_

---

_Branch deleted on 2023-11-10 08:27_

---

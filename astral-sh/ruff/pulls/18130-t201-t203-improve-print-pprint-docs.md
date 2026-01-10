```yaml
number: 18130
title: T201/T203 Improve print/pprint docs
type: pull_request
state: merged
author: dragon-dxw
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-05-16T11:59:59Z
updated_at: 2025-05-18T16:40:42Z
url: https://github.com/astral-sh/ruff/pull/18130
synced_at: 2026-01-10T18:51:01Z
```

# T201/T203 Improve print/pprint docs

---

_Pull request opened by @dragon-dxw on 2025-05-16 11:59_


* discuss legitimate uses like CLI
* discuss how to log

## Summary
These rules are great when referring to adhoc debug print statements, but it's worth flagging up that there are absolutely fine legitimate uses of `print()` and `pprint()` and that the rule applies to debugging more or less exclusively.

It's not easy to come up with a simple, minimal example of what using `logging` looks like; I'm not convinced this one is great.

I'm not sure this is quite right, but I think there's a kernel of something better than what we've got, which is important because this appears to becoming the defacto reference for "why print in production code is bad"

## Test Plan

It hasn't been. But I did check the python code fragments didn't error on ruff.


---

_Label `documentation` added by @ntBre on 2025-05-16 12:55_

---

_Comment by @github-actions[bot] on 2025-05-16 13:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-05-18 16:15_

Thanks. This is great

---

_Merged by @MichaReiser on 2025-05-18 16:40_

---

_Closed by @MichaReiser on 2025-05-18 16:40_

---

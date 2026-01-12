```yaml
number: 14551
title: "[`perflint`] - Fix `manual-list-comprehension` for async generators (`PERF401`)"
type: pull_request
state: closed
author: diceroll123
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix-async-for-perf401
created_at: 2024-11-23T02:52:58Z
updated_at: 2025-01-15T10:50:16Z
url: https://github.com/astral-sh/ruff/pull/14551
synced_at: 2026-01-12T15:55:48Z
```

# [`perflint`] - Fix `manual-list-comprehension` for async generators (`PERF401`)

---

_@diceroll123_

## Summary

Fixes a current bug in PERF401's preview autofix, where it tries to make an async generator within a list.extend, without wrapping the generator in a list, which does not implement `__iter__` which makes it an error.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-11-23 02:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @diceroll123 on 2024-11-23 07:10_

[Found a weird issue with comments](https://play.ruff.rs/9d4b2182-b10b-4c1f-a584-4b40999e90c0), this was already present in the autofix in `0.8.0`, but I decided to fix it and put it in here too.

---

_Comment by @Skylion007 on 2024-11-23 16:41_

@w0nder1ng Another edge case. May want to merge this PR with #14369 

---

_Comment by @diceroll123 on 2024-11-23 22:53_

Hm, yeah, my implementation does not do well with this one:

```py
def f():
    # make sure that `tmp` is not deleted
    tmp = 1; result = [] # commment should be protected
    for i in range(10):
        result.append(i + 1) # PERF401
```



---

_Label `bug` added by @dylwil3 on 2024-11-26 15:56_

---

_Label `fixes` added by @dylwil3 on 2024-11-26 15:56_

---

_Comment by @MichaReiser on 2024-12-12 08:12_

@diceroll123 #14369 is now merged. Would you mind rebasing your PR?

---

_@MichaReiser requested changes on 2024-12-12 08:13_

Needs rebase

---

_Comment by @diceroll123 on 2025-01-15 10:18_

Woop, destroyed the commit here by accident, but after fixing it locally and coming back from rebase hell, it turns out all of the issues my PR solved were solved by 14369, so I'll close this. Thanks all! 😄

---

_Closed by @diceroll123 on 2025-01-15 10:18_

---

_Comment by @MichaReiser on 2025-01-15 10:50_

Thank you and sorry for the rebase struggles

---

```yaml
number: 14832
title: "[`ruff`] Do not simplify `round()` calls (`RUF046`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF046-1
created_at: 2024-12-08T04:15:26Z
updated_at: 2024-12-09T16:39:14Z
url: https://github.com/astral-sh/ruff/pull/14832
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`] Do not simplify `round()` calls (`RUF046`)

---

_@InSyncWithFoo_

## Summary

Part 1 of the big change introduced in #14828. This temporarily causes all fixes for `round(...)` to be considered unsafe, but they will eventually be enhanced.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-08 04:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF046_RUF046.py.snap`:487 on 2024-12-08 17:41_

This now seems incorrect, considering that it is in the "no errors" section

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:123 on 2024-12-08 17:42_

I think this now removes too much. We still need to validate that the `round` call returns a number, that is, that it has an `number` argument and the `ndigits` argument is either absent, `None`, or `0`. 



---

_@MichaReiser requested changes on 2024-12-08 17:43_

Thanks for splitting the PR!

I think the rule is now over eager in removing `int` calls from `rount`

---

_Comment by @InSyncWithFoo on 2024-12-08 21:29_

For reference, here's the logic table for `round()` cases:

|     `number`     |   `ndigits`   |  `round()`   | Error/fix |
|:----------------:|:-------------:|:------------:|:---------:|
|  Literal `int`   |   Not given   |    `int`     |   Safe    |
|  Literal `int`   | Literal `int` |    `int`     |   Safe    |
|  Literal `int`   |    `None`     |    `int`     |   Safe    |
|  Literal `int`   |     Other     |    Other     |     -     |
| Literal `float`  |   Not given   |    `int`     |   Safe    |
| Literal `float`  | Literal `int` |   `float`    |     -     |
| Literal `float`  |    `None`     |    `int`     |   Safe    |
| Literal `float`  |     Other     |    Other     |     -     |
|  Inferred `int`  |   Not given   |    `int`     |  Unsafe   |
|  Inferred `int`  | Literal `int` |    `int`     |  Unsafe   |
|  Inferred `int`  |    `None`     |    `int`     |  Unsafe   |
|  Inferred `int`  |     Other     |    Other     |     -     |
| Inferred `float` |   Not given   |    `int`     |  Unsafe   |
| Inferred `float` | Literal `int` |   `float`    |     -     |
| Inferred `float` |    `None`     |    `int`     |  Unsafe   |
| Inferred `float` |     Other     |    Other     |     -     |
|      Other       |   Not given   | Likely `int` |  Unsafe   |
|      Other       | Literal `int` |    Other     |     -     |
|      Other       |    `None`     | Likely `int` |  Unsafe   |
|      Other       |     Other     |    Other     |     -     |

---

_Comment by @MichaReiser on 2024-12-09 08:26_

Nice table: From testing a few cases, it seems that `round(4, ndigits=10)` always returns an `int`

```
>>> round(4, 10)
4
>>> type(_)
<class 'int'>
```

But we should not create a diagnostic if `ndigits` isn't an integer, e.g. `round(4, 10.4)`

---

_Label `rule` added by @MichaReiser on 2024-12-09 08:26_

---

_Label `preview` added by @MichaReiser on 2024-12-09 08:26_

---

_@MichaReiser approved on 2024-12-09 08:31_

Nice. This looks good now, except that we can also provide a safe fix if the number is an int, and `ndigits` is any integer.

---

_Comment by @InSyncWithFoo on 2024-12-09 13:56_

Logic changed and table updated. `round(0, integer)` and `round(inferred_integer, integer)` are now both fixable, safely and unsafely correspondingly.

---

_Comment by @MichaReiser on 2024-12-09 15:51_

Perfect, thank you

---

_Merged by @MichaReiser on 2024-12-09 15:51_

---

_Closed by @MichaReiser on 2024-12-09 15:51_

---

_Branch deleted on 2024-12-09 16:39_

---

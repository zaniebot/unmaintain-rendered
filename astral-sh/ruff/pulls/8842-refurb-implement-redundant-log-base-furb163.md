```yaml
number: 8842
title: "[`refurb`] Implement `redundant-log-base` (`FURB163`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: redundant-log-base
created_at: 2023-11-26T19:19:09Z
updated_at: 2023-11-28T00:23:43Z
url: https://github.com/astral-sh/ruff/pull/8842
synced_at: 2026-01-10T23:40:55Z
```

# [`refurb`] Implement `redundant-log-base` (`FURB163`)

---

_Pull request opened by @tjkuson on 2023-11-26 19:19_

## Summary

Implement [`simplify-math-log`](https://github.com/dosisod/refurb/blob/master/refurb/checks/math/simplify_log.py) as `redundant-log-base` (`FURB163`).

Auto-fixes

```python
import math

math.log(2, 2)
```

to

```python
import math

math.log2(2)
```

Related to #1348.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-11-26 19:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @tjkuson on 2023-11-26 19:36_

---

_@zanieb reviewed on 2023-11-27 15:43_

---

_Review comment by @zanieb on `crates/ruff_linter/src/codes.rs`:958 on 2023-11-27 15:43_

New rules should be added in preview :)

```suggestion
        (Refurb, "163") => (RuleGroup::Preview, rules::refurb::rules::RedundantLogBase),
```

---

_@zanieb approved on 2023-11-27 15:43_

Nice! This lgtm except the rule needs to go through preview.

---

_@tjkuson reviewed on 2023-11-27 15:45_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/codes.rs`:958 on 2023-11-27 15:45_

Oops, must've missed that. Apologies!

---

_Comment by @bluetech on 2023-11-27 17:57_

IMO the term "redundant" is not quite right (except the `math.log(x, math.e)` case). The reason to prefer `math.log2(x)` over `math.log(x, 2)` is floating-point accuracy, as mentioned in the [function docs](https://docs.python.org/3/library/math.html#math.log2).

---

_Comment by @tjkuson on 2023-11-27 18:21_

Yeah, that's fair. I did think about which name to use when porting the rule, but never found one I fully liked (it has to both describe both the `math.log(foo, math.e)` case and the `math.log(foo, 2)` case). I settled for redundant log base to mean 'there is a way of writing this logarithmic function without specifying a base' instead of 'just remove the base argument'. 

I also tried to make the rule message be explicit with what its recommendation is, to guard against confusion.

```
FURB163.py:15:1: FURB163 [*] Replace with `math.log2(1)`
```

Happy for name to changed, though. I'm not a huge fan of it.

Also, for what it's worth, the corresponding refurb rule [`simplify-math-log`](https://github.com/dosisod/refurb/blob/master/refurb/checks/math/simplify_log.py) cites readability, not performance, as the motivation. I cited both readability and precision when porting the rule in the documentation (as well as efficiency; I found the fixes ran faster).

EDIT: Maybe `superfluous-log-base`?

EDIT 2: 'Accuracy', not 'precision', is the correct term to use here.

---

_Comment by @zanieb on 2023-11-27 19:07_

I think the name is fine. You could use `imprecise-log-base` if you really wanted but I imagine for most users the floating point accuracy is not going to be critical.

---

_Comment by @bluetech on 2023-11-27 22:20_

Got it on the name. So my only nit is: in the rule description, replace "precise" with "accurate"; both `math.log` and `math.log2` have the same FP precision (double), but the latter is more accurate (for base 2).

---

_@bluetech reviewed on 2023-11-27 22:23_

---

_Review comment by @bluetech on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:24 on 2023-11-27 22:23_

Another suggestion: use another value than `2` for the x, to make it clearly distinguishable from the base `2`.

---

_@tjkuson reviewed on 2023-11-27 23:34_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:24 on 2023-11-27 23:34_

Changed to `3`.

---

_@charliermarsh approved on 2023-11-27 23:44_

---

_Label `rule` added by @charliermarsh on 2023-11-27 23:44_

---

_Label `preview` added by @charliermarsh on 2023-11-27 23:44_

---

_Merged by @charliermarsh on 2023-11-27 23:57_

---

_Closed by @charliermarsh on 2023-11-27 23:57_

---

_Branch deleted on 2023-11-28 00:23_

---

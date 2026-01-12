```yaml
number: 1923
title: "Add flake8-pie PIE796: prefer-unique-enum"
type: pull_request
state: merged
author: ljesparis
labels: []
assignees: []
merged: true
base: main
head: flake8-pie796
created_at: 2023-01-16T21:48:00Z
updated_at: 2023-01-17T00:30:29Z
url: https://github.com/astral-sh/ruff/pull/1923
synced_at: 2026-01-12T05:36:32Z
```

# Add flake8-pie PIE796: prefer-unique-enum

---

_Pull request opened by @ljesparis on 2023-01-16 21:48_

I accept any suggestion. By the way, I have a doubt, I have checked and all flake8-pie plugins can be fixed by ruff, but is it necessary that this one is also fixed automatically ?

rel #1543 

---

_Comment by @charliermarsh on 2023-01-16 21:59_

Awesome, thank you -- I'll review this tonight!

> ...but is it necessary that this one is also fixed automatically ?

I think in this case we probably _shouldn't_ autofix it (even though we could), since there's not a clearly-correct way to resolve the error. That is, it's not clear _which_ enum member should be removed, so we should probably just leave it.


---

_@charliermarsh reviewed on 2023-01-16 22:01_

---

_Review comment by @charliermarsh on `src/rules/flake8_pie/rules.rs`:136 on 2023-01-16 22:01_

Could we instead use...

```rs
let comparable_value: ComparableExpr = (&value).into();
if !seen_targets.insert(comparable_value) {
  ...
}
```

Not exact code, just pseudo code. But this would detect any duplicate values, not just constants.

If we _do_ want to leave it as constants, can we change the `ExprKind::Constant` match to use `ExprKind::Constant { value: Constant::Str(value), .. }`? And remove the `value.to_string()`? That way, we'll _only_ match on strings, which I think better matches the desired semantics.

---

_@charliermarsh reviewed on 2023-01-16 22:05_

---

_Review comment by @charliermarsh on `src/violations.rs`:5978 on 2023-01-16 22:05_

I know this is copied over from `flake8-pie` but I think we can do something more descriptive...

Can we change `pub struct PreferUniqueEnums` to:

```rs
pub struct PreferUniqueEnums {
  pub value: String
}
```

Then, change this to, like: `format!("Enum contains duplicate value: `{value}`")`.

In `prefer_unique_enums`, you can pass `violations::PreferUniqueEnums(unparse_expr(&values[i], checker.stylist))` to get the formatted value.

Does that seem ok? Let me know if you have any questions!


---

_@ljesparis reviewed on 2023-01-16 23:21_

---

_Review comment by @ljesparis on `src/rules/flake8_pie/rules.rs`:136 on 2023-01-16 23:21_

good one, i didn't know about **ComparableExpr**  😅. It makes everything a bit more simple.

---

_@ljesparis reviewed on 2023-01-16 23:24_

---

_Review comment by @ljesparis on `src/violations.rs`:5978 on 2023-01-16 23:24_

it seems more explicit, which is a good thing.  I've uploaded the review changes 👍🏿 

---

_Merged by @charliermarsh on 2023-01-17 00:27_

---

_Closed by @charliermarsh on 2023-01-17 00:27_

---

_@charliermarsh reviewed on 2023-01-17 00:30_

---

_Review comment by @charliermarsh on `src/rules/flake8_pie/rules.rs`:122 on 2023-01-17 00:30_

I tweaked this logic to use `resolve_call_path`, which validates that the expression is bound to `enum.Enum` (i.e., it tracks imports, follows aliases, and so on).

---

_@charliermarsh reviewed on 2023-01-17 00:30_

---

_Review comment by @charliermarsh on `src/violations.rs`:5976 on 2023-01-17 00:30_

(Tweaked this to use named fields which we're trying to prefer going forwards.)

---

_Comment by @charliermarsh on 2023-01-17 00:30_

Thank you! Made a few small changes prior to merging (left notes).

---

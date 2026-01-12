```yaml
number: 14398
title: Suggestion to improve TRY003 rule
type: issue
state: open
author: Booplicate
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-17T13:19:49Z
updated_at: 2026-01-02T05:11:10Z
url: https://github.com/astral-sh/ruff/issues/14398
synced_at: 2026-01-12T15:54:53Z
```

# Suggestion to improve TRY003 rule

---

_@Booplicate_

### Issue
I want to propose to change how TRY003 works with some of the builtin exceptions. While I like how the rule applies to custom exceptions and "base" `Exception`, I think it's incorrect in the case of errors like `TypeError` and `ValueError`. It's a common pattern and is widely recommended to use those exceptions when type or value don't match the expectations.
[Here](https://github.com/astral-sh/ruff/issues/2246#issuecomment-1406479505) you suggest to define a subclass of `Exception`, but not only defining a separate exception for _every_ case is redundant and would bloat the code, I'm pretty sure using `ValueError` would _more correct_.

You already ignore `NotImplementedError`, which was a decision in the right direction! It doesn't make sense to subclass it every time. I think the same applies for at least `ValueError` and `TypeError`.

### Solution

Currently you have such code to ignore `NotImplementedError`:
https://github.com/astral-sh/ruff/blob/abb34828bdbce0eff28d4350b8f188436e57be78/crates/ruff_linter/src/rules/tryceratops/rules/raise_vanilla_args.rs#L71-L78
sadly it seems that `match_builtin_expr` can only take a single `symbol`. While we could call it multiple times, I think it'd be slightly ineffective? A better approach might be changing this method (or defining a new one), so it can take an array of symbols to match against.

### Info

Related issues: #2246 #6528

Sample code:
```py
def compute(a: int, b: int) -> int:
     if a <= 0:
         raise ValueError("a must be > 0")  # Ruff: Avoid specifying long messages outside the exception class [TRY003]
     if not b:
         raise ValueError("b must be != 0")  # Ruff: Avoid specifying long messages outside the exception class [TRY003]
     return a + b
```

`ruff` command: `ruff check --no-fix`

`ruff` version: 0.7.4

`ruff` settings:
```toml
[tool.ruff.lint]
select = ["TRY003"]
```



---

_Label `rule` added by @dylwil3 on 2024-11-17 18:47_

---

_Label `needs-decision` added by @dylwil3 on 2024-11-17 18:47_

---

_Comment by @bcyran on 2025-05-22 05:55_

I think the most universal solution would be to make the ignored exceptions list configurable. But I certainly support adding `ValueError` and `TypeError` to the ignored values by default.

---

_Comment by @edwinvehmaanpera on 2025-09-20 20:57_

Full configurability would be ideal, but if not then I would suggest also adding `AssertionError` to the ignored exceptions.

---

_Comment by @bedlamzd on 2025-11-04 21:42_

Another candidate is `ExceptionGroup`

---

_Comment by @k4lizen on 2026-01-02 05:11_

I came to raise an issue about `AssertionError` and found this. I think its completely uncontraversial that `AssertionError` should be added alongside `NotImplementedError`.

---

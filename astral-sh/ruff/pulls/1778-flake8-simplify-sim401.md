```yaml
number: 1778
title: "flake8_simplify : SIM401"
type: pull_request
state: merged
author: chammika-become
labels: []
assignees: []
merged: true
base: main
head: simplify3
created_at: 2023-01-11T03:35:09Z
updated_at: 2023-01-12T15:40:37Z
url: https://github.com/astral-sh/ruff/pull/1778
synced_at: 2026-01-12T05:36:32Z
```

# flake8_simplify : SIM401

---

_Pull request opened by @chammika-become on 2023-01-11 03:35_

Ref #998 

- Implements SIM401 with fix
- Added tests

Notes: 
- only recognize simple ExprKind::Name variables in expr patterns for now
- bug-fix from reference implementation: check 3-conditions (dict-key, target-variable, dict-name) to be equal, `flake8_simplify` only test first two (only first in second pattern)

@charliermarsh I would like to handle cases marked nice-to-have (they are handled in `flake8_simplify` by flattening the expr into strings). Appreciate, any pointers in improving helper `match_name_expr` for more complex exprs.

---

_@charliermarsh reviewed on 2023-01-11 03:50_

---

_Review comment by @charliermarsh on `src/ast/helpers.rs`:733 on 2023-01-11 03:50_

Check out `src/ast/comparable.rs`!

You can do:

```rs
let comparable_value: ComparableExpr = (&expr).into();
```

Then you can compare two expressions ignoring their locations. It's like this function, but generalized.

---

_@chammika-become reviewed on 2023-01-11 04:04_

---

_Review comment by @chammika-become on `src/ast/helpers.rs`:733 on 2023-01-11 04:04_

Oh! great.

---

_Marked ready for review by @chammika-become on 2023-01-11 07:28_

---

_Comment by @chammika-become on 2023-01-11 07:37_

@charliermarsh please review.
I am not entirely happy the way a new `match_comp_expr` turn out to be. I could get rid of this function, if I could directly convert `&Located<ExprKind>` => `&ComparableExpr` . For example,
```rust
// test_key : &Box<Located<ExprKind>>
 let test_key_comp: &ComparableExpr = (&test_key).into(); // <= failed
```
Hope you can improve on that.

---

_Merged by @charliermarsh on 2023-01-12 00:51_

---

_Closed by @charliermarsh on 2023-01-12 00:51_

---

_Comment by @charliermarsh on 2023-01-12 00:51_

Thank you!

I wasn't able to remove that method entirely, but I was able to simplify it a bit.

---

_Comment by @chammika-become on 2023-01-12 10:08_

@charliermarsh some afterthought... we are not going to flag this at all if there are comments in the `stmt` ? If that's the case we can move `has_comments()` check all the way up to the top.

---

_Comment by @charliermarsh on 2023-01-12 15:40_

@chammika-become - Yeah, it's really hard to preserve comments correctly since there are so many cases to handle (above the statement, below the statement, inline, etc.), so for now, I'm just skipping commented blocks when collapsing multi- to single-line statements. We do the same for the ternary-rewrite rule.

I prefer keeping `has_comments` lower as its more computationally expensive (we have to re-tokenize the block).


---

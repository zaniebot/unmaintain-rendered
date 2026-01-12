```yaml
number: 19934
title: "[`perflint`] Handle tuples in dictionary comprehensions (`PERF403`)"
type: pull_request
state: merged
author: liortct
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: bugfix/perf403-tuple-error
created_at: 2025-08-16T09:03:30Z
updated_at: 2025-08-28T21:44:43Z
url: https://github.com/astral-sh/ruff/pull/19934
synced_at: 2026-01-12T15:56:50Z
```

# [`perflint`] Handle tuples in dictionary comprehensions (`PERF403`)

---

_@liortct_

This pull request fixes the bug described in issue [#19153](https://github.com/astral-sh/ruff/issues/19153).

The issue occurred when `PERF403` incorrectly flagged cases involving tuple unpacking in a for loop. For example:

```python
def f():
    v = {}
    for (o, p), x in [("op", "x")]:
        v[x] = o, p
```

This code was wrongly suggested to be rewritten into a dictionary comprehension, which changes the semantics.

Changes in this PR:

Updated the `PERF403` rule to correctly handle tuple unpacking in loop targets.

Added regression tests to ensure this case (and similar ones) are no longer flagged incorrectly.

Why:
This ensures that `PERF403` only triggers when a dictionary comprehension is semantically equivalent to the original loop, preventing false positives.

---

_Comment by @liortct on 2025-08-18 19:39_

@ntBre 
Hello 👋 just a gentle reminder about this PR. I’d be happy to make any changes if needed. Thanks for reviewing when you get the chance!

---

_Review requested from @ntBre by @ntBre on 2025-08-18 22:28_

---

_Label `bug` added by @ntBre on 2025-08-18 22:28_

---

_Label `fixes` added by @ntBre on 2025-08-18 22:28_

---

_Comment by @ntBre on 2025-08-18 22:29_

Thank you! I'll try to get to it this week :)

---

_Comment by @github-actions[bot] on 2025-08-18 22:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:364 on 2025-08-19 17:25_

What do you think about something like this?


```suggestion
    let key_str = if let Expr::Tuple(ast::ExprTuple {
        elts,
        parenthesized,
        ..
    }) = key
    {
        if elts.len() != 1 {
            return None;
        };

        if *parenthesized {
            locator.slice(key).to_string()
        } else {
            format!("({})", locator.slice(key))
        }
    } else {
        locator.slice(key).to_string()
    };
```

That uses the same kind of `parenthesized` check as for `value_str`. I also used a small trick with `slice`: `Expr`s implement `Ranged` directly, so you can just pass them instead of having to call `key.range()`, for example. It also restores the `elts` check, although I don't really see why it shouldn't apply if there are multiple elements in the slice. That's also checked here, but I guess we might as well preserve it:

https://github.com/astral-sh/ruff/blob/59b078b1bf3a9482e08fe5a00a41076efbbc3974/crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs#L130-L139

It's a bit unfortunate to have to call `format!` and `to_string` everywhere, but I believe that is the typical way to add parentheses when needed. We _could_ use a `Cow` if we really want to avoid it, but I think it's probably okay.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:374 on 2025-08-19 17:26_

Similar to the case above, I think we can just do this instead of having a mutable variable:


```suggestion
    // If the value is a tuple without parentheses, add them
    let value_str = if let Expr::Tuple(ast::ExprTuple {
        parenthesized: false,
        ..
    }) = value
    {
        format!("({})", locator.slice(value))
    } else {
        locator.slice(value).to_string()
    };
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/perflint/PERF403.py`:201 on 2025-08-19 17:27_

Your implementation already covers this (nice work!), but we may also want to add a test for the case where the key is already parenthesized just to make sure we don't add a second set:

```py
def issue_19153_3():
    v = {}
    for o, (x,) in ["ox"]:
        v[(x,)] = o
    return v
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:376 on 2025-08-19 17:29_

This call already existed, but if we wanted to avoid one string allocation, we could fold this into the `format!` below:

```rust
let comprehension_str = format!("{{{key_str}: {value_str} {for_type} {target_str} in {iter_str}{if_str}}}");
```

---

_@ntBre reviewed on 2025-08-19 17:31_

Thank you, this looks great! I just had a few small suggestions.

---

_@liortct reviewed on 2025-08-23 10:33_

---

_Review comment by @liortct on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:364 on 2025-08-23 10:33_

Thanks for the comment, Fixed!

---

_@liortct reviewed on 2025-08-23 10:33_

---

_Review comment by @liortct on `crates/ruff_linter/resources/test/fixtures/perflint/PERF403.py`:201 on 2025-08-23 10:33_

You are absolutely right, I added this test case.

---

_@liortct reviewed on 2025-08-23 10:34_

---

_Review comment by @liortct on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:374 on 2025-08-23 10:34_

Fixed

---

_@liortct reviewed on 2025-08-23 10:37_

---

_Review comment by @liortct on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:376 on 2025-08-23 10:37_

Nice tip, thanks.

---

_Comment by @liortct on 2025-08-23 10:40_

@ntBre 
Thanks a lot for the review and comments!  
I’ve addressed all the issues, and the PR is ready for another review.  
If there’s anything else that needs to be adjusted, please let me know and I’ll update it promptly.  

Thanks again!

---

_@ntBre approved on 2025-08-28 21:10_

Thank you!

---

_Renamed from "[perf403] bug handle tuple in dictionary compression" to "[`perflint`] Handle tuples in dictionary comprehensions (`PERF403`)" by @ntBre on 2025-08-28 21:22_

---

_Merged by @ntBre on 2025-08-28 21:37_

---

_Closed by @ntBre on 2025-08-28 21:37_

---

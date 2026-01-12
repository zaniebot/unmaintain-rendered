```yaml
number: 5987
title: "`PLR1714` doesn't check for Yoda conditions"
type: issue
state: closed
author: dosisod
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-07-22T20:48:27Z
updated_at: 2023-08-23T03:45:32Z
url: https://github.com/astral-sh/ruff/issues/5987
synced_at: 2026-01-12T15:54:45Z
```

# `PLR1714` doesn't check for Yoda conditions

---

_@dosisod_

See https://github.com/astral-sh/ruff/issues/1348#issuecomment-1646520368.

Ruff doesn't detect Yoda conditions for the following:

```python
x = ""

_ = x == "abc" or "def" == x
_ = "abc" == x or "def" == x
_ = "abc" == x or x == "def"
```

Ruff doesn't output anything.

For comparison, Refurb outputs errors for each line:

```
$ refurb x.py
x.py:3:5 [FURB108]: Replace `x == y or z == x` with `x in (y, z)`
x.py:4:5 [FURB108]: Replace `x == y or z == y` with `y in (x, z)`
x.py:5:5 [FURB108]: Replace `x == y or y == z` with `y in (x, z)`
```

If you disable/don't fix the Yoda errors you won't get the `PLR1714` errors. If you do fix the Yoda errors, you will get the proper/expected error, `PLR1714`.

---

_Comment by @tjkuson on 2023-07-22 21:02_

Are Yoda conditions like this common? I had an implementation of this rule that did consider Yoda conditions, but it was more complicated and had a performance hit. Though, if people use Yoda conditions, I could open a PR to re-add that logic.

---

_Comment by @dosisod on 2023-07-22 21:16_

I didn't see that you explicitly mentioned Yoda conditions in your PR, thanks for pointing that out. I can't give you an exact number on how many people use Yoda conditions, I was more concerned with handling all the cases as opposed to the canonical ones (and, Refurb doesn't detect Yoda conditions).

For what it's worth, this is how Refurb handles the Yoda case checking ([link](https://github.com/dosisod/refurb/blob/71a2d3dc97bc1a5a4e29d04bc5454e4f16c19738/refurb/checks/common.py#L187-L217)):

```python
def get_common_expr_in_comparison_chain(
    node: OpExpr, oper: str, cmp_oper: str = "=="
) -> tuple[Expression, tuple[int, int]] | None:
    """
    This function finds the first expression shared between 2 comparison
    expressions in the binary operator `oper`.

    For example, an OpExpr that looks like the following:

    1 == 2 or 3 == 1

    Will return a tuple containing the first common expression (`IntExpr(1)` in
    this case), and the indices of the common expressions as they appear in the
    source (`0` and `3` in this case). The indices are to be used for display
    purposes by the caller.

    If the binary operator is not composed of 2 comparison operators, or if
    there are no common expressions, `None` is returned.
    """

    match extract_binary_oper(oper, node):
        case (
            ComparisonExpr(operators=[lhs_oper], operands=[a, b]),
            ComparisonExpr(operators=[rhs_oper], operands=[c, d]),
        ) if (
            lhs_oper == rhs_oper == cmp_oper
            and (indices := get_common_expr_positions(a, b, c, d))
        ):
            return a, indices

    return None  # pragma: no cover
```

---

_Comment by @tjkuson on 2023-07-22 22:37_

IIRC, the ruff implementation uses a hash map, where each key maps an LHS value to a vector of RHS values. This was efficient but difficult to make work with Yoda conditions.

---

_Comment by @tjkuson on 2023-07-22 22:41_

Actually, there is value in the rule being order-agnostic beyond just supporting Yoda conditions. I don't believe the current implementation will emit a warning on the following, entirely plausible code.

```python
foo == bar or baz == foo or qux == foo
```

---

_Label `bug` added by @charliermarsh on 2023-07-23 00:15_

---

_Label `accepted` added by @charliermarsh on 2023-07-23 00:15_

---

_Comment by @zanieb on 2023-08-14 17:53_

@tjkuson are you interested in addressing the yoda condition issue and/or the additional issue you raised?

---

_Comment by @tjkuson on 2023-08-14 18:21_

Sure! I wasn't able to find a good solution before, but the issue has been open for a while, and I am probably better at Rust now, so would be interested in having a go this week.

---

_Closed by @charliermarsh on 2023-08-23 03:45_

---

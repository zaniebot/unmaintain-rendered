```yaml
number: 6933
title: Can omit parentheses layout for patterns
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - accepted
  - style
assignees: []
created_at: 2023-08-28T07:37:13Z
updated_at: 2024-09-26T06:35:23Z
url: https://github.com/astral-sh/ruff/issues/6933
synced_at: 2026-01-10T11:09:49Z
```

# Can omit parentheses layout for patterns

---

_Issue opened by @MichaReiser on 2023-08-28 07:37_

Ruff avoids adding parentheses to expressions that start or end with a parenthesized node (see `can_omit_parentheses`). 

This is implemented by using `optional_parentheses` in conjunction with the `in_parentheses_only_*` builders. We need something similar for patterns to support 

```python
match test:
    case A | B | ["Aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", "bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb"]: ...

# Formatted
match test:
    case (
        A
        | B
        | ["Aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", "bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb"]
    ):
        ...

# Instead of
match test:
    case A | B | [
        "Aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
        "bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb",
    ]:
        ...

```

---

_Label `formatter` added by @MichaReiser on 2023-08-28 07:51_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-28 07:51_

---

_Comment by @dhruvmanila on 2023-09-25 04:46_

This seems to be fixed: https://play.ruff.rs/e7f95918-d71e-479f-8fae-29cdaa476463

Closing the issue but feel free to re-open it if you think otherwise.

---

_Closed by @dhruvmanila on 2023-09-25 04:47_

---

_Comment by @MichaReiser on 2023-09-25 07:14_

This isn't fixed yet. Notice how the expression gets wrapped in parentheses.

We need to avoid adding the parentheses in this case. I'm not sure why the function isn't using `maybe_parenthesize(IfBreaks)` directly

https://github.com/astral-sh/ruff/blob/c05e4628b1d385d106e7e3d355f5d1d76fbfe401/crates/ruff_python_formatter/src/other/match_case.rs#L38-L58

---

_Reopened by @MichaReiser on 2023-09-25 07:14_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 14:39_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-27 14:39_

---

_Comment by @charliermarsh on 2023-09-27 14:40_

We want to make this change (it improves consistency), but it's not blocking for the Beta.

The challenge here is that we need to re-implement `can_omit_parentheses` for patterns, since that logic only applies to expressions.


---

_Label `accepted` added by @charliermarsh on 2023-09-27 14:40_

---

_Comment by @MichaReiser on 2024-02-08 14:31_

I'm going to remove this from the stable release since it hasn't come up often. 

---

_Label `style` added by @MichaReiser on 2024-02-08 14:32_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-02-08 14:32_

---

_Closed by @MichaReiser on 2024-09-26 06:35_

---

```yaml
number: 6750
title: "Formatter: inconsistency for parenthesized `with` with trailing comment"
type: issue
state: closed
author: dhruvmanila
labels:
  - formatter
  - accepted
assignees: []
created_at: 2023-08-22T03:06:05Z
updated_at: 2023-09-28T06:40:21Z
url: https://github.com/astral-sh/ruff/issues/6750
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: inconsistency for parenthesized `with` with trailing comment

---

_@dhruvmanila_

Given:
```python
with (
    open('file')  # trailing
):
    pass


while (
    True  # trailing
):
    pass


match (
    some  # trailing
):
    case (
        pattern  # trailing
    ):
        pass
```

Formatted as:
```python
with open("file"):  # trailing
    pass


while (
    True  # trailing
):
    pass


match (
    some  # trailing
):
    case (
        pattern  # trailing
    ):
        pass

```

Notice that for `with` and `case` pattern the formatting is not collapsed while it does happen for the rest. The `case` pattern formatting was adopted from `with` which is the reason they both are the same.

I think the reason is that the nodes are different i.e, `WithItem` and `Pattern*` for `with` and `case` pattern while it's an expression for `while` which can utilize the `maybe_parenthesized_expression` helper directly.

https://play.ruff.rs/6cacd9e5-03dc-4da7-b185-fb45eb264be4

---

_Comment by @dhruvmanila on 2023-08-22 03:08_

I'm not sure if this is intentional but if it is then for `case` pattern we should use the same formatting style as `while` i.e, collapse if < line-length.

Also, this is what Black does: https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AE2AJxdAD2IimZxl1N_WmufDs0Lm7j3vYVv-fyEpjcGjBT5by7AjxamaYxYeXCngIOH5QOeUk1iGkcjENXoEKV2cVzP_u3sv7VfD5lAxtwFCY23EQCvVGAvRPjLiEhAwVufltiyjII1kYitIFme5SB59fpPGx3iYamtMmF8SVyHBiIH-3KV8PmEYg2CJ_nu-AHtt1oWkU5wBozrLxhgr3WuOQBS1mEMOm0c5AABuAG3AgAAOM6Ni7HEZ_sCAAAAAARZWg%3D%3D

---

_Label `formatter` added by @dhruvmanila on 2023-08-22 03:08_

---

_Comment by @charliermarsh on 2023-08-22 03:12_

I _believe_ we want to collapse all of these (i.e., the `while` formatting is correct). We should only expand if there's `has_trailing_own_line`, not _any_ `own_line`.

---

_Comment by @charliermarsh on 2023-08-22 03:23_

On `main`, we get the `with` right, in my testing...

---

_Comment by @dhruvmanila on 2023-08-22 03:29_

Yeah, might be 181131272. So, the `MatchCase` needs to be updated accordingly.

---

_Renamed from "Formatter: inconsistency for parenthesized `with`/`while` with trailing comment" to "Formatter: inconsistency for parenthesized `case` pattern with trailing comment" by @dhruvmanila on 2023-08-22 03:52_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:15_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 14:36_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-27 14:36_

---

_Comment by @charliermarsh on 2023-09-27 14:37_

On main, it's now the case that these all preserve parentheses except the `with`.

We should make it consistent, but it's low priority and not required for the Beta.

---

_Renamed from "Formatter: inconsistency for parenthesized `case` pattern with trailing comment" to "Formatter: inconsistency for parenthesized `with` with trailing comment" by @charliermarsh on 2023-09-27 14:37_

---

_Label `accepted` added by @charliermarsh on 2023-09-27 14:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-28 00:08_

---

_Closed by @charliermarsh on 2023-09-28 00:16_

---

_Comment by @MichaReiser on 2023-09-28 06:37_

@charliermarsh Do you mind if we keep this issue open to ensure we fix it eventually (It's already part of the stable milestone) or did your PR fix all cases?

---

_Reopened by @MichaReiser on 2023-09-28 06:37_

---

_Closed by @MichaReiser on 2023-09-28 06:40_

---

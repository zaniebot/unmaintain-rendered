```yaml
number: 759
title: Polars concat fails to typecheck with ty
type: issue
state: closed
author: kszlim
labels:
  - generics
assignees: []
created_at: 2025-07-02T21:21:52Z
updated_at: 2025-10-06T17:03:07Z
url: https://github.com/astral-sh/ty/issues/759
synced_at: 2026-01-12T15:54:23Z
```

# Polars concat fails to typecheck with ty

---

_@kszlim_

### Summary

Using polars 1.3.1 and ty fails to typecheck the following block with:
```
error[unresolved-attribute]: Type `DataFrame` has no attribute `collect`
 --> r.py:5:7
  |
3 | ldf = pl.LazyFrame({"a": [1,2,3]})
4 | z = pl.concat([ldf, ldf])
5 | print(z.collect())
  |       ^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default
```

MRE:
```python
import polars as pl

ldf = pl.LazyFrame({"a": [1,2,3]})
z = pl.concat([ldf, ldf])
print(z.collect())
```

This could be an issue in polars code for this function is [here](https://github.com/pola-rs/polars/blob/bfcb40e629e4e3a3d2ee4ab8f0653886fa4a7dd8/py-polars/polars/functions/eager.py#L26), but that method does seem to typecheck with pyright. Not sure, but maybe it's related to https://github.com/astral-sh/ty/issues/688.

### Version

0.0.1-alpha.13

---

_Label `generics` added by @carljm on 2025-07-02 21:53_

---

_Comment by @carljm on 2025-07-02 21:54_

This is because we currently still infer `list[Unknown]` for the list literal `[ldf, ldf]`. There might also be an issue with how we then resolve the type var in the `concat` call, given an argument of type `list[Unknown]` -- it seems we resolve it to `DataFrame` (I'm not sure why), but we should just consider the return type `Unknown`.

---

_Comment by @AlexWaygood on 2025-10-06 16:22_

We type-check this correctly now that https://github.com/astral-sh/ruff/pull/20360 has landed

---

_Closed by @AlexWaygood on 2025-10-06 16:22_

---

_Comment by @kszlim on 2025-10-06 17:01_

@AlexWaygood is this in the latest pre-release?

---

_Comment by @AlexWaygood on 2025-10-06 17:03_

> [@AlexWaygood](https://github.com/AlexWaygood) is this in the latest pre-release?

yup!

---

```yaml
number: 1789
title: Types and arguments are not being detected on polars DataFrames
type: issue
state: closed
author: theelderbeever
labels: []
assignees: []
created_at: 2025-12-05T23:01:39Z
updated_at: 2025-12-08T20:20:01Z
url: https://github.com/astral-sh/ty/issues/1789
synced_at: 2026-01-12T15:54:25Z
```

# Types and arguments are not being detected on polars DataFrames

---

_@theelderbeever_

### Summary

`basedpyright` correctly recognizes the type following the `join` as well as the incorrect argument being passed to the call.

<img width="460" height="298" alt="Image" src="https://github.com/user-attachments/assets/164fdb02-d0f3-4313-b007-fdb1ac176aeb" />

`ty`  loses the typing and doesn't alert on the incorrect keyword arg.
<img width="505" height="296" alt="Image" src="https://github.com/user-attachments/assets/f13f1815-cae3-49e4-b896-e8a3a16854dd" />

### Version

0.0.1-alpha.32

---

_Comment by @carljm on 2025-12-05 23:15_

Hi, thanks for the report! This is https://github.com/astral-sh/ty/issues/1136. (`polars.DataFrame.join` is decorated with a `deprecate_renamed_parameter` decorator that currently causes us to lose its typing.)

Will close this as a duplicate, but we'll make sure this case also works when that issue is fixed.

(For future reference, bug reports with text code samples instead of screenshots are easier to reproduce and deal with! But I realize the screenshot gave you the inlay types, which was handy.)

---

_Closed by @carljm on 2025-12-05 23:15_

---

_Comment by @theelderbeever on 2025-12-08 20:20_

@carljm Apologies for not adding the code snippet directly but, yes the intent was to convey what the inlay hints showed.

```python
import polars as pl

df1 = pl.DataFrame(
    {
        "a": [1, 2, 3, None],
        "b": ["a", "b", "c", "z"],
    }
)
df2 = pl.DataFrame(
    {
        "a": [1, 2, 3, None],
        "c": ["albatross", "bear", "cougar", "zebra"],
    }
)

df3 = df1.join(df2, on="a", nulls_equal=True)
df4 = df1.join(df2, on="a", join_nulls=True)
```

---

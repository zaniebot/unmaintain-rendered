```yaml
number: 9279
title: "[Panic] VS Code extension Jupyter notebook UTF8 in doc string"
type: issue
state: closed
author: rkrueck
labels:
  - bug
assignees: []
created_at: 2023-12-25T21:32:22Z
updated_at: 2023-12-26T22:09:07Z
url: https://github.com/astral-sh/ruff/issues/9279
synced_at: 2026-01-12T15:54:49Z
```

# [Panic] VS Code extension Jupyter notebook UTF8 in doc string

---

_@rkrueck_

In Jupyter notebooks with Ruff extension, when using UTF8 lines in doc string (""" ... """) I get:
Ruff: Lint failed (error: Ruff crashed ...
thread 'main' panicked at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1\library\core\src\str\mod.rs:660:13:
byte index 2 is not a char boundary; it is inside '╭' (bytes 0..3) of `╭─────────┬─────────┬─────────┬─────────┬─────────╮`
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
)

This type of table is returned for example from polars dataframe with `print(df)`
Then I copy the output in the doc string to remind me what output to expect

Removing `docstring-code-format = true` from pyproject.toml  does not help
When using the same UTF8 in comment (#) it works.
Workaround: For now I can select `pl.Config.set_ascii_tables(True) `with `print(df)`

Sample code: 
```
import polars as pl


def create_df() -> pl.DataFrame:
    """sample

    Returns:
        pl.DataFrame:
    shape: (3, 5)
    ╭─────────┬─────────┬─────────┬─────────┬─────────╮
    │ A (i64) ┆ B (i64) ┆ C (i64) ┆ D (i64) ┆ E (i64) │
    ╞═════════╪═════════╪═════════╪═════════╪═════════╡
    │ 20      ┆ 43      ┆ 31      ┆ 31      ┆ 42      │
    │ 36      ┆ 23      ┆ 0       ┆ 39      ┆ 46      │
    │ 5       ┆ 14      ┆ 5       ┆ 25      ┆ 13      │
    ╰─────────┴─────────┴─────────┴─────────┴─────────╯
    """
    return pl.DataFrame(
        [
            {'A': 20, 'B': 43, 'C': 31, 'D': 31, 'E': 42},
            {'A': 36, 'B': 23, 'C': 0, 'D': 39, 'E': 46},
            {'A': 5, 'B': 14, 'C': 5, 'D': 25, 'E': 13},
        ]
    )
```

pyproject.toml settings:
[tool.ruff.format]
quote-style = "single"
docstring-code-format = true

v2023.58.0 from VS Code

---

_Comment by @MichaReiser on 2023-12-26 03:53_

I think this should be fixed in the latest version of Ruff. What version of ruff (not the VS code extension) are you using in your project?

---

_Comment by @rkrueck on 2023-12-26 14:09_

I checked. There is no Ruff installed. Only the VS Code extension

---

_Comment by @charliermarsh on 2023-12-26 22:07_

I was able to reproduce by copying the above into a Jupyter notebook, and running on Ruff v0.1.8. But it works without error on `main`, as expected. We'll cut a new VS Code release soon which will increment the bundled version of Ruff (the latest extension uses v0.1.8, not v0.1.9), or in the meantime you can install Ruff outside of the extension and it should pick up the latest version from your environment.

---

_Closed by @charliermarsh on 2023-12-26 22:07_

---

_Label `bug` added by @charliermarsh on 2023-12-26 22:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-26 22:09_

---

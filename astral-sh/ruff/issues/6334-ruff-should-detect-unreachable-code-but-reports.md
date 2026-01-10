---
number: 6334
title: "Ruff should detect unreachable code but reports 'Missing explicit return', fixing it with `return None`"
type: issue
state: open
author: bittner
labels:
  - bug
assignees: []
created_at: 2023-08-04T09:14:16Z
updated_at: 2023-08-04T13:51:40Z
url: https://github.com/astral-sh/ruff/issues/6334
synced_at: 2026-01-10T01:22:45Z
---

# Ruff should detect unreachable code but reports 'Missing explicit return', fixing it with `return None`

---

_Issue opened by @bittner on 2023-08-04 09:14_

In the code below Ruff should detect unreachable code, but it reports 'Missing explicit return'.

It tries to `--fix` the problem appending a `return None` line even after the unreachable code, which clearly makes no sense.

## Example

```python
# FILE: foo.py

def get_rates(data, dataframe_foo):
    """A data science example with data frames."""
    dt_tmp = data.copy()
    rates = (
        dt_tmp.sort_values(dataframe_foo.date_col)
        .groupby([dataframe_foo.id_col, dataframe_foo.bar_col])
        .agg(list)
    )
    rates = rates.reset_index().rename(columns={0: "baz"})
    foobar_rates = rates[["dki_id", "bar", "baz"]]
    return foobar_rates
    rates = rates.reset_index()
```

After running `ruff foo.py --fix` you get:

```diff
    return foobar_rates
    rates = rates.reset_index()
+   return None
```

Interestingly, with `ruff foo.py --fix --isolated` nothing gets changed.

## Ruff details

```console
$ ruff --version
ruff 0.0.282
```

Configuration in `pyproject.toml`:

```toml
[tool.ruff]
extend-exclude = []
extend-select = ["B", "BLE", "C4", "C90", "COM", "DJ", "DTZ", "EM", "G", "I", "N", "PIE", "PL", "PT", "PTH", "R", "RUF", "S", "SIM", "T10", "TID", "W", "YTT"]
extend-ignore = []

[tool.ruff.per-file-ignores]
"tests/**/*.py" = ["S101"]
```

## Maybe related

I found PR #5384, which seems to address the very issue. Interestingly, the configuration rule `RUF014` that the PR supposedly implements is not mentioned in any of the [recent releases](https://github.com/astral-sh/ruff/releases). (I tried to install Ruff from source code to verify the claim of the merged feature not being included, but my system ships a too-old version of `rustc`, which makes the build fail.)

---

_Comment by @zanieb on 2023-08-04 13:51_

#5384 is [behind a feature gate](https://github.com/astral-sh/ruff/blob/a459d8ffc7118d0db338491f3ed88f2aec48fa9e/crates/ruff/src/rule_selector.rs#L252-L253) and is not available in releases yet.

It seems like we may be able to address this false positive separately.

---

_Label `bug` added by @zanieb on 2023-08-04 13:51_

---

_Referenced in [ASFHyP3/actions#261](../../ASFHyP3/actions/issues/261.md) on 2025-02-11 17:48_

---

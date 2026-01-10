```yaml
number: 6432
title: PD rules trigger on non-Pandas DataFrames
type: issue
state: closed
author: beskep
labels:
  - bug
  - good first issue
  - type-inference
  - help wanted
assignees: []
created_at: 2023-08-09T00:06:09Z
updated_at: 2025-09-05T00:21:36Z
url: https://github.com/astral-sh/ruff/issues/6432
synced_at: 2026-01-10T11:09:48Z
```

# PD rules trigger on non-Pandas DataFrames

---

_Issue opened by @beskep on 2023-08-09 00:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

command: `ruff check test.py`
ruff version: `ruff 0.0.282`
settings: `select = ['ALL']`

example:
```python
import polars as pl

pldf = pl.DataFrame()
pldf.pivot()  # PD010 `.pivot_table` is preferred to `.pivot` or `.unstack`; provides same functionality
```

polars DataFrame provides [`.pivot()` function](https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.pivot.html) but no `.pivot_table()` unlike pandas.


---

_Label `bug` added by @charliermarsh on 2023-08-09 02:27_

---

_Label `type-inference` added by @charliermarsh on 2023-08-09 02:27_

---

_Comment by @charliermarsh on 2023-08-09 02:29_

Difficult for us to fully resolve this without a full type inference engine (we could use heuristics, like avoid flagging these rules if `polars` is imported, but that comes with other problems: you don't have to import Polars in order to access a Polars DataFrame, and just because you import Polars doesn't mean you aren't working with Pandas DataFrames anywhere). Likely won't be fixed in the near-term.

(I'd recommend against using these rules if you're working with Polars.)


---

_Comment by @MarcoGorelli on 2023-08-23 13:57_

for a simpler heuristic, would it be possible to check the alias used to instantiate the dataframe? `pl.DataFrame` rather than `pd.DataFrame` gives a pretty strong clue that it's not pandas

---

_Comment by @kleinicke on 2024-04-17 12:19_

Currently the pandas rules are applied on many non pandas objects. For example PD011 tries to stop you from using .values anywhere, even if you use a library where you should use it.
Therefore some kind of check, if the object is even belonging to pandas would be pretty useful.

---

_Renamed from "PD010 false positive on polars.DataFrame" to "PD rules trigger on non-Pandas DataFrames" by @charliermarsh on 2024-05-02 14:31_

---

_Comment by @bje- on 2024-07-01 02:51_

The same thing happens with the Python `DEAP` package which has class members named `values`.

---

_Comment by @ItsDrike on 2024-07-21 13:22_

Ruff is actually really trigger happy here, just posting another quick example that causes ruff to trigger while just messing around with python builtins:

```python
# ruff: noqa: F841
# pyright: reportUnusedVariable=false

x = {}
values_dict_func = x.values  # PD011
```

---

_Comment by @bje- on 2024-07-22 17:56_

> Difficult for us to fully resolve this without a full type inference engine (we could use heuristics, like avoid flagging these rules if `polars` is imported, but that comes with other problems: you don't have to import Polars in order to access a Polars DataFrame, and just because you import Polars doesn't mean you aren't working with Pandas DataFrames anywhere). Likely won't be fixed in the near-term.

I think the false positive rate on this warning is so high it should be abandoned. Could Pandas be modified to emit a deprecation warning instead?

---

_Comment by @charliermarsh on 2024-07-22 17:58_

Why not just turn it off in your project? By definition you've opted into it.

---

_Comment by @bje- on 2024-07-22 18:07_

A good lint tool should be one that doesn't require littering your source files with pragmas to disable false positives. Isn't one of the purposes of a linter to improve code readability?

(I just used a noqa pragma to disable NPY002, but in this case, ruff is correct, but I can't change it).

---

_Comment by @ncooder on 2024-11-18 20:39_

This problem persists with pyspark. It tries to replace pivot with pivot_table.

---

_Comment by @SerTetora on 2025-01-28 14:14_

I think, i have same issue, but with `pytest.param`

```python
import pytest


def foo(bar: list[pytest.param]) -> list[str]:
    ids = []
    for e in bar:
        a = e.values[0]["a"]  # noqa: PD011 # RUF100: Unused `noqa` directive (unused: `PD011`)
        ids.append(a)
    return ids
``` 


Playground:
https://play.ruff.rs/8b6c0675-5d75-4eb5-a7ed-df33b194e7fa

---

_Comment by @dhruvmanila on 2025-01-29 10:50_

@SerTetora I'm confused, it seems like it's working as expected i.e., you should remove the `noqa: PD011` because Ruff won't highlight `e.values` for `PD011`. We updated the rules related to Pandas to only trigger when there's a `pandas` import in the module (https://github.com/astral-sh/ruff/pull/14671).

---

_Comment by @SerTetora on 2025-01-29 11:14_

I can't reproduce it anymore. Maybe something wrong was on my local. Sorry for a noise.

---

_Comment by @MeGaGiGaGon on 2025-06-19 02:27_

I just double-checked all the `PD` rules, and currently the only one that still triggers without a `pandas` import is `PD002` [playground](https://play.ruff.rs/8ac74de3-e23d-47f3-9187-7473bc3b705d) so it probably got missed in #14671.

---

_Comment by @dhruvmanila on 2025-06-19 05:06_

Thanks for reporting!

I think we should replace the following code:

https://github.com/astral-sh/ruff/blob/e352a50b74329268589cdc18eafab123832559ac/crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs#L55-L62

with the following which is more robust:

```rs
    if !checker.semantic().seen_module(Modules::PANDAS) {
        return;
    }
```

---

_Label `help wanted` added by @MichaReiser on 2025-06-19 09:15_

---

_Label `good first issue` added by @MichaReiser on 2025-06-19 09:15_

---

_Comment by @jordyjwilliams on 2025-06-26 15:14_

> Thanks for reporting!
> 
> I think we should replace the following code:

@dhruvmanila @MeGaGiGaGon thanks for doing all the hard work here xD
 
PR in: https://github.com/astral-sh/ruff/pull/18963

---

_Comment by @GDYendell on 2025-08-02 16:04_

This should have been closed when #18963 was merged

---

_Closed by @MichaReiser on 2025-08-02 19:49_

---

_Comment by @nataziel on 2025-09-05 00:21_

This behaviour still occurs on non-Pandas objects, such as `xarray.DataArray`, if pandas is also imported. 

<img width="756" height="187" alt="Image" src="https://github.com/user-attachments/assets/60c65e1c-c2c2-4140-9dec-1ed5528248e7" />

Seems like type aware linting is needed for these types of rules. I believe you're already working towards using `ty` to do the type inference for when to apply the lint?

---

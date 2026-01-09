---
number: 13977
title: "IPython mode: load cell magics from configuration"
type: issue
state: open
author: minrk
labels:
  - configuration
  - notebook
assignees: []
created_at: 2024-10-29T10:09:39Z
updated_at: 2024-10-30T11:24:09Z
url: https://github.com/astral-sh/ruff/issues/13977
synced_at: 2026-01-07T13:12:16-06:00
---

# IPython mode: load cell magics from configuration

---

_Issue opened by @minrk on 2024-10-29 10:09_

- Searched for: cell magics, Jupyter, IPython, configuration, pyproject.toml
- ruff version: 0.7.1


In handling IPython magics, the list of magics that contain Python code to be checked/formatted is currently [hard-coded](https://github.com/astral-sh/ruff/blob/56c796acee6feaa3bd27b43c7ccca33b08f25d4c/crates/ruff_notebook/src/cell.rs#L243-L254).

Since loads of extensions (especially profiling, etc.) and even users can define these magics interactively, it seems like the list of cell magics that should be treated as containing Python code should be loaded from configuration, so users can lint their notebooks without a patch to ruff itself.

nbqa [has this feature](https://nbqa.readthedocs.io/en/latest/configuration.html#cell-magics) and can be used to run ruff on the contents of _arbitrary_ cell magics, with the same default of assuming unrecognized cell magics don't contain Python code. A similar feature in ruff itself would be great!

[Example notebook](https://gist.github.com/minrk/1db663c2b4ae42e731a626865fd91ccf) with the cells:

```python
from IPython import get_ipython


def decorate_cell(line, cell):
    print("before")
    get_ipython().run_cell(cell)
    print("after")

get_ipython().register_magic_function(decorate_cell, "cell")
```

```python
%%decorate_cell
x = 5
```

```python
print(x) # <- F821 false positive
```


---

_Referenced in [ipython/ipyparallel#897](../../ipython/ipyparallel/issues/897.md) on 2024-10-29 10:15_

---

_Label `configuration` added by @dhruvmanila on 2024-10-29 10:19_

---

_Label `notebook` added by @dhruvmanila on 2024-10-29 10:19_

---

_Comment by @dhruvmanila on 2024-10-29 10:32_

This seems like a reasonable request to me.

Unlike `nbqa`, Ruff is able to differentiate between line magics and cell magics. Ruff is able to parse out the line magics and continue parsing any other lines surrounding these line magics. The hard coded values are specific to cell magics and Ruff ignores them because they're applied on the entire cell. I'm sure you're aware of this but just re-iterating for context.

I'd prefer to have an alternate option name like `allowed_cell_magics` / `cell_magics` along with the `extend_*` version to make it clear that this is related to the "cell magics". This option should be at the top level `[tool.ruff]` because both the linter and the formatter will consider them as is done so today. The non-extend option would have the default value that is hard coded today.

---

_Comment by @minrk on 2024-10-30 11:05_

I'm also not sure if ruff handles it, but cell magics can be arbitrarily nested because the code-running cell magics (many of them anyway) call run_cell, which in turn may call cell magics, etc.. This is a valid cell containing Python code that should be recognized as a candidate for Python formatting assuming `prun`, `time`, and `timeit` are understood to be cell magic functions that accept code:

```
%%prun
%%time
%%timeit
for i in range(5):
    _env = %env
```

whereas this one should not format the contents, since `sql` is not a Python-code cell magic:

```
%%prun
%%sql
SELECT * FROM TABLE
```

---

_Comment by @dhruvmanila on 2024-10-30 11:23_

I think we _should_ handle it correctly because we check if _any_ of the lines contains a cell magic which might not have valid Python code:

https://github.com/astral-sh/ruff/blob/bd0d782b76d75c02668741c64303f6164910f8f2/crates/ruff_notebook/src/cell.rs#L206-L207

So, the two cells that you've provided works fine.

---

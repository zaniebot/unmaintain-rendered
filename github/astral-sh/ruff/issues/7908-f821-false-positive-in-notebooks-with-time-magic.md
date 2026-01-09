---
number: 7908
title: F821 false positive in notebooks with %%time magic
type: issue
state: closed
author: david-waterworth
labels: []
assignees: []
created_at: 2023-10-11T01:34:34Z
updated_at: 2023-10-13T00:39:10Z
url: https://github.com/astral-sh/ruff/issues/7908
synced_at: 2026-01-07T13:12:15-06:00
---

# F821 false positive in notebooks with %%time magic

---

_Issue opened by @david-waterworth on 2023-10-11 01:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

If I use the time magic (%%time) in a notebook cell

```
%%time
x=1
```

then any subsequent cell that makes use of `x` will report F821 Undefined name `x`




---

_Comment by @charliermarsh on 2023-10-11 03:31_

\cc @dhruvmanila - mind taking a look?

---

_Label `bug` added by @charliermarsh on 2023-10-11 03:31_

---

_Comment by @dhruvmanila on 2023-10-11 04:20_

This is happening because `%%time` is a cell magic which applies to the entire cell. We ignore all cells containing those and that's why `x` is undefined in subsequent cells. This is because: 
* Everything inside that cell is consumed by IPython and usually it's executed in such a way that anything inside isn't leaked outside that cell
* The cell could contain basically any syntax, even non-Python using `%%bash`, `%%html`, etc. which means we have to consume the entire content as string and so we can't really do analysis

FWIW, Pyright raises the same error. Another interesting thing is that `%%time` isn't mentioned in their documentation but when I look at the help text (using `%%time?`) they've suggested to use `%%timeit` instead. Using `%%timeit` the variable isn't leaked outside the cell:

```
%%timeit x = 1
x * x
```

---

_Comment by @dhruvmanila on 2023-10-11 04:22_

I wouldn't classify this as a bug as it's expected behavior. It's interesting that `%%time` modifies the global scope but not the local one:

```
def foo():
	%time x = 1
	print(x)

foo()
```

Running the above will raise a `NameError: name 'x' is not defined`

---

_Label `bug` removed by @charliermarsh on 2023-10-11 04:23_

---

_Closed by @dhruvmanila on 2023-10-13 00:39_

---

_Referenced in [astral-sh/ruff#8304](../../astral-sh/ruff/issues/8304.md) on 2023-11-01 06:40_

---

_Referenced in [astral-sh/ruff#13374](../../astral-sh/ruff/issues/13374.md) on 2024-09-17 08:25_

---

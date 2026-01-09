---
number: 7418
title: False positive for PERF401 when using assignment expression
type: issue
state: closed
author: mounirmesselmeni
labels: []
assignees: []
created_at: 2023-09-15T20:11:39Z
updated_at: 2024-10-03T23:06:11Z
url: https://github.com/astral-sh/ruff/issues/7418
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive for PERF401 when using assignment expression

---

_Issue opened by @mounirmesselmeni on 2023-09-15 20:11_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello,

the PERF401 is giving false positive when using [Python's assignment expression](https://peps.python.org/pep-0572/)

Python version: 3.11.5
ruff: 0.0.287

Example code:

```python
results = []
for x in [0, 1, 2, 3, 4]:
    if result := my_func(x):
        results.append(result)
```

Screenshot from VSCode
<img width="708" alt="image" src="https://github.com/astral-sh/ruff/assets/1055731/ed6acf0f-a384-4a95-9f73-3c6391cb2bb4">

PS: Thanks for this great library and amazing work you're doing!



---

_Comment by @charliermarsh on 2023-09-15 20:19_

Thanks for the kind words :)

I think this snippet _can_ be rewritten as:

```python
results = [
    result for x in [0, 1, 2, 3, 4] if (result := my_func(x))
]
```

Though it's a bit less clear than the originating code, so I'm unsure whether to recommend it or not.


---

_Comment by @mounirmesselmeni on 2023-09-15 20:27_

Nice one!

Yeah, you can see with the community tbh if Ruff should ignore those use case or not.

For myself now, I can live with this 😄  Thanks again @charliermarsh  

---

_Closed by @mounirmesselmeni on 2023-09-15 20:27_

---

_Comment by @jamesbraza on 2024-10-03 23:06_

For posterity/linking, this is a dupe of https://github.com/astral-sh/ruff/issues/6056

---

---
number: 7398
title: uv uses old wheels in cache when building bindings locally
type: issue
state: closed
author: pacjac
labels: []
assignees: []
created_at: 2024-09-14T20:10:11Z
updated_at: 2024-09-14T20:16:46Z
url: https://github.com/astral-sh/uv/issues/7398
synced_at: 2026-01-10T01:24:14Z
---

# uv uses old wheels in cache when building bindings locally

---

_Issue opened by @pacjac on 2024-09-14 20:10_

For a project I'm using pybind11 to create python bindings. 

Running uv 0.3.2. on x86_64 GNU/Linux.

* A minimal code snippet that reproduces the bug.
1. Set up an example project:
`uv tool run cookiecutter gh:scientific-python/cookie`, select 9 "scikit-build".
2. Install first version
`uv add pip & uv run pip install .`
3. Modify source code
`sed -i -e "s/add/plus/g" src/main.cpp"
4. in a second terminal, open a python session `uv run python`
5. Install upgraded wheel
`uv run pip install . --upgrade`
Here, everything works as expected, the changes take effect.
In the running python session
```python
import mymodule._core as mwe
print(dir(mwe))
```
shows the renamed plus function.
5. Run another uv utility, e.g. `uv run python`
The old version of our local wheel is installed again


---

_Comment by @pacjac on 2024-09-14 20:16_

The great [documentation on caching](https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata) fully solves my problem, I was merely too stupid to RTFM.



---

_Closed by @pacjac on 2024-09-14 20:16_

---

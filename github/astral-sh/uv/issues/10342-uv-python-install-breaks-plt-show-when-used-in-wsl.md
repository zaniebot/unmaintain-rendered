---
number: 10342
title: Uv Python install breaks plt.show() when used in wsl
type: issue
state: closed
author: Matt-Ord
labels:
  - compatibility
assignees: []
created_at: 2025-01-07T09:14:31Z
updated_at: 2025-08-08T22:11:10Z
url: https://github.com/astral-sh/uv/issues/10342
synced_at: 2026-01-07T13:12:18-06:00
---

# Uv Python install breaks plt.show() when used in wsl

---

_Issue opened by @Matt-Ord on 2025-01-07 09:14_


Working in a dev container, I had previously relied on the python feature to install python
```
{
	"name": "Debian",
	"image": "mcr.microsoft.com/devcontainers/base:bullseye",
	"features": {
                "ghcr.io/devcontainers/features/python:1": {
			"version": "3.13"
		},
		"ghcr.io/va-h/devcontainers-features/uv:1": {
			"shellautocompletion": true,
			"version": "latest"
		},
		"ghcr.io/devcontainers/features/rust:1": {
			"version": "latest",
			"profile": "minimal"
		}
	},
	"onCreateCommand": "uv sync --all-extras"
}
```
however I swapped to using uv pyhton install

```
{
	"name": "Debian",
	"image": "mcr.microsoft.com/devcontainers/base:bullseye",
	"features": {
		"ghcr.io/va-h/devcontainers-features/uv:1": {
			"shellautocompletion": true,
			"version": "latest"
		},
		"ghcr.io/devcontainers/features/rust:1": {
			"version": "latest",
			"profile": "minimal"
		}
	},
	"onCreateCommand": "uv python install && uv sync --all-extras"
}
```

with this configuration I get this error when attempting fig.show() on a matplotlib figure

```
from __future__ import annotations

from matplotlib import pyplot as plt

plt.plot([1, 2, 3, 4])
plt.show()
input()
```

```
UserWarning: FigureCanvasAgg is non-interactive, and thus cannot be shown
```

How do I get uv to install a compatible python environment in this case?

---

_Comment by @konstin on 2025-01-07 10:12_

I think this is another case of #6893

---

_Label `compatibility` added by @charliermarsh on 2025-01-07 13:49_

---

_Comment by @tacaswell on 2025-01-10 17:18_

I agree this is a duplicate.  To be sure add `plt.switch_backend('tkagg')` before calling `plt.plot()` and if it is a duplicate you will get an exception raised.

---

_Comment by @ncoghlan on 2025-06-09 02:20_

I ran into this while trying to use a couple of the `sklearn` demos as a demo for `venvstacks` based deployment. Initial symptom was the warning reported here, while adding `plt.switch_backend('tkagg')` changed the outcome to (confirming the "duplicate of #6893" assessment):

```
AttributeError: module '_tkinter' has no attribute '__file__'. Did you mean: '__name__'?

The above exception was the direct cause of the following exception:

ImportError: failed to load tkinter functions

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/acoghlan/devel/venvstacks/examples/sklearn/_build/app-clustering-demo/lib/python3.11/site-packages/sklearn_clustering.py", line 139, in <module>
    plt.switch_backend('tkagg')
  File "/home/acoghlan/devel/venvstacks/examples/sklearn/_build/cpython-3.11/lib/python3.11/site-packages/matplotlib/pyplot.py", line 425, in switch_backend
    module = backend_registry.load_backend_module(newbackend)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/acoghlan/devel/venvstacks/examples/sklearn/_build/cpython-3.11/lib/python3.11/site-packages/matplotlib/backends/registry.py", line 317, in load_backend_module
    return importlib.import_module(module_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/acoghlan/devel/venvstacks/examples/sklearn/_build/cpython-3.11/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<frozen importlib._bootstrap>", line 1204, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1176, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1147, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 690, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 940, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/home/acoghlan/devel/venvstacks/examples/sklearn/_build/cpython-3.11/lib/python3.11/site-packages/matplotlib/backends/backend_tkagg.py", line 1, in <module>
    from . import _backend_tk
  File "/home/acoghlan/devel/venvstacks/examples/sklearn/_build/cpython-3.11/lib/python3.11/site-packages/matplotlib/backends/_backend_tk.py", line 25, in <module>
    from . import _tkagg
ImportError: initialization failed
```


---

_Comment by @ncoghlan on 2025-06-09 02:31_

Further confirmation: adding `pyside6` to the stack demo (and taking the explicit use of the TkAgg backend out again) gave a passing result.

---

_Comment by @konstin on 2025-06-10 10:43_

I'll close this issue in favor of #6893.

---

_Closed by @konstin on 2025-06-10 10:43_

---

_Comment by @geofft on 2025-08-08 22:11_

Copying my note from #6893 - I believe this should be resolved in uv 0.8.7, released just now. Please make sure you've updated both uv and your local Python versions:

```
uv self update
uv python install --reinstall --no-bin 3.13.6
```

If you're using another Python version, use 3.9.23, 3.10.18, 3.11.13, 3.12.11, and/or 3.14.0rc1 in that command. (This ensures that you're on the latest patch release and you pick up the new build in case you were on an older build of that same patch release.)

Please try it out and let me know if there are remaining issues.

---

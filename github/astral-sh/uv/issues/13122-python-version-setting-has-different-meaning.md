---
number: 13122
title: "`python-version` setting has different meaning between `uv pip compile` and `uv pip install`"
type: issue
state: open
author: watsonjj
labels:
  - enhancement
assignees: []
created_at: 2025-04-26T15:51:14Z
updated_at: 2025-04-27T15:21:16Z
url: https://github.com/astral-sh/uv/issues/13122
synced_at: 2026-01-07T13:12:18-06:00
---

# `python-version` setting has different meaning between `uv pip compile` and `uv pip install`

---

_Issue opened by @watsonjj on 2025-04-26 15:51_

### Summary

My aim was to have a consistent `uv pip compile` output between the pre-commit running locally and on the CI, even if different Python versions may be present between two. The minimum Python version my package supports is 3.9. I decided to use the settings in the pyproject.toml for this, as it seemed the right thing to do:

```toml
[tool.uv.pip]
python-version = "3.9"
universal = true
```

And this achieved what I wanted.

However, I later struggled with a strange problem when running numpy:
<details>
  <summary>error message</summary>

```bash
>>>  import numpy
Traceback (most recent call last):
  File "/usr/local/Caskroom/miniforge/base/envs/ultrasat/lib/python3.10/site-packages/numpy/core/__init__.py", line 24, in <module>
    from . import multiarray
  File "/usr/local/Caskroom/miniforge/base/envs/ultrasat/lib/python3.10/site-packages/numpy/core/multiarray.py", line 10, in <module>
    from . import overrides
  File "/usr/local/Caskroom/miniforge/base/envs/ultrasat/lib/python3.10/site-packages/numpy/core/overrides.py", line 8, in <module>
    from numpy.core._multiarray_umath import (
ModuleNotFoundError: No module named 'numpy.core._multiarray_umath'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/local/Caskroom/miniforge/base/envs/ultrasat/lib/python3.10/site-packages/numpy/__init__.py", line 130, in <module>
    from numpy.__config__ import show as show_config
  File "/usr/local/Caskroom/miniforge/base/envs/ultrasat/lib/python3.10/site-packages/numpy/__config__.py", line 4, in <module>
    from numpy.core._multiarray_umath import (
  File "/usr/local/Caskroom/miniforge/base/envs/ultrasat/lib/python3.10/site-packages/numpy/core/__init__.py", line 50, in <module>
    raise ImportError(msg)
ImportError:

IMPORTANT: PLEASE READ THIS FOR ADVICE ON HOW TO SOLVE THIS ISSUE!

Importing the numpy C-extensions failed. This error can happen for
many reasons, often due to issues with your setup or how NumPy was
installed.

We have compiled some common reasons and troubleshooting tips at:

    https://numpy.org/devdocs/user/troubleshooting-importerror.html

Please note and check the following:

  * The Python version is: Python3.10 from "/usr/local/Caskroom/miniforge/base/envs/ultrasat/bin/python"
  * The NumPy version is: "1.26.4"

and make sure that they are the versions you expect.
Please carefully study the documentation linked above for further help.

Original error was: No module named 'numpy.core._multiarray_umath'


The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/Caskroom/miniforge/base/envs/ultrasat/lib/python3.10/site-packages/numpy/__init__.py", line 135, in <module>
    raise ImportError(msg) from e
ImportError: Error importing numpy: you should not try to import numpy from
        its source directory; please exit the numpy source tree, and relaunch
        your python interpreter from there.
```
</details>

I have nailed down the issue being a result of:
```bash
uv pip install numpy==1.26.4 --python-version 3.9
```

Which was effectively occurring due to the above configuration inside the pyproject.toml of the directory I was in. My python version is 3.10.14, and therefore this incompatibility occurred.

I would not consider this a bug, but it was quite surprising behaviour - the purpose of the `python-version` setting is quite different between `uv pip compile` and `uv pip install`/`uv pip sync`. I think this is somewhat reflected in the Commands documentation page, as the `--python-version` description differs slightly between the commands. 

It would be great if there was a different setting in the pyproject.toml for the minimum Python version considered for `uv pip compile`. Failing that, then at least a warning could be included on the `python-version` setting documentation.

### Example

_No response_

---

_Label `enhancement` added by @watsonjj on 2025-04-26 15:51_

---

_Comment by @charliermarsh on 2025-04-27 15:07_

Yeah this has come up before (though I don't have the issue on-hand). I think it's a footgun, we should fix it. (I suggest passing `--python-version` explicitly to the `uv pip compile` call.)

---

_Comment by @charliermarsh on 2025-04-27 15:21_

One option is to put these under `[tool.uv.pip.compile]`.

---

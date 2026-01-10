---
number: 10464
title: "error: Distribution `kaleido==0.2.1.post1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform"
type: issue
state: open
author: jamesdeluk
labels: []
assignees: []
created_at: 2025-01-10T10:14:48Z
updated_at: 2025-02-20T11:19:47Z
url: https://github.com/astral-sh/uv/issues/10464
synced_at: 2026-01-10T01:24:54Z
---

# error: Distribution `kaleido==0.2.1.post1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

---

_Issue opened by @jamesdeluk on 2025-01-10 10:14_

New to uv, sorry if PEBKAC.

I'm trying to install pycaret (`uv add pycaret`) to a new project on Windows, Python 3.11.

Get this error:

```
error: Distribution `kaleido==0.2.1.post1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

The repo (https://pypi.org/simple/kaleido/) has:
```
kaleido-0.2.1-py2.py3-none-win_amd64.whl
kaleido-0.2.1.post1-py2.py3-none-manylinux2014_armv7l.whl
```
So, no `kaleido-0.2.1.post1` for Windows.

Checking the PyPi website, there is https://pypi.org/project/kaleido/0.2.1.post1/

```
pip install kaleido==0.2.1.post1
```

Although that applies to the manylinux version.

Yet using `conda`, it works fine; it installs:

```
Downloading kaleido-0.2.1-py2.py3-none-win_amd64.whl (65.9 MB)
```

Is there a way for uv to use the compatible version like conda does?

---

_Comment by @konstin on 2025-01-10 10:45_

The problem is that kaleido 0.2.1 and 0.2.1.post1 are technically different versions, and the latter has only a `kaleido-0.2.1.post1-py2.py3-none-manylinux2014_armv7l.whl` (and due to universal resolution, uv doesn't know which wheels it will need eventually). Setting required platforms is planned in #10067, currently you can try something like

```
[tool.uv]
environments = [
	"platform_system != "Windows",
	"platform_system == "Windows"
]
```

or banning `0.2.1.post1` in a constraint.

---

_Comment by @jamesdeluk on 2025-01-10 17:41_

Thanks @konstin .

> ```
> [tool.uv]
> environments = [
> 	"platform_system != "Windows",
> 	"platform_system == "Windows"
> ]
> ```

The same error occurred with this (note it's `"platform_system != 'Windows'"` not `"platform_system != "Windows"` for anyone else who tried this and initially had an error).

> or banning `0.2.1.post1` in a constraint.

How can I do this?

EDIT: I tried

```
dependencies = ["kaleido<0.2.1.post1"]
```

But then got a weird error:

```
  × Failed to build `sklearn==0.0.post12`
The 'sklearn' PyPI package is deprecated, use 'scikit-learn'
```

sklearn 0.0.post12 is ancient!

```
dependencies = ["kaleido<=0.2.1"]
```

Seems to be working, but it's taking _ages_ to install - far longer than both `pip` and `conda`, making me think something isn't working correctly (`warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.`)

---

_Comment by @konstin on 2025-01-10 20:43_

With this input:

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
  "pycaret",
]

[tool.uv]
constraint-dependencies = ["kaleido!=0.2.1.post1"]
```

I get a resolution that does not contain `sklearn==0.0.post12` and - at least on linux python 3.11 - successfully installs.

> Seems to be working, but it's taking ages to install - far longer than both pip and conda, making me think something isn't working correctly (warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.)

This usually happens when the cache and the install directory aren't on the same drive, but should not have a _massive_ impact. Usually, something really is some package not having a wheel for the target environment, and that build takes a long time. You can get more information running with `-v`.

---

_Comment by @jamesdeluk on 2025-01-11 09:10_

> With this input:
> 

To confirm, you mean edit the toml then `uv add pycaret`? I did that and it seems to have worked - thanks! Or is there a way to "run" the toml directly, similar to building from a requirements file?

> This usually happens when the cache and the install directory aren't on the same drive, but should not have a _massive_ impact. Usually, something really is some package not having a wheel for the target environment, and that build takes a long time. You can get more information running with `-v`.
>

Turns out it was because I was working within Google Drive, so it was the annoying upload/download causing the delay.

---

_Comment by @konstin on 2025-01-13 19:20_

Inside of `docker run --rm -it -v $(pwd):/io ghcr.io/astral-sh/uv:python3.11-bookworm bash`, i can create a pyproject.toml with:

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.11"

[tool.uv]
constraint-dependencies = ["kaleido!=0.2.1.post1"]
```

and run
```
uv add pycaret
```

I get a lockfile with sklearn-compat 0.1.3, but no `sklearn==0.0.post12`.

---

_Comment by @this-josh on 2025-02-20 11:19_

To anyone looking to install kaleido itself you can install the previous version with `uv add kaleido==0.2.0` which works fine on mac for me 

---

_Referenced in [astral-sh/uv#11726](../../astral-sh/uv/issues/11726.md) on 2025-02-24 15:15_

---

_Referenced in [astral-sh/uv#12507](../../astral-sh/uv/issues/12507.md) on 2025-04-08 09:04_

---

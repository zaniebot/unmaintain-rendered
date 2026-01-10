```yaml
number: 1504
title: "Automatically detect virtual environments when used via `python -m uv`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/venv-python
created_at: 2024-02-16T15:48:35Z
updated_at: 2024-02-16T18:55:24Z
url: https://github.com/astral-sh/uv/pull/1504
synced_at: 2026-01-10T15:33:24Z
```

# Automatically detect virtual environments when used via `python -m uv`

---

_Pull request opened by @zanieb on 2024-02-16 15:48_

Closes https://github.com/astral-sh/uv/issues/1501

---

_Comment by @zanieb on 2024-02-16 16:05_

cc @altendky :)

---

_@henryiii reviewed on 2024-02-16 16:10_

---

_Review comment by @henryiii on `python/uv/__main__.py`:17 on 2024-02-16 16:10_

Shouldn't this be something like `(Path(sys.prefix) / "pyvenv.cfg").exists()` (probably without the pathlib for speed, but I'm lazy here)? This assumes that there is exactly one level between the `bin` or `Scripts` dir and the base prefix, which I don't think is guaranteed.

---

_@zanieb reviewed on 2024-02-16 16:11_

---

_Review comment by @zanieb on `python/uv/__main__.py`:17 on 2024-02-16 16:11_

Thanks! I thought what I was doing might be brittle.. let me look into that.

---

_Comment by @altendky on 2024-02-16 16:13_

Thanks!

```console
$ python3.11 -m venv venv
```

```console
$ venv/bin/python -m pip install 'uv @ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6'
Collecting uv@ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6
  Using cached uv-0.1.2-py3-none-linux_x86_64.whl
Installing collected packages: uv
Successfully installed uv-0.1.2

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: /home/altendky/tmp/venv/bin/python -m pip install --upgrade pip
```

Installed `main` above and see it failing below.

```console
$ venv/bin/python -m uv pip install attrs
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
```

```console
$ venv/bin/python -m pip install 'uv @ git+https://github.com/astral-sh/uv@967188a914df3461b92de27e9b9234fd8c22be85'
Collecting uv@ git+https://github.com/astral-sh/uv@967188a914df3461b92de27e9b9234fd8c22be85
  Using cached uv-0.1.1-py3-none-linux_x86_64.whl
Installing collected packages: uv
  Attempting uninstall: uv
    Found existing installation: uv 0.1.2
    Uninstalling uv-0.1.2:
      Successfully uninstalled uv-0.1.2
Successfully installed uv-0.1.1

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: /home/altendky/tmp/venv/bin/python -m pip install --upgrade pip
```

Installed this PR above and see it succeeding below.

```console
$ venv/bin/python -m uv pip install attrs
Resolved 1 package in 107ms
Installed 1 package in 9ms
 + attrs==23.2.0
```

---

_@altendky reviewed on 2024-02-16 16:16_

---

_Review comment by @altendky on `python/uv/__main__.py`:17 on 2024-02-16 16:16_

https://docs.python.org/3/library/sysconfig.html#installation-paths Looks like you can sometimes get a `posix_prefix`.  Might lead to more details about 'correctness' as well.

---

_Review requested from @charliermarsh by @zanieb on 2024-02-16 16:33_

---

_Merged by @zanieb on 2024-02-16 18:55_

---

_Closed by @zanieb on 2024-02-16 18:55_

---

_Branch deleted on 2024-02-16 18:55_

---

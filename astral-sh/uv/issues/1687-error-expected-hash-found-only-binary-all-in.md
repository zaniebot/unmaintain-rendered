---
number: 1687
title: "error: Expected '--hash', found '\"--only-binary=:all:\"' in `requirements.txt`"
type: issue
state: closed
author: hugovk
labels:
  - duplicate
assignees: []
created_at: 2024-02-19T12:14:19Z
updated_at: 2024-02-19T15:37:10Z
url: https://github.com/astral-sh/uv/issues/1687
synced_at: 2026-01-10T01:23:08Z
---

# error: Expected '--hash', found '"--only-binary=:all:"' in `requirements.txt`

---

_Issue opened by @hugovk on 2024-02-19 12:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```console
❯ pip --version
pip 24.0 from /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages/pip (python 3.12)

❯ uv --version
uv 0.1.5
```

Both pip and uv can install NumPy with `--only-binary=":all:"` (this only installs from binaries, don't attempt building from source):

```console
❯ pip install numpy --only-binary=":all:"
Collecting numpy
  Using cached numpy-1.26.4-cp312-cp312-macosx_11_0_arm64.whl.metadata (61 kB)
Using cached numpy-1.26.4-cp312-cp312-macosx_11_0_arm64.whl (13.7 MB)
Installing collected packages: numpy
Successfully installed numpy-1.26.4

❯ uv pip install numpy --only-binary=":all:"
Resolved 1 package in 138ms
Downloaded 1 package in 960ms
Installed 1 package in 16ms
 + numpy==1.26.4
```

However if I have a requirements file containing:

```txt
numpy --only-binary=:all:
```

pip works:

```console
❯ pip install -r requirements.txt
Collecting numpy (from -r requirements.txt (line 1))
  Using cached numpy-1.26.4-cp312-cp312-macosx_11_0_arm64.whl.metadata (61 kB)
Using cached numpy-1.26.4-cp312-cp312-macosx_11_0_arm64.whl (13.7 MB)
Installing collected packages: numpy
Successfully installed numpy-1.26.4
```

But uv fails:

```console
❯ uv pip install -r requirements.txt --verbose
 uv::requirements::from_source source=requirements.txt
error: Expected '--hash', found '"--only-binary=:all:"' in `requirements.txt` at position 25
```

There are other options like `--no-binary`, `--prefer-binary` and more:

* https://pip.pypa.io/en/stable/reference/requirements-file-format/#supported-options


---

_Referenced in [tox-dev/tox-uv#16](../../tox-dev/tox-uv/issues/16.md) on 2024-02-19 12:18_

---

_Comment by @charliermarsh on 2024-02-19 14:30_

Ah yeah, we don't support those in requirements files right now. We can probably add them. I think I will merge with https://github.com/astral-sh/uv/issues/1461, since it's the same issue but for `--no-binary`.

---

_Closed by @charliermarsh on 2024-02-19 14:30_

---

_Label `duplicate` added by @zanieb on 2024-02-19 15:37_

---

_Referenced in [astral-sh/uv#4420](../../astral-sh/uv/issues/4420.md) on 2024-06-19 21:51_

---

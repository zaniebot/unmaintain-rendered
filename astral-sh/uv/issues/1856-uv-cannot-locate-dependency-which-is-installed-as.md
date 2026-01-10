```yaml
number: 1856
title: Uv cannot locate dependency which is installed as editable
type: issue
state: closed
author: kkpattern
labels:
  - bug
assignees: []
created_at: 2024-02-22T05:22:57Z
updated_at: 2024-04-01T21:31:46Z
url: https://github.com/astral-sh/uv/issues/1856
synced_at: 2026-01-10T05:40:32Z
```

# Uv cannot locate dependency which is installed as editable

---

_Issue opened by @kkpattern on 2024-02-22 05:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Given two packages `foo` and `bar`, `bar` depends on `foo`.

The `foo/pyproject.toml`:

```toml
[project]
name = "foo"
dependencies = []
version = "0.0.1"


[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

The `bar/pyproject.toml`:

```toml
[project]
name = "bar"
dependencies = ["foo"]
version = "0.0.1"


[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

First, install foo as editable: 

```bash
uv pip install -e ./foo
```

Then uv will fail to install bar:

```bash
uv pip install -e ./bar
   Built file:///home/test/uv/depends_on_dev/bar                                                                Built 1 editable in 996ms
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of foo and bar==0.0.1 depends on foo, we can conclude that bar==0.0.1 cannot be used.
      And because you require bar==0.0.1, we can conclude that the requirements are unsatisfiable.
```

Pip can install bar in this case:

```bash
pip install -e ./bar
Obtaining file:///home/test/uv/depends_on_dev/bar
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Installing backend dependencies ... done
  Preparing editable metadata (pyproject.toml) ... done
Requirement already satisfied: foo in ./.venv/lib/python3.11/site-packages (from bar==0.0.1) (0.0.1)
Building wheels for collected packages: bar
  Building editable for bar (pyproject.toml) ... done
  Created wheel for bar: filename=bar-0.0.1-0.editable-py3-none-any.whl size=2417 sha256=41194ddee86dcd04067762778ced041e1c5fe2f5480282e39eadf9424629ea9d
  Stored in directory: /tmp/pip-ephem-wheel-cache-fs8mgp4c/wheels/23/5a/37/0e1593ffa950e8b00faa7776b41fc4d9c247b80fd8ceacd332
Successfully built bar
Installing collected packages: bar
Successfully installed bar-0.0.1
```

---

_Comment by @charliermarsh on 2024-02-22 05:27_

This is a duplicate of https://github.com/astral-sh/uv/issues/1661.

---

_Closed by @charliermarsh on 2024-02-22 05:27_

---

_Label `bug` added by @charliermarsh on 2024-02-22 05:27_

---

_Comment by @zanieb on 2024-04-01 21:31_

Hi! This should be addressed in the latest release ([0.1.27](https://github.com/astral-sh/uv/releases/tag/0.1.27)) via #2596.

Let us know if you have any problems.

---

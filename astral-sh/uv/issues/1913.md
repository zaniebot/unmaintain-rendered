```yaml
number: 1913
title: Changes to pyproject.toml dependency versions not updated on install
type: issue
state: closed
author: RonNabuurs
labels:
  - bug
assignees: []
created_at: 2024-02-23T13:45:01Z
updated_at: 2024-09-10T01:46:11Z
url: https://github.com/astral-sh/uv/issues/1913
synced_at: 2026-01-10T04:45:09Z
```

# Changes to pyproject.toml dependency versions not updated on install

---

_Issue opened by @RonNabuurs on 2024-02-23 13:45_

Say I have the following `pyproject.toml`

```toml
[project]
name = "example"
version = "0.0.0"
dependencies = [
  "httpcore==1.0.2"
]
requires-python = ">=3.8"
```

To create a venv I would run `uv venv`. This creates a venv in which I can install packages. For comparison I'll also install pip in that environment `uv pip install pip`. 

Now I can run the following command to create an editable install with httpcore installed:

```bash
uv pip install -e .
```

This results in an venv with httpcore and it's dependencies installed. 

Now comes the issue, when I change dep versions in the pyproject.toml uv will not reinstall that package. E.g.

Change the pyproject.toml to:
```toml
[project]
name = "example"
version = "0.0.0"
dependencies = [
  "httpcore==1.0.1"
]
requires-python = ">=3.8"
```

Run uv again `uv pip install -e .`. This results in 
```bash
Audited 1 package in 3ms
```

Though if I run the same command with pip it will reinstall the new version:
```
.venv/bin/pip install -e .

...
  Attempting uninstall: example
    Found existing installation: example 0.0.0
    Uninstalling example-0.0.0:
      Successfully uninstalled example-0.0.0
Successfully installed example-0.0.0 httpcore-1.0.1
```

---

_Label `bug` added by @konstin on 2024-02-23 14:09_

---

_Comment by @charliermarsh on 2024-02-23 15:17_

You can run with `--reinstall` to force uv to reinstall it.

---

_Comment by @danielhollas on 2024-02-23 15:33_

Just noting that I have also ran into this yesterday with uv v0.1.8.
In my case I was changing dependencies in `setup.cfg` (`pyproject.toml` only containing the `build-system` table),
and `uv` was not picking up the changes upon subsequent invocations.

---

_Comment by @charliermarsh on 2024-02-23 15:46_

I can improve this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-24 03:01_

---

_Closed by @charliermarsh on 2024-02-24 19:55_

---

_Comment by @charliermarsh on 2024-09-10 01:46_

For onlookers, we have a new API whereby you can add additional files to consider when invalidating the cache. You can also include the current Git commit (i.e., invalidate whenever the SHA changes).

Looks like this:

```toml
[tool.uv]
cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" }, { git = true }]
```

See: https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata.


---

---
number: 1991
title: Dynamic optional dependencies not audited upon re-install
type: issue
state: closed
author: kendallbailey
labels:
  - enhancement
assignees: []
created_at: 2024-02-26T21:17:03Z
updated_at: 2024-02-28T01:56:26Z
url: https://github.com/astral-sh/uv/issues/1991
synced_at: 2026-01-07T13:12:16-06:00
---

# Dynamic optional dependencies not audited upon re-install

---

_Issue opened by @kendallbailey on 2024-02-26 21:17_

uv version: 0.1.11
platform: Ubuntu 20.04, python 3.10

I have a `pyproject.toml` that uses dynamic optional dependencies

```
[tool.setuptools.dynamic]
optional-dependencies.tests = {file = ["reqs/test.txt"]}
```

After an editable install using `uv`, and then editing the `reqs/test.txt` file, a re-install doesn't notice the changes in the optional dependencies the way `pip` does.  I have to `touch pyproject.toml` to convince `uv` to look at the dynamic optional dependencies and ensure the changes are applied to the environment. I'm used to doing `pip install -e ".[tests]"` after updating `test.txt`, but `uv pip install -e ".[tests]"` says "Audited 1 package in 90ms" and does not install the package(s) that were added to `test.txt`.

The above is a simplified example. In reality I have multiple optional dependencies, many of which involve multiple `.txt` files. 


---

_Comment by @charliermarsh on 2024-02-26 21:19_

This is mostly by design. We don't validate changes across the full environment. But you can pass `--reinstall` to force it.

---

_Closed by @kendallbailey on 2024-02-26 21:36_

---

_Comment by @sbidoul on 2024-02-26 22:28_

@charliermarsh would you reconsider this? Checking the timestamp of pyproject.toml makes sense except when some metadata is declared as dynamic, in which case it can be computed from other sources than pyproject.toml itself. This way you could have the fastest performance for fully static pyproject.toml, but keep a correct behaviour for those who need dynamic metadata. 

---

_Comment by @charliermarsh on 2024-02-26 23:08_

@sbidoul -- Yeah definitely open to reconsidering it, but how could we "tell" that some metadata is dynamic?

---

_Comment by @charliermarsh on 2024-02-26 23:08_

Oh, if you have the above in your `pyproject.toml`, does it in turn require that have you `dynamic = ["dependencies"]` set?

---

_Comment by @sbidoul on 2024-02-26 23:11_

If there is a `[project]` table, yes, every dynamic metadata MUST be declared in `project.dynamic`. If there is no `[project]` table, everything is to be considered dynamic.

---

_Reopened by @charliermarsh on 2024-02-26 23:12_

---

_Comment by @charliermarsh on 2024-02-26 23:12_

That makes sense, we can probably support this.

---

_Comment by @charliermarsh on 2024-02-26 23:13_

Or rather, we can support this, I just have to get through a few other things first.

---

_Label `enhancement` added by @charliermarsh on 2024-02-26 23:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-26 23:17_

---

_Referenced in [astral-sh/uv#2029](../../astral-sh/uv/pulls/2029.md) on 2024-02-28 01:28_

---

_Closed by @charliermarsh on 2024-02-28 01:56_

---

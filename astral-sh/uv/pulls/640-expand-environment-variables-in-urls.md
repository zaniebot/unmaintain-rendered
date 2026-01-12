```yaml
number: 640
title: Expand environment variables in URLs
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/env-var
created_at: 2023-12-13T21:52:47Z
updated_at: 2024-12-19T06:58:53Z
url: https://github.com/astral-sh/uv/pull/640
synced_at: 2026-01-12T16:04:05Z
```

# Expand environment variables in URLs

---

_@charliermarsh_

## Summary

This PR enables users to express relative dependencies via environment variables. Like pip, PDM, Hatch, Rye, and others, we now allow users to express dependencies like:

```text
flask @ file://${PROJECT_ROOT}/flask-3.0.0-py3-none-any.whl
```

In the compiled requirements file, we'll also preserve the unexpanded environment variable.

Closes https://github.com/astral-sh/puffin/issues/592.


---

_Review comment by @konstin on `crates/pep508-rs/src/verbatim_url.rs`:110 on 2023-12-14 12:38_

Is it intentional that `PROJECT_ROOT` defined in the user env takes precedence over `PROJECT_ROOT?

---

_@konstin approved on 2023-12-14 12:39_

---

_@charliermarsh reviewed on 2023-12-14 15:04_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/verbatim_url.rs`:110 on 2023-12-14 15:04_

Yeah

---

_Merged by @charliermarsh on 2023-12-14 15:09_

---

_Closed by @charliermarsh on 2023-12-14 15:09_

---

_Branch deleted on 2023-12-14 15:09_

---

_Comment by @alelavelli on 2024-03-12 10:09_

How it is possible to dynamically read the local package content like in poetry with `my-local-package = { path = "local/package/path", develop = true }`? 

Because for now, the only solution I found is to recompile the pypropject.toml with `--refresh-package my-local-package`and then reinstall it

---

_Comment by @charliermarsh on 2024-03-12 14:27_

@alelavelli -- Are you looking for `uv pip install -e ./local/package/path`?

---

_Comment by @alelavelli on 2024-03-12 14:31_

@charliermarsh 

Yes but directly from the pyproject.toml. Something like poetry:

[tool.poetry.dependencies]
my-local-package = {path = "../my-local-package", develop = true}

but with uv, like this:
[project]
dependencies = [
    "my-package @ file:/path/to/my/package" # with something to say -e
]

---

_Comment by @charliermarsh on 2024-03-12 16:55_

Unfortunately I think it's not possible in `pyproject.toml`, because it's just not part of the standard PEP 508 specification.

---

_Comment by @fruch on 2024-12-19 06:58_

> Unfortunately I think it's not possible in `pyproject.toml`, because it's just not part of the standard PEP 508 specification.

just an update for whom running into it, `uv add` now supports it

```
uv add --editable ./local-path/
```

which maps to `pyproject.toml`
```
[tool.uv.sources]
my_package = { path = "local-path", editable = true }
```

---

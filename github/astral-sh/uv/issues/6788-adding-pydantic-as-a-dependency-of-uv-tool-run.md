---
number: 6788
title: "Adding `pydantic` as a dependency of `uv tool run refurb`"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2024-08-29T02:20:27Z
updated_at: 2024-08-29T02:23:38Z
url: https://github.com/astral-sh/uv/issues/6788
synced_at: 2026-01-07T13:12:17-06:00
---

# Adding `pydantic` as a dependency of `uv tool run refurb`

---

_Issue opened by @jamesbraza on 2024-08-29 02:20_

With `uv==0.4.0` and a `pyproject.toml` like so:

```toml
[tool.mypy]
plugins = ["pydantic.mypy"]

[tool.refurb]
enable_all = true
```

Running the below, I get an error:

```none
> uv tool run refurb src
pyproject.toml:1:1: error: Error importing plugin "pydantic.mypy": No module named 'pydantic'  [misc]
    [build-system]
    ^
```

How can I add `pydantic` as a dependency of `uv tool run refurb`?

---

_Comment by @jamesbraza on 2024-08-29 02:22_

Running `uv tool upgrade` doesn't seem to work:

```bash
> uv tool upgrade refurb --upgrade-package pydantic
Nothing to upgrade
```

---

_Comment by @jamesbraza on 2024-08-29 02:23_

Nevermind it was documented here: https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies

```bash
uv tool install --with pydantic refurb
```

---

_Closed by @jamesbraza on 2024-08-29 02:23_

---

```yaml
number: 2656
title: Same way to ban import of certain modules?
type: issue
state: closed
author: ehdr
labels: []
assignees: []
created_at: 2023-02-08T11:26:21Z
updated_at: 2023-02-08T15:45:24Z
url: https://github.com/astral-sh/ruff/issues/2656
synced_at: 2026-01-12T15:54:43Z
```

# Same way to ban import of certain modules?

---

_@ehdr_

It would be great to have a kind-of inverse of [`required-imports`](https://github.com/charliermarsh/ruff#required-imports), to **prevent** certain modules from being imported.

In our specific use case, we'd like to prevent certain modules from being imported only in `tests/*`.

Googling, I found some seemingly related solutions like [python-import-linter](https://921kiyo.com/python-import-linter/) and [pylint-restricted-imports](https://pypi.org/project/pylint-restricted-imports/), but haven't used either.

(And mandatory praise: ruff is ⭐ ! Please continue and build better versions of every tool in the Python ecosystem!)

---

_Comment by @ngnpope on 2023-02-08 12:07_

Can you use [`banned-api`](https://github.com/charliermarsh/ruff#flake8-tidy-imports-tid) to achieve this?

I use something like the following:
```toml
[tool.ruff.flake8-tidy-imports]
[tool.ruff.flake8-tidy-imports.banned-api]
"dateutil.tz".msg = "Use `zoneinfo` instead."
"mock".msg = "Use `pytest-mock` instead."
"pytz".msg = "Use `zoneinfo` instead."
"unittest".msg = "Use `pytest` instead."
"unittest.mock".msg = "Use `pytest-mock` instead."
```

> In our specific use case, we'd like to prevent certain modules from being imported only in `tests/*`.

Not sure this can be achieved though at the moment...

---

_Comment by @ehdr on 2023-02-08 12:11_

Oh nice! Yes this looks very close to what I need, except, as you point out, that I'd need to also restrict it to only `tests/*` somehow.

---

_Comment by @ngnpope on 2023-02-08 12:15_

Come to think of it, I think you might be able to achieve this with `extend`?

Try adding `tests/ruff.toml` with something like the following:

```toml
extend = "../pyproject.toml"

[flake8-tidy-imports]
[flake8-tidy-imports.banned-api]
"dateutil.tz".msg = "Use `zoneinfo` instead."
"mock".msg = "Use `pytest-mock` instead."
"pytz".msg = "Use `zoneinfo` instead."
"unittest".msg = "Use `pytest` instead."
"unittest.mock".msg = "Use `pytest-mock` instead."
```

See the [documentation](https://github.com/charliermarsh/ruff#pyprojecttoml-discovery) for details...

---

_Comment by @ehdr on 2023-02-08 12:31_

Beautiful! Ever closer.

For more detail, I want warning when we import from a certain top level module (e.g. `import top` or `from top import ...`), but allow importing from submodules of `top` (e.g. `import top.internal` or `from top.internal import ...`). Since [`flake8-tidy-imports`](https://pypi.org/project/flake8-tidy-imports/) seems to support `*` wildcards, would it be possible to change it so `"top".msg = "Import submodules directly instead."` matches only `top` exactly, and you'd have to specify the `top.*` wildcard if you want to match submodules too? I guess that would not be backwards compatible though.

---

_Comment by @ngnpope on 2023-02-08 12:42_

Yeah, I think that's not possible as this stands.

---

_Comment by @charliermarsh on 2023-02-08 15:43_

Thanks for the kind words :)

Ah yeah, that's not possible today -- to disallow the top-level import, but allow importing submodules, since we treat `from top.internal` as matching `top`. I was kind of hoping to avoid adding globbing there, just for simplicity. But I'll open a new issue to track that functionality. (I likely won't get to it soon, but it'd be good to know if others need this too.)

---

_Comment by @charliermarsh on 2023-02-08 15:45_

Tracking here: #2660.

---

_Closed by @charliermarsh on 2023-02-08 15:45_

---

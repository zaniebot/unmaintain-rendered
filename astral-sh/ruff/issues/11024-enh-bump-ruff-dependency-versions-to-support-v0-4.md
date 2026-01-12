```yaml
number: 11024
title: "ENH: Bump `ruff` dependency versions to support `v0.4.0` and Python 3.12"
type: issue
state: closed
author: HenryAsa
labels: []
assignees: []
created_at: 2024-04-19T03:30:09Z
updated_at: 2024-04-19T03:37:56Z
url: https://github.com/astral-sh/ruff/issues/11024
synced_at: 2026-01-12T15:54:50Z
```

# ENH: Bump `ruff` dependency versions to support `v0.4.0` and Python 3.12

---

_@HenryAsa_

With the release of [`v0.4.0` of `ruff`](https://github.com/astral-sh/ruff/releases/tag/v0.4.0), I noticed that some of `ruff`'s dependencies were not updated to their latest versions.  The [`ruff-pre-commit`](https://github.com/astral-sh/ruff-pre-commit) package released [`v0.4.0`](https://github.com/astral-sh/ruff-pre-commit/releases/tag/v0.4.0) at the same time `ruff` was updated, but `ruff` still referenced `v0.3.7` of the package, not the newly updated version.

In a similar light, I noticed that the version of the [`dill`](https://github.com/uqfoundation/dill) package being used was not the latest version that now [fully supports Python 3.12](https://github.com/uqfoundation/dill/releases/tag/0.3.8).


---

_Closed by @charliermarsh on 2024-04-19 03:37_

---

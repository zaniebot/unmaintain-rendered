```yaml
number: 11025
title: "ENH: Bump `ruff` dependency versions to support the latest release of `v0.4.0` and Python 3.12"
type: pull_request
state: merged
author: HenryAsa
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2024-04-19T03:31:34Z
updated_at: 2024-04-19T03:37:55Z
url: https://github.com/astral-sh/ruff/pull/11025
synced_at: 2026-01-10T22:37:01Z
```

# ENH: Bump `ruff` dependency versions to support the latest release of `v0.4.0` and Python 3.12

---

_Pull request opened by @HenryAsa on 2024-04-19 03:31_

## Summary

With the release of [`v0.4.0`](https://github.com/astral-sh/ruff/releases/tag/v0.4.0) of `ruff`, I noticed that some of `ruff`'s dependencies were not updated to their latest versions.  The [`ruff-pre-commit`](https://github.com/astral-sh/ruff-pre-commit) package released [`v0.4.0`](https://github.com/astral-sh/ruff-pre-commit/releases/tag/v0.4.0) at the same time `ruff` was updated, but `ruff` still referenced `v0.3.7` of the package, not the newly updated version.  I updated the `ruff-pre-commit` reference to be `v0.4.0`.

In a similar light, I noticed that the version of the [`dill`](https://github.com/uqfoundation/dill) package being used was not the latest version.  I bumped `dill` from version `0.3.7` to `0.3.8`, which now [fully supports Python 3.12](https://github.com/uqfoundation/dill/releases/tag/0.3.8).

## Related Issues

Resolves #11024

---

_Comment by @charliermarsh on 2024-04-19 03:35_

Thanks for bumping.

---

_Label `internal` added by @charliermarsh on 2024-04-19 03:35_

---

_Merged by @charliermarsh on 2024-04-19 03:37_

---

_Closed by @charliermarsh on 2024-04-19 03:37_

---

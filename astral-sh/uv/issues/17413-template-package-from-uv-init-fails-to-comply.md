```yaml
number: 17413
title: "Template package from `uv init` fails to comply with RUF067"
type: issue
state: open
author: martimlobao
labels:
  - needs-decision
assignees: []
created_at: 2026-01-12T09:43:57Z
updated_at: 2026-01-14T01:37:07Z
url: https://github.com/astral-sh/uv/issues/17413
synced_at: 2026-01-14T02:32:50Z
```

# Template package from `uv init` fails to comply with RUF067

---

_@martimlobao_

### Summary

The default `uv init --package example-pkg` package template from the [docs](https://docs.astral.sh/uv/concepts/projects/init/#packaged-applications) fails to comply with Ruff's [RUF067 rule](https://docs.astral.sh/ruff/rules/non-empty-init-module/#non-empty-init-module-ruf067).

### Platform

Darwin 25.2.0 arm64

### Version

uv 0.9.22 (Homebrew 2026-01-06)

### Python version

_No response_

---

_Label `bug` added by @martimlobao on 2026-01-12 09:43_

---

_Comment by @my1e5 on 2026-01-12 12:31_

Related:

- https://github.com/astral-sh/uv/issues/8178

Which advocates that

> `__init__.py` should not contain general purpose code

---

_Label `bug` removed by @zanieb on 2026-01-14 01:35_

---

_Label `needs-decision` added by @zanieb on 2026-01-14 01:35_

---

_Comment by @zanieb on 2026-01-14 01:37_

I don't think this qualifies as a "bug" since it's working as intended.

I'm not sure we should make up our own file name for a `main` function to go in. I think having it in an `__init__.py` is reasonable for an initialized project, though I'd probably move it elsewhere once I was further along.

We can consider another pattern regardless.

---

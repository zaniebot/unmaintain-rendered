---
number: 7974
title: "support module-level banned imports in `flake8-tidy-imports.banned-api`"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2023-10-16T04:33:17Z
updated_at: 2023-10-16T13:34:41Z
url: https://github.com/astral-sh/ruff/issues/7974
synced_at: 2026-01-07T13:12:15-06:00
---

# support module-level banned imports in `flake8-tidy-imports.banned-api`

---

_Issue opened by @DetachHead on 2023-10-16 04:33_

i wrote a pylint plugin to enforce certain rules about which modules can import each other within my project: https://github.com/detachhead/pylint-module-boundaries

but it would be nice to be able to ditch pylint in favor of ruff. if `flake8-tidy-imports.banned-api` supported setting rules per module that would render the pylint plugin obsolete. something like this:

```toml
[tool.ruff.flake8-tidy-imports.banned-api]
"foo.bar".msg = "this import is banned globally"

[tool.ruff.flake8-tidy-imports.banned-api.per-file-rules."bar/**/*.py"]
"foo.bar".msg =  "this import is banned in the `bar` module"
```

---

_Comment by @charliermarsh on 2023-10-16 13:34_

I believe this is a dupe of https://github.com/astral-sh/ruff/issues/7696.

---

_Closed by @charliermarsh on 2023-10-16 13:34_

---

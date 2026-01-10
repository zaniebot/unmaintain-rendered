```yaml
number: 12459
title: Ruff formatter per file ignores problem
type: issue
state: closed
author: nonoash
labels:
  - configuration
assignees: []
created_at: 2024-07-22T18:13:18Z
updated_at: 2024-07-22T18:23:20Z
url: https://github.com/astral-sh/ruff/issues/12459
synced_at: 2026-01-10T11:09:54Z
```

# Ruff formatter per file ignores problem

---

_Issue opened by @nonoash on 2024-07-22 18:13_

Hi,

i'd like to setup different line lengths based on file extensions, for example default 120 and .ipynb 88
I know there is already a way for the linter with:
`[tool.ruff.lint.per-file-ignores]`

but thats for linting, is there something similar for the formatter or a way to fit my case? because i can't find infos on it or make it work with something like:
```
[tool.ruff]
line-length = 120

[tool.ruff.format.per-file-ignores]
"**/*.ipynb" = { line-length = 88 }
```

or
```
[tool.ruff]
line-length = 120

[tool.ruff.format.specific-files]
"**/*.ipynb" = { line-length = 88 }
```

I'm using Ruff v0.5.3 with native server,
Thanks.

---

_Comment by @charliermarsh on 2024-07-22 18:20_

Unfortunately we don't support this today, but it's tracked in https://github.com/astral-sh/ruff/issues/7696.

---

_Comment by @charliermarsh on 2024-07-22 18:20_

Merging into that issue.

---

_Closed by @charliermarsh on 2024-07-22 18:20_

---

_Label `configuration` added by @charliermarsh on 2024-07-22 18:20_

---

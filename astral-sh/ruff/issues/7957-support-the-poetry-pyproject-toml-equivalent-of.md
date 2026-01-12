```yaml
number: 7957
title: "Support the Poetry pyproject.toml equivalent of `requires-python`"
type: issue
state: closed
author: newbery
labels: []
assignees: []
created_at: 2023-10-14T02:57:17Z
updated_at: 2023-10-14T19:26:25Z
url: https://github.com/astral-sh/ruff/issues/7957
synced_at: 2026-01-12T15:54:47Z
```

# Support the Poetry pyproject.toml equivalent of `requires-python`

---

_@newbery_

The docs on the `target-version` setting, 
https://docs.astral.sh/ruff/settings/#target-version,
say the following:

    If omitted, and Ruff is configured via a pyproject.toml file, the target version
    will be inferred from its project.requires-python field

In the Poetry case, the python version would actually be elsewhere, under `tool.poetry.dependencies.python`. Can the inference logic be updated to support the Poetry version of pyproject.toml?


---

_Comment by @charliermarsh on 2023-10-14 18:36_

Thanks for the note. This has been discussed a bit in the past (https://github.com/astral-sh/ruff/issues/5448), and the suggestion from a Poetry maintainer was to _not_ support this as Poetry is going to deprecate `tool.poetry.dependencies.python` in favor of the standards-compliant `requires-python`.

---

_Closed by @charliermarsh on 2023-10-14 18:36_

---

_Comment by @newbery on 2023-10-14 19:26_

Ahh, that make sense. Thanks  👍🏻

---

```yaml
number: 8797
title: Disallow Certain Rules from Inline Error Supression
type: issue
state: open
author: travis-cook-sfdc
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-11-20T22:15:05Z
updated_at: 2024-07-11T04:08:24Z
url: https://github.com/astral-sh/ruff/issues/8797
synced_at: 2026-01-12T15:54:48Z
```

# Disallow Certain Rules from Inline Error Supression

---

_@travis-cook-sfdc_

hi there -

I help to manage a fairly large repo at my company.

In certain scenarios, we'd like to use ruff rule [TID251](https://docs.astral.sh/ruff/rules/banned-api/) to help prevent certain functions from being used in our repo.

While this rule currently works fine, anyone can add `noqa: TID251` to their code.  I have no problem with someone doing this, but I wish there was a way to have this trigger a required review by the repo owner.

Ideally, we could use `per-file-ignores` to ignore this rule in certain allow-listed cases, but there's a few issues here:

```
[tool.ruff.per-file-ignores]
"__init__.py" = ["TID251"]
"path/to/file.py" = ["TID251"]
```

1️⃣  It's still possible to do an in-line ignore.  It could be nice to not allow inline ignores for this rule
2️⃣ This allows all banned apis in this file as opposed to just one specific.

Perhaps ruff isn't the right tool for the job here, but wanted to bring this up as a potential feature.



---

_Label `configuration` added by @charliermarsh on 2023-11-21 01:36_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-21 01:36_

---

_Comment by @KotlinIsland on 2024-07-11 04:08_

take a look at [pylint-module-boundaries](https://pypi.org/project/pylint-module-boundaries/), maybe that would suite your situation better

---

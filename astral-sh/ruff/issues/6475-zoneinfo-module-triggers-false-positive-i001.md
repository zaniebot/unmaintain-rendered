```yaml
number: 6475
title: zoneinfo module triggers false positive I001
type: issue
state: closed
author: Atilla
labels:
  - needs-info
assignees: []
created_at: 2023-08-10T09:35:42Z
updated_at: 2023-08-10T14:05:14Z
url: https://github.com/astral-sh/ruff/issues/6475
synced_at: 2026-01-12T15:54:46Z
```

# zoneinfo module triggers false positive I001

---

_@Atilla_

Ruff considers the python stdlib module `zoneinfo` to be 3rd party now, so it sorts it in the wrong section. This disagrees with `isort`. 

It seems to have regressed between 0.0.283 and 0.0.284. 

Relevant ruff config for reference. 

 [tool.ruff.isort]
section-order = [
    "future",
    "standard-library",
    "django",
    "third-party",
    "first-party",
    "local-folder",
]
combine-as-imports = true

[tool.ruff.isort.sections]
django = ["django"]



---

_Comment by @charliermarsh on 2023-08-10 12:49_

zoneinfo was added in 3.9, and we now default to 3.8 as the minimum target version, if it’s not specified via a requires-python in pyproject.toml or Ruff’s target-version support. Does upping your target-version resolve this? (I don’t believe we changed anything related to module classification.)

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-10 12:49_

---

_Comment by @Atilla on 2023-08-10 13:19_

Oh, sorry, I didn't really realise when it was added to stdlib, and that's what's really happening. 

Our pyproject didn't define any of those, which it should have. 

Having the `requires-python`  or `target-version` set higher (at 3.11 in our case) works just fine. 

Thank you for the quick help. 

---

_Closed by @Atilla on 2023-08-10 13:19_

---

_Comment by @charliermarsh on 2023-08-10 14:05_

No worries, sorry for the churn.

---

---
number: 8050
title: "`external` support code groups"
type: issue
state: closed
author: Avasam
labels: []
assignees: []
created_at: 2023-10-18T17:04:06Z
updated_at: 2023-10-27T10:48:03Z
url: https://github.com/astral-sh/ruff/issues/8050
synced_at: 2026-01-10T01:22:47Z
---

# `external` support code groups

---

_Issue opened by @Avasam on 2023-10-18 17:04_

Instead of the following configuration:
```toml
external = [
    # Flake8-PYI issues that have valid noqa suppression reasons
    "Y001",
    "Y008",
    "Y022",
    "Y026",
    "Y029",
    "Y034",
    "Y037",
    "Y038",
    "Y039",
    "Y041",
    "Y042",
    "Y043",
    "Y046",
    "Y047",
    "Y049",
    "Y054",
    "Y057",
    # and more, these are just those currently noqa'd

    # Flake8 has false-positive with ellipsis
    "F821", 
]
[tool.ruff.per-file-ignores]
# False positives in stubs
"*.pyi" = ["F821"] # Undefined name: https://github.com/astral-sh/ruff/issues/3011
```
I'd like to be able to do
```toml
external = [
    # Work with Flake8-PYI
    "Y",
    # Flake8 has false-positive with ellipsis
    "F821"
]
[tool.ruff.per-file-ignores]
# False positives in stubs
"*.pyi" = ["F821"] # Undefined name: https://github.com/astral-sh/ruff/issues/3011
```


---

_Comment by @Avasam on 2023-10-27 10:47_

Closed by #8177 , duplicated by #8174

---

_Closed by @Avasam on 2023-10-27 10:47_

---

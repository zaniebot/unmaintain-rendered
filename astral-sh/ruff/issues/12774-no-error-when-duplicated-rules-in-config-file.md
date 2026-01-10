```yaml
number: 12774
title: no error when duplicated rules in config file
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2024-08-09T05:45:27Z
updated_at: 2024-08-09T06:23:21Z
url: https://github.com/astral-sh/ruff/issues/12774
synced_at: 2026-01-10T11:09:54Z
```

# no error when duplicated rules in config file

---

_Issue opened by @DetachHead on 2024-08-09 05:45_

```json
{
  "lint": {
    "select": [
      "RUF031",
      "RUF031"
    ],
    "ignore": [
      "D106",
      "D106"
    ]
  }
}
```

ruff should report an error when duplicated rules are present in the `select`/`ignore` lists, as it's an obvious mistake.

it could also account for redundant rules, eg:
```jsonc
{
  "lint": {
    "select": [
      "ALL",
      "D106"  // error: this rule is already covered by ALL
    ]
  }
}
```

---

_Comment by @DetachHead on 2024-08-09 05:48_

also:

```jsonc
{
  "lint": {
    "select": [
      "D106",
    ],
    "ignore": [
      "D106" // error: conflicts with "select" section
    ]
  }
}
```

---

_Comment by @MichaReiser on 2024-08-09 06:22_

I'm not sure if ruff should error in this case because it can do a decent job at recovering (it very much seems that you wanted to enable the rule and the ordering in `select` doesn't matter). 

I think what would be nice is if we can lint your pyproject.toml and point out that you repeated a value in the selector. I consider this similar to a lint rule that detects `set(True, True)` and proposes to simplify it to `set(True)`

---

_Comment by @MichaReiser on 2024-08-09 06:23_

I'll merge this into https://github.com/astral-sh/ruff/issues/10487 which raises this problem in a more broader sense.

---

_Closed by @MichaReiser on 2024-08-09 06:23_

---

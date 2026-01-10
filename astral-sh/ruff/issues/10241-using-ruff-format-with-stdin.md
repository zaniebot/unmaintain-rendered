---
number: 10241
title: "Using \"ruff format\" with stdin"
type: issue
state: closed
author: Ga68
labels:
  - question
  - formatter
assignees: []
created_at: 2024-03-05T21:00:29Z
updated_at: 2024-04-12T02:09:50Z
url: https://github.com/astral-sh/ruff/issues/10241
synced_at: 2026-01-10T01:22:49Z
---

# Using "ruff format" with stdin

---

_Issue opened by @Ga68 on 2024-03-05 21:00_

I've started using the Zed editor and want to have ruff format my code (as I'm switching from black). I can set up Zed to format-on-save using black as follows

```
  "language_overrides": {
    "Python": {
      "preferred_line_length": 88,
      "formatter": {
        "external": {
          "command": "black",
          "arguments": ["-"]
        }
      }
    }
  }
```

but when I try to the do the same with ruff, no such luck. Trying either of two below fails to work.

```
          "command": "ruff format",
          "arguments": ["-"]
```
```
          "command": "ruff",
          "arguments": ["format", "-"]
```

---

_Comment by @charliermarsh on 2024-03-05 22:01_

I would expect the latter to work -- any idea what error you're running into? Any trace you could share?

---

_Label `question` added by @MichaReiser on 2024-03-06 09:04_

---

_Comment by @dhruvmanila on 2024-03-07 10:06_

Can you share your entire config? Could it be that there's a `"format_on_save": "off"` somewhere in the global setting? Zed also allows folder specific settings so I would check those as well. Can you also share a code snippet which you're facing this problem?

---

_Label `formatter` added by @zanieb on 2024-03-11 17:16_

---

_Comment by @charliermarsh on 2024-04-12 02:09_

Closing now in the absence of more info, but can always re-open.

---

_Closed by @charliermarsh on 2024-04-12 02:09_

---

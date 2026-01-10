```yaml
number: 8352
title: Ruff formatter missing character limit argument
type: issue
state: closed
author: ryan-cpi
labels:
  - configuration
  - formatter
assignees: []
created_at: 2023-10-30T10:33:26Z
updated_at: 2023-11-02T01:39:54Z
url: https://github.com/astral-sh/ruff/issues/8352
synced_at: 2026-01-10T11:09:50Z
```

# Ruff formatter missing character limit argument

---

_Issue opened by @ryan-cpi on 2023-10-30 10:33_

We are using the ruff pre-commits formatter however there doesn't seem to be a way to pass in an argument to set the line character limit without having a config file dedicated for this. Is it possible to add an argument for this `-l 120`?

```yaml
repos:
-   repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.2
    hooks:
      - id: ruff-format
```

---

_Comment by @zanieb on 2023-10-30 14:17_

Some related discussion at https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7423598, https://github.com/astral-sh/ruff-vscode/issues/6#issuecomment-1783548460, and #8131 

We are considering restoring this option. Note that we do not use character limits, we use line width which accounts for the width of each unicode character.

---

_Label `configuration` added by @zanieb on 2023-10-30 14:17_

---

_Label `formatter` added by @zanieb on 2023-10-30 14:17_

---

_Closed by @zanieb on 2023-11-02 01:39_

---

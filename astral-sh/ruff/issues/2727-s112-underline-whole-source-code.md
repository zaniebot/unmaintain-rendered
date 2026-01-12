```yaml
number: 2727
title: S112 underline whole source code
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-02-10T18:23:20Z
updated_at: 2023-02-10T18:27:19Z
url: https://github.com/astral-sh/ruff/issues/2727
synced_at: 2026-01-12T15:54:43Z
```

# S112 underline whole source code

---

_@spaceone_

`S112` underlines only the continue, which doesn't help me when using `--show-source`:
```
332 |                             continue
    |                             ^^^^^^^^ S112

```
it should show the whole try-except block.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-10 18:25_

---

_Closed by @charliermarsh on 2023-02-10 18:27_

---

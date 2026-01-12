```yaml
number: 1712
title: "uv pip compile pyproject.toml doesn't resolve any dependency"
type: issue
state: closed
author: ClementWalter
labels:
  - duplicate
assignees: []
created_at: 2024-02-19T18:23:23Z
updated_at: 2024-02-19T18:56:20Z
url: https://github.com/astral-sh/uv/issues/1712
synced_at: 2026-01-12T15:58:31Z
```

# uv pip compile pyproject.toml doesn't resolve any dependency

---

_@ClementWalter_

So I tried to migrate from `poetry` to `uv` for the repo https://github.com/kkrt-labs/kakarot but the documented command

```
uv pip compile pyproject.toml -o requirements.txt
```

doesn't return anything

```
$ uv pip compile pyproject.toml -o requirements.txt
warning: Requirements file requirements.txt does not contain any dependencies
Resolved 0 packages in 163ms
```

On the other hand, `poetry export -f requirements.txt -o requirements.txt` does write a meaningful `requirements.txt` file

---

_Comment by @hauntsaninja on 2024-02-19 18:52_

Duplicate of #1619 , #1630 and others 

---

_Comment by @AlexWaygood on 2024-02-19 18:54_

> Duplicate of #1619 , #1630 and others

Agreed

---

_Closed by @AlexWaygood on 2024-02-19 18:54_

---

_Label `duplicate` added by @AlexWaygood on 2024-02-19 18:56_

---

---
number: 15476
title: "[red-knot] No error when using some `types` symbols without importing them"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
  - duplicate
assignees: []
created_at: 2025-01-14T14:46:21Z
updated_at: 2025-02-14T09:47:53Z
url: https://github.com/astral-sh/ruff/issues/15476
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] No error when using some `types` symbols without importing them

---

_Issue opened by @sharkdp on 2025-01-14 14:46_

Red Knot does not raise any errors when using some symbols from `types` without importing them. I haven't done an exhaustive investigation, but all of these should raise errors:
```py
a: Literal[2] =   # no error here
b: Any = 2  # no error here
c: Never = 3
d: Annotated[int, "…"] = 2
e: Union[int, str] = 2
```

---

_Label `bug` added by @sharkdp on 2025-01-14 14:46_

---

_Label `red-knot` added by @sharkdp on 2025-01-14 14:46_

---

_Comment by @AlexWaygood on 2025-01-14 15:54_

I think this is a duplicate of https://github.com/astral-sh/ruff/issues/14099 -- these symbols are imported in typeshed's `builtins.pyi` file, and we haven't yet implemented an understanding the re-export conventions for stub files. (No import in a stub file should be understood as a public re-export unless it uses the explicit `import foo as foo` or `from foo import bar as bar` syntax)

---

_Label `duplicate` added by @sharkdp on 2025-01-14 21:44_

---

_Closed by @sharkdp on 2025-01-14 21:45_

---

_Referenced in [astral-sh/ruff#15848](../../astral-sh/ruff/pulls/15848.md) on 2025-01-31 12:15_

---

_Referenced in [astral-sh/ruff#16073](../../astral-sh/ruff/pulls/16073.md) on 2025-02-12 08:20_

---

_Closed by @dhruvmanila on 2025-02-14 09:47_

---

_Referenced in [astral-sh/ruff#16133](../../astral-sh/ruff/pulls/16133.md) on 2025-02-17 10:33_

---

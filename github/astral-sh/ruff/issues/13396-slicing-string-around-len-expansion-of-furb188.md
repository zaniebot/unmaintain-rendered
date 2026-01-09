---
number: 13396
title: "Slicing string around `len()` (expansion of FURB188 slice-to-remove-prefix-or-suffix)"
type: issue
state: open
author: opk12
labels:
  - rule
assignees: []
created_at: 2024-09-18T18:58:26Z
updated_at: 2024-09-20T18:03:33Z
url: https://github.com/astral-sh/ruff/issues/13396
synced_at: 2026-01-07T13:12:15-06:00
---

# Slicing string around `len()` (expansion of FURB188 slice-to-remove-prefix-or-suffix)

---

_Issue opened by @opk12 on 2024-09-18 18:58_

This old code slices a string, using the `len()` of the prefix (which is a string variable) and suffix (which is a literal). I don't know if it was a common idiom, but Python 3.9 [introduced](https://docs.python.org/3.9/library/stdtypes.html#str.removeprefix) the more readable alternatives, `str.removeprefix()` + `str.removesuffix()`. They have the advantage that they don't raise, if the string does not match the prefix / suffix.

This is related to [FURB188](https://docs.astral.sh/ruff/rules/slice-to-remove-prefix-or-suffix/) `slice-to-remove-prefix-or-suffix`.

**Source**
```
def path_to_qualified_name(path: str, project_dir: str) -> str:
    return path[len(project_dir):-len('.py')].replace('/', '.')
```

**Command**
`ruff check .  --select ALL --isolated --preview`


---

_Label `rule` added by @MichaReiser on 2024-09-19 07:28_

---

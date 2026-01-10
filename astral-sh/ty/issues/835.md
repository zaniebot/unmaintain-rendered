```yaml
number: 835
title: detect unused names (including unused because shadowed)
type: issue
state: open
author: karlicoss
labels:
  - feature
  - lint
assignees: []
created_at: 2025-07-16T23:33:53Z
updated_at: 2025-10-06T16:23:11Z
url: https://github.com/astral-sh/ty/issues/835
synced_at: 2026-01-10T02:06:24Z
```

# detect unused names (including unused because shadowed)

---

_Issue opened by @karlicoss on 2025-07-16 23:33_

### Summary

## What happens
If a function is declared twice, ty happily accepts it and infers the type as of the latest definition:

```py
def x() -> int:
    return 1

def x() -> str:
    return ""

reveal_type(x)
# Revealed type: `def x() -> str` (revealed-type) [Ln 7, Col 13]
```

ty playground link: https://play.ty.dev/9143957d-9d8d-4f20-b8c0-f6ce700d69f8

## Expected behaviour

Should probably complain (especially if the types are mismatching).

For the same code, mypy complains with [`no-redef` error](https://mypy.readthedocs.io/en/stable/error_code_list.html#check-that-each-name-is-defined-once-no-redef)

 https://mypy-play.net/?mypy=latest&python=3.12&gist=592caaea970d4463db57c27616547448


Apologies if this is already discussed somewhere, but couldn't find!

### Version

[03f35a4](https://github.com/astral-sh/ty/commit/03f35a4908bfe4d3ff201293beb3aa1fb1e52f05) -- latest git (playground)

---

_Label `lint` added by @carljm on 2025-07-16 23:38_

---

_Comment by @carljm on 2025-07-16 23:39_

I think we see this as more of a lint rule, not a core type checker feature. This code doesn't have any type errors in it.

I do think a "this function/class was defined and then shadowed without ever being used" lint rule would make sense to add on top of ty in the future.

---

_Label `feature` added by @carljm on 2025-07-16 23:39_

---

_Comment by @karlicoss on 2025-07-16 23:47_

Ah thanks! Out of interest, is there a special lint mode planned for ty? Or you mean it would be part of ruff?

---

_Comment by @carljm on 2025-07-16 23:56_

The details are still somewhat TBD, but in the long term we envision unifying ruff and ty in some way such that we can have lint rules powered by ty's type analysis. 

---

_Renamed from "ty doesn't detect same name defined multiple types (aka mypy no-redef)" to "detect unused names (including unused because shadowed)" by @carljm on 2025-07-17 03:45_

---

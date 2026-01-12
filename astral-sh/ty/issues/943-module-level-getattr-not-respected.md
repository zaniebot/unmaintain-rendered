```yaml
number: 943
title: Module level __getattr__ not respected
type: issue
state: closed
author: pelme
labels:
  - help wanted
  - runtime semantics
assignees: []
created_at: 2025-08-05T19:37:56Z
updated_at: 2025-08-08T17:39:39Z
url: https://github.com/astral-sh/ty/issues/943
synced_at: 2026-01-12T15:54:24Z
```

# Module level __getattr__ not respected

---

_@pelme_

### Summary

Defining a module level `__getattr__` is not picked up by ty: https://play.ty.dev/d17f33d6-87ec-4305-b390-9a15f9d88e7c


ty:
```
❯ ty check --output-format=concise main.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
main.py:1:23: error[unresolved-import] Module `upper` has no member `whatever`
main.py:3:1: warning[undefined-reveal] `reveal_type` used without importing it
main.py:3:13: info[revealed-type] Revealed type: `str`
main.py:4:1: warning[undefined-reveal] `reveal_type` used without importing it
main.py:4:13: info[revealed-type] Revealed type: `Unknown`
Found 5 diagnostics
```

mypy and pyright supports this:
```
❯ mypy main.py
main.py:3: note: Revealed type is "builtins.str"
main.py:4: note: Revealed type is "builtins.str"
Success: no issues found in 1 source file

❯ pyright main.py
/private/tmp/stuff.XZqSg2LO0/main.py
  /private/tmp/stuff.XZqSg2LO0/main.py:3:13 - information: Type of "hi" is "str"
  /private/tmp/stuff.XZqSg2LO0/main.py:4:13 - information: Type of "whatever" is "str"
0 errors, 0 warnings, 2 informations
```



### Version

ty 0.0.1-alpha.16 (c452e53a0 2025-07-25)

---

_Label `runtime semantics` added by @carljm on 2025-08-05 19:49_

---

_Added to milestone `GA` by @carljm on 2025-08-05 19:49_

---

_Label `help wanted` added by @carljm on 2025-08-05 19:49_

---

_Comment by @sharkdp on 2025-08-06 06:28_

If someone wants to work on this, this is a good place to start:

https://github.com/astral-sh/ruff/blob/18ad2848e3bb88ef7140b2e51de36312aa5981ab/crates/ty_python_semantic/src/types.rs#L3285-L3353

This functionality should probably be factored out into a function or moved to the top of the containing function, such that it can be used in [the `Type::ModuleLiteral` branch](https://github.com/astral-sh/ruff/blob/18ad2848e3bb88ef7140b2e51de36312aa5981ab/crates/ty_python_semantic/src/types.rs#L3247) as well.

Also note that there is a comment regarding a fake `__getattr__` on `types.ModuleType` which should probably not affect the functionality here, though... because `Type::NominalInstance(tyes.ModuleType)` is just the supertype of all module literals, not a module literal itself.

---

_Closed by @carljm on 2025-08-08 17:39_

---

```yaml
number: 16106
title: "feat/fix: Make `uv` work better with Nix"
type: issue
state: open
author: gabyx
labels:
  - enhancement
assignees: []
created_at: 2025-10-02T19:37:56Z
updated_at: 2025-10-02T19:42:23Z
url: https://github.com/astral-sh/uv/issues/16106
synced_at: 2026-01-12T16:02:24Z
```

# feat/fix: Make `uv` work better with Nix

---

_@gabyx_

### Summary

This is a rephrasal of the #15709 to explain the core underlying problem when using `uv` in a Nix environement.

**The culprit is that Nix python executables are sometimes wrapped.**

I try to explain in simple terms (as good as I can ;)

Wrapped means: The user calls `python-A`  which will call `python-raw`. 

- `python-A` is the interpreter (although wrapped) with which you want to build a virtual environment.
- `python-A` is a C compiled wrapper to set additional env. variables  **before calling `python-raw`** (which is (mostly) the pure interpreter)

> [!NOTE]
> 
> The `python-A` arises from a Nix expressions like `python3_13.withPackages { p: [ p.pyyaml ] }`.

What we want:

- Tell `uv` to really use `python-A` as the "base executable for virtual environments". 
  
The above can be tried with no success:

```shell
uv venv -vv --python="/nix/store/g0drgy172i4msg6vvkkm241djwam3x49-python3-3.13.7-env/bin/python"
```

> [!NOTE]
>
> `/nix/store/g0drgy172i4msg6vvkkm241djwam3x49-python3-3.13.7-env/bin/python` is the `python-A` executable.

```
DEBUG uv 0.8.14
DEBUG Found project root: `/persist/repos/dac-portal/tests/playwright`
DEBUG No workspace root found, using project root
DEBUG Using Python request `/nix/store/g0drgy172i4msg6vvkkm241djwam3x49-python3-3.13.7-env/bin/python` from explicit request
DEBUG Checking for Python interpreter at path `/nix/store/g0drgy172i4msg6vvkkm241djwam3x49-python3-3.13.7-env/bin/python`
TRACE Querying interpreter executable at /nix/store/g0drgy172i4msg6vvkkm241djwam3x49-python3-3.13.7-env/bin/python
Using CPython 3.13.7 interpreter at: /nix/store/g0drgy172i4msg6vvkkm241djwam3x49-python3-3.13.7-env/bin/python3.13
Creating virtual environment at: .devenv/state/venv

DEBUG Using base executable for virtual environment: /nix/store/829wb290i87wngxlh404klwxql5v18p4-python3-3.13.7/bin/python
```

**which uses now `/nix/store/829wb290i87wngxlh404klwxql5v18p4-python3-3.13.7/bin/python`**

This is due to:

1. `python-raw` beeing called by `get_interpreter_info.py` which reports `sys_base_executbale = python-raw`.

**The question to astral is now how can `uv` still call `get_interpreter_info.py`  (cause you rely on this for other attributes etc) 
but also give Nix a workaround to overwrite the base executable?**


---

_Label `enhancement` added by @gabyx on 2025-10-02 19:37_

---

_Renamed from "Make `uv` work better with Nix" to "feat/fix: Make `uv` work better with Nix" by @gabyx on 2025-10-02 19:42_

---

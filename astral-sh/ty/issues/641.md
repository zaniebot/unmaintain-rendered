```yaml
number: 641
title: TY hanging when checking files
type: issue
state: closed
author: jad-haddad
labels:
  - hang
assignees: []
created_at: 2025-06-12T09:50:10Z
updated_at: 2025-06-13T19:50:58Z
url: https://github.com/astral-sh/ty/issues/641
synced_at: 2026-01-10T02:08:20Z
```

# TY hanging when checking files

---

_Issue opened by @jad-haddad on 2025-06-12 09:50_

### Summary

Hello,
I'm running `ty check` on my repo and it reaches a point where it hangs and nothing happens anymore
```
Checking ------------------------------------------------------------ 221/227 files
```
and I tried waiting for 5 min, and still nothing, happened. I ran the command with verbose

```
  2025-06-12T09:40:00.033674Z DEBUG ty_python_semantic::module_resolver::resolver: Module `pyarrow` not found in search paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:43 on ThreadId(3)
    in ty_project::check_file with file=file(Id(c04))
    in ty_project::Project::check

  2025-06-12T09:40:00.046298Z DEBUG ty_python_semantic::module_resolver::resolver: Module `pymupdf._mupdf` not found in search paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:43 on ThreadId(8)
    in ty_project::check_file with file=file(Id(c1d))
    in ty_project::Project::check

  2025-06-12T09:40:00.046354Z DEBUG ty_python_semantic::module_resolver::resolver: Module `_mupdf` not found in search paths
    at crates/ty_python_semantic/src/module_resolver/resolver.rs:43 on ThreadId(8)
    in ty_project::check_file with file=file(Id(c1d))
    in ty_project::Project::check

Checking ------------------------------------------------------------ 221/227 files
```

as for the settings, I don't have anything specified, so it is the default settings.

### Version

ty 0.0.1-alpha.9 (2feba5eb2 2025-06-11)

---

_Label `hang` added by @AlexWaygood on 2025-06-12 09:53_

---

_Comment by @AlexWaygood on 2025-06-12 09:54_

Are you using pandas by any chance? If so, this might be the same issue as https://github.com/astral-sh/ty/issues/627

---

_Comment by @jad-haddad on 2025-06-12 10:02_

I do have some modules that use pandas yes (and ty worked fine checking them in previous alpha versions)

---

_Comment by @flockoftravi on 2025-06-12 17:02_

I am seeing this issue as well. It seems specific to 0.0.1a9 in my case, as reverting back to 0.0.1a8 resolves the hang.

---

_Comment by @pratt-fds on 2025-06-13 09:40_

When the hang occurs, there is a significant memory leak. Version 0.0.1a8 does not have this error on my system

Ubuntu 24.04 running inside WSL2 on a Win11 system

![Image](https://github.com/user-attachments/assets/3f1c0998-a124-47e3-bc6e-e048752b9b07)

---

_Comment by @sharkdp on 2025-06-13 09:47_

> When the hang occurs, there is a significant memory

Interesting, yes. I observe the same thing on my [MRE here](https://github.com/astral-sh/ty/issues/627#issuecomment-2969761630):

![Image](https://github.com/user-attachments/assets/4b643e17-3095-4960-bc91-4b82ff2bddaf)

---

_Closed by @carljm on 2025-06-13 19:50_

---

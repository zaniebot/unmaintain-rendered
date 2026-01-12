```yaml
number: 2273
title: "Auto Import: Add support for namespace packages "
type: issue
state: open
author: Zhmjharmgal
labels:
  - server
  - imports
  - completions
assignees: []
created_at: 2025-12-30T09:24:44Z
updated_at: 2026-01-06T14:20:18Z
url: https://github.com/astral-sh/ty/issues/2273
synced_at: 2026-01-12T15:54:26Z
```

# Auto Import: Add support for namespace packages 

---

_@Zhmjharmgal_

### Summary

Hello,

Thanks for the great work. ty is indeed much faster than pyright and basedpyright. However, I found that it cannot auto-import any functions defined in workspace python files. Below is a minimal reproduction:

My root directory is `test` and below is the directory structure:

<img width="895" height="355" alt="Image" src="https://github.com/user-attachments/assets/2bc15a54-1e32-46b2-a096-a3752738c493" />

Inside `src/path2/file2.py` I have a dummy function defined as 

<img width="1150" height="272" alt="Image" src="https://github.com/user-attachments/assets/5247ef21-1e8c-4d7c-b0dd-7bd08f8400ad" />

Then, in `src/path1/file1.py`, when I start typing my_custom<CURSOR>, `my_custom_function` is not an auto-completion entry from ty:

<img width="493" height="124" alt="Image" src="https://github.com/user-attachments/assets/8be545f7-30a5-4a66-9839-f8e28676893a" />

Further, after I completely typed my_custom_function there and invoked code actions, no auto-import entry is displayed, only "ignore 'unresolved-reference' for this line [ty]":

<img width="1404" height="312" alt="Image" src="https://github.com/user-attachments/assets/9edf2d5c-5675-4011-9d7d-f5c30576cc65" />

However, after I started the lsp basedpyright, `my_cumtom_function` does become available as an auto-completion suggestion:

<img width="816" height="139" alt="Image" src="https://github.com/user-attachments/assets/60781664-a7d1-4a85-8f40-6a29930a8789" />

So, I would like to ask if this is a bug or an intentional plan. Thank you!

---

## Context:

- I use Arch Linux.
- I used `uv init test` to init this python project and used `uv tool install ty` to install ty. I don't have any other modifications on `pyproject.toml`.
- I used `export PYTHONPATH=$PWD` to explicitly set the root directory in `test` (parent directory of `src`)
- The ty version is 0.0.8.
- I tested this issue on both neovim and vscode and the behavior is the same.



### Version

ty 0.0.8

---

_Renamed from "ty fails to auto-import methods defined in workspace" to "Auto Import: Add support for namespace packages " by @MichaReiser on 2025-12-30 10:00_

---

_Comment by @MichaReiser on 2025-12-30 10:02_

Thanks for the report. I can reproduce this in the playground. See how ty offers completions (and auto-import) for the `my_custom_function` function in `path1` (a regular python module with an __init__.py) but it fails to suggest the function defined in `namespace`

https://play.ty.dev/a04dc430-8b7c-4854-9951-0818a1553ae3

CC: @BurntSushi 

---

_Label `completions` added by @MichaReiser on 2025-12-30 10:02_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-30 10:02_

---

_Label `server` added by @AlexWaygood on 2025-12-30 10:05_

---

_Comment by @BurntSushi on 2026-01-05 15:42_

This is "intentional" in the sense that we are intentionally not handling this yet in the code path for listing all modules.

We specifically do not handle namespace packages when listing submodules:

https://github.com/astral-sh/ruff/blob/f3dea6e5c911b2b34dfb00d3d8696a5af66bb732/crates/ty_module_resolver/src/module.rs#L151-L158

Which is used when asking for all modules in an environment:

https://github.com/astral-sh/ruff/blob/f3dea6e5c911b2b34dfb00d3d8696a5af66bb732/crates/ty_module_resolver/src/list.rs#L11-L23

Which is in turn what we use as a starting point for auto-import:

https://github.com/astral-sh/ruff/blob/f3dea6e5c911b2b34dfb00d3d8696a5af66bb732/crates/ty_ide/src/all_symbols.rs#L33

So I think we need to figure out a way to make `Module::all_submodules` work for namespace packages. I think this requires checking all search paths for a directory at the location where part of a namespace package _could_ exist.

If we do that, then I _think_ this will just work. The "list modules" implementation already has namespace package support. It just doesn't know how to list the submodules of a namespace package.

---

_Label `imports` added by @BurntSushi on 2026-01-05 15:43_

---

_Assigned to @Gankra by @Gankra on 2026-01-05 15:44_

---

_Comment by @Zhmjharmgal on 2026-01-06 10:47_

Thank you for your detailed explanation. I now understand why namespace packages are not handled.

Previously, I relied heavily on namespace packages because pyright supports them, and my Python projects were primarily for research implementation purposes. I simply wasn’t motivated to explore Python’s "module/package" conventions at the time.

However, I decided to switch to the uv + ty ecosystem last week for two main reasons: (i) the pip/conda package management system failed me when attempting to reproduce my development environment on another machine with a different architecture, and (ii) pyright has been increasingly slow.

Reflecting on it now, I suspect my reliance on namespace packages might have contributed to pyright’s performance issues. If the exclusion of namespace packages in ty is a deliberate design choice to maintain performance, I fully understand and support that decision. Speed is indeed a crucial factor for me, and I’m okay with closing this issue if enabling namespace packages would compromise performance.

That said, it would be helpful if this design decision is mentioned in the documentation. Highlighting it could encourage users like me to adopt Python’s standard module/package conventions.

Thank you again for this incredible tooling!

---

_Comment by @BurntSushi on 2026-01-06 14:20_

No, I think this is something we should fix.

The good news is that namespace package support should only make listing submodules more expensive specifically when a namespace package is used and not make other cases slow. That is, when we have a `Module` internally, we know whether it's a namespace package or not. But yeah, I could for sure see things being slower for namespace packages compared to regular packages.

I would generally recommend not using namespace packages if you don't specifically need them though. For your case, you might just need to drop an `__init__.py` in your directories.

---

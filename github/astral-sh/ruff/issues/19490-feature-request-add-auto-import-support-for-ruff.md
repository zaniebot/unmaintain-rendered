---
number: 19490
title: "[Feature Request] Add auto-import support for Ruff LSP"
type: issue
state: closed
author: seamuswn
labels: []
assignees: []
created_at: 2025-07-22T15:14:24Z
updated_at: 2025-07-22T16:24:37Z
url: https://github.com/astral-sh/ruff/issues/19490
synced_at: 2026-01-07T13:12:16-06:00
---

# [Feature Request] Add auto-import support for Ruff LSP

---

_Issue opened by @seamuswn on 2025-07-22 15:14_

### Summary

### What 

Supporting the detection, selection and addition of missing imports is a potential fix for the following lint rules:
https://docs.astral.sh/ruff/rules/undefined-name/
https://docs.astral.sh/ruff/rules/undefined-local-with-import-star-usage/

Essentially re-opening this feature request from 2023 which was closed at that time:
https://github.com/astral-sh/ruff/issues/4059

### Why am I asking

With the current lack of Pylance support for non-VsCode Vscodium projects (e.g. Cursor), there's no real viable option for an LSP offering this code action. BasedPyright is supposed to support this but has [a significant limitation](https://github.com/DetachHead/basedpyright/issues/545) in that it can only detect and suggest auto-imports for files that have previously been opened within the same IDE session (so in order to be able to suggest `from src.foo import bar`, `src/foo.py` must have been opened already).

If Ruff were to support this, it would significantly improve the developer experience for folks looking for open source LSPs that would support this pretty rudimentary code action, that kind of falls between the cracks in where we expect this to be supported, as to my knowledge the only real viable alternative is [rope](https://github.com/python-rope/rope).

### Reference Implementation: Pycharm

For reference of how this might work, I think the Pycharm IDE actions implementation works pretty nicely (see docs [here](https://www.jetbrains.com/help/pycharm/creating-and-optimizing-imports.html#import-packages)):
1. It has a code action for `Import this name` or `Import this name locally`. The former option will add an import statement at the top of the module, the latter will insert the import statement on the line immediately preceding the reference, at the same level of indentation.
2. If there's only a single option found for the name, that name is inlined in the Code Action:
<img width="488" height="87" alt="Image" src="https://github.com/user-attachments/assets/85d9f921-b38e-45fe-835e-4ab2ecfd6019" />

3. If there are multiple options found for the name, it displays `Import this name`, which then shows a dropdown of options when selected:

<img width="232" height="82" alt="Image" src="https://github.com/user-attachments/assets/c4fd0b1e-d79c-4049-a3fe-1c3d59861f74" />

<img width="382" height="136" alt="Image" src="https://github.com/user-attachments/assets/9bb011f0-c1c4-47b9-960a-c104e60dce29" />

---

_Comment by @ntBre on 2025-07-22 16:24_

I believe we're working on this in our upcoming type-checker and LSP ty: https://github.com/astral-sh/ty/issues/535. ty has the necessary infrastructure for module resolution and multi-file analysis that isn't currently integrated into Ruff.

---

_Closed by @ntBre on 2025-07-22 16:24_

---

```yaml
number: 15990
title: "[red-knot] Fix relative imports in `src.root`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha/fix-relative-imports-in-src-root
created_at: 2025-02-06T09:08:24Z
updated_at: 2025-02-06T17:56:08Z
url: https://github.com/astral-sh/ruff/pull/15990
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Fix relative imports in `src.root`

---

_Pull request opened by @MichaReiser on 2025-02-06 09:08_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15989

Red Knot failed to resolve relative imports if the importing module is located at a search path root. 

The issue was that the module resolver returned an `Err(TooManyDots)` as soon as the parent of the current module is `None` (which is the case for a module at the search path root). 
However, this is incorrect if there's a `tail` (a module name). 

## Test Plan

Added mdtests

---

_Label `bug` added by @MichaReiser on 2025-02-06 09:25_

---

_Label `red-knot` added by @MichaReiser on 2025-02-06 09:25_

---

_Marked ready for review by @MichaReiser on 2025-02-06 09:25_

---

_Review requested from @carljm by @MichaReiser on 2025-02-06 09:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-06 09:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-06 09:25_

---

_@AlexWaygood reviewed on 2025-02-06 13:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:2 on 2025-02-06 13:52_

not sure what this is doing here :P

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2522 on 2025-02-06 14:00_

We might be eagerly creating a `ModuleName` from the `tail` here (which might be expensive) in some unnecessary situations... this could be a lazy closure rather than an eager assignment? But what you have is also probably fine

---

_@AlexWaygood approved on 2025-02-06 14:00_

Nice -- well diagnosed!

---

_@MichaReiser reviewed on 2025-02-06 14:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:2 on 2025-02-06 14:01_

lol, Pycharm tried to be clever

---

_@MichaReiser reviewed on 2025-02-06 14:02_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2522 on 2025-02-06 14:02_

The only case where we unnecessarily create a `ModuleName` here is if the import is invalid. Considering that this should (hopefully) be the exception, allocating should be fine.

---

_@AlexWaygood reviewed on 2025-02-06 14:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2522 on 2025-02-06 14:03_

Ah right, because the `extend` call at the bottom of the function requires that the argument passed in has to already be a `ModuleName` instance. Sounds good.

---

_@MichaReiser reviewed on 2025-02-06 14:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:2 on 2025-02-06 14:04_

Thanks for catching this

---

_Merged by @MichaReiser on 2025-02-06 14:08_

---

_Closed by @MichaReiser on 2025-02-06 14:08_

---

_Branch deleted on 2025-02-06 14:08_

---

_Comment by @carljm on 2025-02-06 17:44_

I think this change is not correct? Python does not actually allow this at runtime. It seems like a bug in pyright if it allows it.

If I create these two files:

`lib.py`:

```py
x = 1
```

`main.py`:

```py
from .lib import x
```

The result of `python3 -c 'import main'` is `ImportError: attempted relative import with no known parent package`

The tomllib modules actually exist within a `tomllib` package, so the relative imports are fine; they are not meant to be top-level modules.

---

_Comment by @MichaReiser on 2025-02-06 17:56_

Hmm true. I'll revert tomorrow and add an explicit test for it. I didn't consider testing it, I trusted Pyright but it seems I shouldn't have.

---

```yaml
number: 1355
title: possibly-missing-attribute warnings with asyncio on python 3.14
type: issue
state: closed
author: oliverlambson
labels:
  - bug
  - imports
  - control flow
assignees: []
created_at: 2025-10-14T19:55:20Z
updated_at: 2025-12-03T16:52:33Z
url: https://github.com/astral-sh/ty/issues/1355
synced_at: 2026-01-12T15:54:25Z
```

# possibly-missing-attribute warnings with asyncio on python 3.14

---

_@oliverlambson_

### Summary

`asyncio.timeout and asyncio.TaskGroup` both flag `possibly-missing-attribute` when targeting python 3.14, but I don't believe they should for anything >=3.11

```python
# ex.py
import asyncio


async def main() -> None:
    async with asyncio.TaskGroup():
        ...
    async with asyncio.timeout(1):
        await asyncio.sleep(1)
```

```
$ uv run ty check --python-version 3.13 ex.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                          All checks passed!

$ uv run ty check --python-version 3.14 ex.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                          warning[possibly-missing-attribute]: Attribute `TaskGroup` on type `<module 'asyncio'>` may be missing
 --> ex.py:5:16
  |
4 | async def main() -> None:
5 |     async with asyncio.TaskGroup():
  |                ^^^^^^^^^^^^^^^^^
6 |         ...
7 |     async with asyncio.timeout(1):
  |
info: rule `possibly-missing-attribute` is enabled by default

warning[possibly-missing-attribute]: Attribute `timeout` on type `<module 'asyncio'>` may be missing
 --> ex.py:7:16
  |
5 |     async with asyncio.TaskGroup():
6 |         ...
7 |     async with asyncio.timeout(1):
  |                ^^^^^^^^^^^^^^^
8 |         await asyncio.sleep(1)
  |
info: rule `possibly-missing-attribute` is enabled by default

Found 2 diagnostics
```

### Version

ty 0.0.1-alpha.22 (2f3190d09 2025-10-10)

---

_Label `bug` added by @sharkdp on 2025-10-15 06:55_

---

_Label `control flow` added by @sharkdp on 2025-10-15 06:55_

---

_Comment by @sharkdp on 2025-10-15 08:16_

Thank you for reporting this. I managed to find a MRE:

https://play.ty.dev/c3a85c2a-0617-4fd7-a0fb-f758508a9ac1

It looks to me like the problem is that the conditional `from .graph import *` star import (mistakenly) imports `__all__` from `asyncio.graph` (??), which seems to affect the inference of `__all__` for `asyncio`.

---

_Comment by @AlexWaygood on 2025-10-15 18:41_

it looks like there might be at least two bugs here (exciting!):

- If you have a file `a.py` with `__all__ = ["foo"]` and a file `b.py` with `from a import *`, that shouldn't lead to an `__all__` symbol being considered to be defined in `b.py`. `a.py` would need to have `__all__ = ["foo", "__all__"]` for the `from a import *` statement to import `__all__` from `a.py` as well as `foo` from `a.py`
- Even if you accept the (incorrect) premise that the `from .graph import *` import should lead to `asyncio.graph.__all__` being imported into `asyncio/__init__.pyi`, our dunder-all inference machinery _should_ detect that earlier definition of `__all__` as being entirely shadowed by the `__all__` definition later on in the module [here](https://github.com/python/typeshed/blob/11c7821a79a8ab7e1982f3ab506db16f1c4a22a9/stdlib/asyncio/__init__.pyi#L36-L995)

---

_Comment by @jfaldanam on 2025-10-24 14:45_

Also found the issue on `ty 0.0.1-alpha.24`, nice to know that targeting 3.13 can serve as a workaround for now!

---

_Label `imports` added by @carljm on 2025-10-30 16:09_

---

_Comment by @sghng on 2025-11-16 00:44_

I updated ty to 0.0.1a26, seems like it has been fixed. At least `asyncio.TaskGroup` can be detected now.

EDIT: this is on macOS Tahoe

---

_Comment by @sharkdp on 2025-11-17 12:24_

@Gankra Is it plausible that this was fixed by one of your PRs? And if so, *should* it have been fixed by your PRs, or was that unexpected?

---

_Added to milestone `Stable` by @carljm on 2025-11-17 15:26_

---

_Comment by @refi64 on 2025-11-23 23:46_

as an extra data point, I can still observe this on ty 0.0.1a26, a27, and `e642874cf` targeting python 3.14: https://play.ty.dev/6286cc96-37b9-4fe1-a351-490c0569147f for the exact same reproducer as the original comment.

so I don't think it's (incidentally or intentionally) fixed yet.

---

_Comment by @zsol on 2025-11-27 12:09_

Here's a slightly [more minimal mre](https://play.ty.dev/1854205a-5352-4c42-b436-6a0d3abd1792):
* all of `taskgroups.pyi` was removed, and the import star replaced with an inline class declaration
* the `__all__` assignment in `my_asyncio/__init__.py` was removed

A few things I noticed that might be clues for someone more knowledgeable about ty internals:
1. Making the `sys.version_info` conditionals mutually exclusive makes the warning go away
2. Copying over `class TaskGroup: ...` into the first conditional (for 3.14) makes the warning go away
3. In `my_asyncio/__init__.py` if I replace `import sys` with `import sys as sys2` (and all references accordingly), the warning goes away
4. Similarly replacing `import sys` in `graph.pyi` also makes the warning go away
5. If the type of `__all__` in `graph.pyi` is not a tuple of string literals, then `__all__ = ()` will get a funky type error in `my_asyncio/__init__.py` [like so](https://play.ty.dev/298691e1-a307-4f60-afba-db360ce5b3f4) 

---

_Removed from milestone `Stable` by @AlexWaygood on 2025-11-28 18:42_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-11-28 18:42_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-11-28 18:42_

---

_Comment by @AlexWaygood on 2025-12-01 21:43_

Using a [slightly modified version of @zsol's repro](https://play.ty.dev/89ee7408-9b97-4333-a36f-1cb8b161c6cc) (altered so that the `sys.version_info` branch uses Python 3.13 rather than 3.14), I did a `git bisect` to find which commit caused this bug. I ran `cargo run --manifest-path ../ruff/Cargo.toml -p ty check  main.py --python-version=3.13` using each commit `git bisect` gave me, from a directory that contained the modified repro:

```
7d468ee58aee6a476ec1a0b54275210c9add8622 is the first bad commit
commit 7d468ee58aee6a476ec1a0b54275210c9add8622
Author: David Peter <sharkdp@users.noreply.github.com>
Date:   Tue Jul 1 11:06:37 2025 +0200

    [ty] Model reachability of star import definitions for nonlocal lookups (#19066)
    
    ## Summary
    
    Temporarily modify `UseDefMapBuilder::reachability` for star imports in
    order for new definitions to pick up the right reachability. This was
    already working for `UseDefMapBuilder::place_states`, but not for
    `UseDefMapBuilder::reachable_definitions`.
    
    closes https://github.com/astral-sh/ty/issues/728
    
    ## Test Plan
    
    Regression test

 .../resources/mdtest/import/star.md                | 30 ++++++++++++++--------
 .../resources/mdtest/terminal_statements.md        |  6 ++---
 .../src/semantic_index/builder.rs                  | 25 +++++++++++++++++-
 .../src/semantic_index/use_def.rs                  |  7 ++---
 4 files changed, 48 insertions(+), 20 deletions(-)
```

which points to https://github.com/astral-sh/ruff/commit/7d468ee58aee6a476ec1a0b54275210c9add8622. And indeed, `uvx ty@0.0.1a13 check main.py --python-version=3.13` on the modified repro exhibits the false-positive error, but the same command with `uvx ty@0.0.1a12` does not. 

---

_Unassigned @AlexWaygood by @carljm on 2025-12-02 18:54_

---

_Assigned to @sharkdp by @sharkdp on 2025-12-03 12:52_

---

_Closed by @sharkdp on 2025-12-03 13:51_

---

_Reopened by @sharkdp on 2025-12-03 13:51_

---

_Comment by @sharkdp on 2025-12-03 14:26_

Thank you both for your analysis here. Especially the *"Similarly replacing import sys in graph.pyi also makes the warning go away"* hint was very helpful.

Let's look at the relevant part of the code again (from `asyncio/__init__.pyi`):
```py
import sys

if sys.version_info >= (3, 14):
    from .graph import *
if sys.version_info >= (3, 11):
    class TaskGroup: ...
```
What's going on is the following: When we see the `from .graph import *` star import, we don't know yet which symbols from `.graph` will be available later on (because we might later find out that their definitions are unreachable, or that they might not be re-exported via `__all__`). For this reason, in the semantic index builder, we pretend that *all* of the symbols that we can see in `.graph` are imported, but we guard each of these import-definitions with a special star-import reachability constraint. One of the symbols in `graph` is `sys`! We will later find out that it's not actually re-exported (because it's not listed in `__all__`), but we do create a definition, just in case it might be. The star-import basically creates a structure like the following (where `symbol_is_reexported_and_visible` is a made-up function that we can later evaluate in type checking):
```py
import sys

if sys.version_info >= (3, 14):
    if symbol_is_reexported_and_visible(".graph", "sys"):
        from .graph import sys
    if symbol_is_reexported_and_visible(".graph", "FutureCallGraph"):
        from .graph import FutureCallGraph
    # <more of these, one for every visible symbol in .graph>

if sys.version_info >= (3, 11):
    class TaskGroup: ...
```
So if we are on 3.14, when we later evaluate the *second* `sys.version_info >= (3, 11)` condition, we see *two* live bindings of `sys`. All should be good though, because once we evaluate `symbol_is_reexported_and_visible(".graph", "sys")` (to `AlwaysFalse`), we see that only the first definition is actually reachable.

And indeed, if we check the type of `sys` after that star-import, it's still `<module 'sys'>` as it should be. However, if we check the type of `sys.version_info`, we find that it has a type of `Never`! [Here's an even smaller MRE](https://play.ty.dev/cf6ac260-2932-496b-8d68-c4b8559eb0e0) that demonstrates this.

This has to do with another mechanism that the semantic index builder helps with: attribute-narrowing. When we narrow the type of an attribute (`if obj.attr is not None`), and then later see an assignment to `obj`, we [invalidate the narrowing](https://play.ty.dev/e90d5778-d90d-43bc-9f90-00df4f780632). To do this, the semantic index builder keeps track of relationships between members (`obj.attr`) and corresponding symbols (`obj`). When it sees a binding to `obj`, it records a `DefinitionState::Deleted` binding for the corresponding member. I'm pretty sure that this is the root cause here. We probably do not keep track of the state of the `sys.version_info` member in *both* control flow branches (the one where `symbol_is_reexported_and_visible(".graph", "sys")` is true, and the one where it's false), due to the way we model these special star-import pseudo branches (which itself is necessary for performance reasons). Fixing this will require us to not just model the state and the merging of symbols-states, but also the state and merging of their associated member-states.

---

_Closed by @sharkdp on 2025-12-03 16:52_

---

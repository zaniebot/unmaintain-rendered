---
number: 11653
title: "[red-knot] PEP 561 compliant module resolver"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-05-31T21:48:51Z
updated_at: 2025-03-10T23:18:54Z
url: https://github.com/astral-sh/ruff/issues/11653
synced_at: 2026-01-10T01:22:51Z
---

# [red-knot] PEP 561 compliant module resolver

---

_Issue opened by @carljm on 2024-05-31 21:48_

(Or intentional choices to diverge from PEP 561 compliance.)

Subtasks:
- [x] Vendor typeshed's stubs: https://github.com/astral-sh/ruff/pull/11340
- [x] Add automation for keeping typeshed's stubs up to date: https://github.com/astral-sh/ruff/pull/11427
- [x] Implement `--custom-typeshed-dir` internally: https://github.com/astral-sh/ruff/pull/11767
- [x] Implement the typing spec's [module resolution order](https://typing.readthedocs.io/en/latest/spec/distributing.html#import-resolution-ordering):
  - https://github.com/astral-sh/ruff/pull/11767
  - Proposal for tweaking the spec posted at https://discuss.python.org/t/pep-561-s-module-resolution-order-seems-incorrect/55048/1
- [x] Make vendored typeshed stubs available from the built Ruff binary: https://github.com/astral-sh/ruff/pull/11779
- [x] Use vendored typeshed stubs at runtime to resolve symbols to stdlib definitions if `--custom-typeshed-dir` is not passed
  - [x] Add a `VendoredFileSystem` implementation: https://github.com/astral-sh/ruff/pull/11863
  - [x] Use the `VendoredFileSystem` implementation to resolve stdlib stubs: astral-sh/ruff#12224
- [ ] Add support for stubs packages suffixed with `-stubs` installed into `site-packages` (must take priority over other packages installed into `site-packages`)
- [x] Add support for typeshed's `VERSIONS` file: we shouldn't fallback to a typeshed module if the `VERSIONS` file specifies it does not exist with the `--target-version` the user passes.
  - [x] Add a parser for `VERSIONS`: https://github.com/astral-sh/ruff/pull/11836
  - [x] Use the `VERSIONS` file when resolving stdlib modules: https://github.com/astral-sh/ruff/pull/12141
- [x] Add support for editable installs that use static `._pth` files:
  - [x] https://github.com/astral-sh/ruff/pull/12289
  - [x] https://github.com/astral-sh/ruff/pull/12307
- [x] Fix astral-sh/ruff#12278
- [ ] Add understanding of `py.typed` files with `partial` in them, which are used to explicitly mark a package's annotations as partially complete
- [ ] Audit mypy's and pyright's module resolvers to see if there's anything that's not in the spec that still needs to be implemented

---

_Label `red-knot` added by @carljm on 2024-05-31 21:48_

---

_Assigned to @AlexWaygood by @carljm on 2024-05-31 21:48_

---

_Renamed from "PEP 561 compliant module resolver" to "[red-knot] PEP 561 compliant module resolver" by @carljm on 2024-05-31 21:56_

---

_Referenced in [astral-sh/ruff#11767](../../astral-sh/ruff/pulls/11767.md) on 2024-06-06 13:13_

---

_Referenced in [astral-sh/ruff#11779](../../astral-sh/ruff/pulls/11779.md) on 2024-06-06 19:55_

---

_Comment by @MichaReiser on 2024-06-08 15:28_

One thing that we need to implement as part of this is a vendored filesystem. It's not a functionality but a task to complete on our way to get there.

---

_Comment by @AlexWaygood on 2024-06-08 15:36_

> One thing that we need to implement as part of this is a vendored filesystem. It's not a functionality but a task to complete on our way to get there.

Right. I was considering that part of

>  Use vendored typeshed stubs at runtime to resolve symbols to stdlib definitions if --custom-typeshed-dir is not passed

since, as you say, it's necessary to implement a vendored filesystem in order to be able to do that

---

_Referenced in [astral-sh/ruff#11836](../../astral-sh/ruff/pulls/11836.md) on 2024-06-12 11:03_

---

_Referenced in [astral-sh/ruff#11863](../../astral-sh/ruff/pulls/11863.md) on 2024-06-13 19:16_

---

_Referenced in [astral-sh/ruff#12224](../../astral-sh/ruff/pulls/12224.md) on 2024-07-07 22:01_

---

_Referenced in [astral-sh/ruff#12307](../../astral-sh/ruff/pulls/12307.md) on 2024-07-12 19:51_

---

_Referenced in [ami-iit/jaxsim#190](../../ami-iit/jaxsim/issues/190.md) on 2024-08-26 12:44_

---

_Comment by @MichaReiser on 2024-10-30 08:26_

@AlexWaygood is there more work that needs doing or can we consider this done?

---

_Comment by @AlexWaygood on 2024-10-30 08:35_

We can't call ourself PEP-561-compliant until we do this, which is still outstanding, and important. It's not huge, but also not trivial:

>     * [ ]  Add support for stubs packages suffixed with `-stubs` installed into `site-packages

But we decided to defer work on it for now because the type-inference work is more important.

We should also add support for `py.typed` files that explicitly mark type annotations as `partial`, which is specified by the PEP but not listed in the tasks above; I'll add that now.

---

_Referenced in [astral-sh/ruff#14007](../../astral-sh/ruff/issues/14007.md) on 2024-11-03 06:28_

---

_Referenced in [astral-sh/ruff#15697](../../astral-sh/ruff/issues/15697.md) on 2025-03-07 13:37_

---

_Comment by @carljm on 2025-03-10 23:18_

Finer-grained issues are easier to manage for prioritizing and tracking work to be done, so I've created https://github.com/astral-sh/ruff/issues/16612 and https://github.com/astral-sh/ty/issues/184 for the last two items here.

I don't think "audit mypy and pyright resolvers" is likely worth the trouble, it will be more efficient to discover issues from encountering them in the wild. If someone disagrees, feel free to create an issue for it! (Or just do it and create issues for anything observed.)

---

_Closed by @carljm on 2025-03-10 23:18_

---

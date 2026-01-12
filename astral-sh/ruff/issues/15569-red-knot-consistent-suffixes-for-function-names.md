```yaml
number: 15569
title: "[red-knot] Consistent suffixes for function names"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2025-01-18T12:32:42Z
updated_at: 2025-01-22T08:06:58Z
url: https://github.com/astral-sh/ruff/issues/15569
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Consistent suffixes for function names

---

_@sharkdp_

We previously had a scheme where functions that had a `_ty` suffix would return a `Type<'db>`. This has been broken in a few places, for example:

- `bindings_ty` which returns a `Symbol<'db>`
- `declaration_ty` which returns a `TypeAndQualifiers<'db>`

We should probably clean this up, i.e. either come up with new suffixes, with entirely new function names, or decide that we don't want to do anything about it.

---

_Label `red-knot` added by @sharkdp on 2025-01-18 12:32_

---

_Assigned to @sharkdp by @sharkdp on 2025-01-18 12:32_

---

_Comment by @AlexWaygood on 2025-01-19 12:53_

Could we maybe just remove the suffixes? I'm not sure they add much in a Rust codebase, since all functions have return-type annotations anyway.

I also think it's non-obvious to newcomers to the codebase that `ty` means "type". It took me a while to get used to it and I still don't really love the use of the abbreviation everywhere. (In some cases it's obviously necessary since `type` is a reserved keyword.)

---

_Comment by @MichaReiser on 2025-01-19 12:56_

I do think the suffixes are useful here or we have to come up with new names entirely. E.g. `bindings` suggests that it returns an iterator, `bindings_ty` makes it clear it doesn't. 

---

_Comment by @sharkdp on 2025-01-20 14:07_

I opened a https://github.com/astral-sh/ruff/pull/15618 as a proposal on how to rename `bindings_ty` and `declarations_ty`, the two functions for which the inconsistency is most apparent.

> I also think it's non-obvious to newcomers to the codebase that `ty` means "type". It took me a while to get used to it and I still don't really love the use of the abbreviation everywhere. (In some cases it's obviously necessary since `type` is a reserved keyword.)

I opened another draft which renames all `*_ty` functions to `*_type`, just to see how it looks like: https://github.com/astral-sh/ruff/pull/15617. This builds on top of #15618. I personally don't have a preference in either direction. If we rename those suffixes from `_ty` to `_type`, we should consider doing something similar for variables as well?

---

_Closed by @sharkdp on 2025-01-22 08:06_

---

_Closed by @sharkdp on 2025-01-22 08:06_

---

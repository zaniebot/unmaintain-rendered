---
number: 2272
title: Hover on function calls reveals Never in unreachable code
type: issue
state: open
author: hauntsaninja
labels:
  - server
  - unreachable-code
assignees: []
created_at: 2025-12-30T08:55:01Z
updated_at: 2025-12-31T15:46:41Z
url: https://github.com/astral-sh/ty/issues/2272
synced_at: 2026-01-10T01:51:14Z
---

# Hover on function calls reveals Never in unreachable code

---

_Issue opened by @hauntsaninja on 2025-12-30 08:55_

### Summary

If you hover on `bar` in the following, you get `Never`

https://play.ty.dev/61a6752c-3844-4494-9ad5-367de301f768

This is totally reasonable behaviour from a type checker, but it's not as ideal for an LSP. It's pretty common to temporarily have unreachable code while refactoring and want to be able to see types — at least types of globals, like other functions

### Version

_No response_

---

_Comment by @mtshiba on 2025-12-30 09:22_

One possible solution to this issue is to have the `Never` type hold a context (the type that the variable would have if it were reachable).

---

_Label `unreachable-code` added by @mtshiba on 2025-12-30 09:23_

---

_Label `server` added by @AlexWaygood on 2025-12-30 10:07_

---

_Comment by @carljm on 2025-12-30 21:41_

> One possible solution to this issue is to have the `Never` type hold a context (the type that the variable would have if it were reachable).

This also would require adding some nuance to our understanding of reachability -- currently we don't really distinguish between an unreachable use and an unreachable definition, we just care whether there is a path from a definition to a use. See #2281 also.

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:46_

---

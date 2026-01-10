```yaml
number: 1294
title: "Completions show function-local symbols with a type of `Never`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-10-01T14:13:00Z
updated_at: 2025-12-11T13:26:17Z
url: https://github.com/astral-sh/ty/issues/1294
synced_at: 2026-01-10T01:56:40Z
```

# Completions show function-local symbols with a type of `Never`

---

_Issue opened by @sharkdp on 2025-10-01 14:13_

Completions show function-local symbols with a type of `Never` if the end of the function scope is not reachable (usually because there is a `return` statement). This happens because we use `all_end_of_scope_symbol_declarations` and `all_end_of_scope_symbol_bindings` when listing members in `ide_support::all_declarations_and_bindings`, instead of getting the type for a (synthetic) use of the symbol at the cursor position. We might also be able to fix this by skipping reachability analysis for purposes of determining these end-of-scope-use-types?

For example:

https://play.ty.dev/c45ba126-18cd-4420-9b56-41cbf2b5fd1f

<img width="460" height="228" alt="Image" src="https://github.com/user-attachments/assets/d605878d-8d63-4e1c-bcbe-ec3b495a606d" />

<img width="671" height="398" alt="Image" src="https://github.com/user-attachments/assets/c0a8fcb4-ff22-4d22-86ac-4e2fcb720c5c" />

Note that we can still get proper attribute completions, for example, because as soon as the symbol name is fully spelled out, we have an *actual* use of the symbol at that position:

<img width="739" height="307" alt="Image" src="https://github.com/user-attachments/assets/32efb446-4376-42c8-ba78-efc8faa5802e" />

---

_Label `bug` added by @sharkdp on 2025-10-01 14:13_

---

_Label `server` added by @sharkdp on 2025-10-01 14:13_

---

_Label `completions` added by @sharkdp on 2025-10-01 14:13_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:44_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-12-08 18:15_

---

_Comment by @BurntSushi on 2025-12-08 18:15_

It looks like this might have gotten worse in the sense that the completions for this case no longer show up (instead of showing up with the wrong type): https://github.com/astral-sh/ty-vscode/issues/233#issuecomment-3628384609

I'll take a look at this this week, since "not shown at all" is categorically worse than "shown but wrong info."

---

_Comment by @carljm on 2025-12-08 18:23_

Seems like the LSP is probably looking up `end_of_scope_*` from the use-def map where (for the local scope) it should probably be using `all_reachable_*` instead. Because if a function has an unconditional return, _no_ bindings/declarations reach "end-of-scope". (The ideal would be neither of those, it would be "definitions reaching the cursor position" -- but this would be quite tricky -- it would require knowing the cursor position when we do semantic indexing so we can record the live definitions at that point.)

Switching from "end-of-scope" to "all-reachable" for module-global scopes would probably not be desirable -- it would mean in a case like this at module level we would respect both definitions instead of just the latter:

```py
x: int = 1
x: str = "foo"
```

---

_Comment by @MichaReiser on 2025-12-08 18:56_

> Switching from "end-of-scope" to "all-reachable" for module-global scopes would probably not be desirable -- it would mean in a case like this at module level we would respect both definitions instead of just the latter:

I think what's correct for this example also depends on the cursor position? 

```py
x: int = 1
// `x` is an `int` here
x: str = "foo"
// `x` is a `str` here
```

We do have a similar issue with go to definitions and renames where we rename all definitions instead of just the last reachable.

---

_Comment by @sharkdp on 2025-12-08 19:42_

> Seems like the LSP is probably looking up `end_of_scope_*` from the use-def map where (for the local scope) it should probably be using `all_reachable_*` instead

I also suggested two strategies of solving this in first post here (type for a "synthetic use" at the cursor position, skip reachability analysis when looking up the end-of-scope symbol). Those two still seem like very reasonable ideas to me. The latter sounds rather easy to implement (?). The former has a less clear implementation strategy (I'm thinking about using those definition_by_definition maps maybe?), but might be more correct in some cases?

---

_Comment by @gracepetryk on 2025-12-08 22:18_

Was about to file an issue for this but found this one. I did some quick bisecting and it looks like the completions stopped showing up in `ty==0.0.1a30`

---

_Closed by @BurntSushi on 2025-12-11 13:26_

---

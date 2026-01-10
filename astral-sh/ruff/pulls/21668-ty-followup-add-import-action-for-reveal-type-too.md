```yaml
number: 21668
title: "[ty] followup: add-import action for `reveal_type` too"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/auto-imp2
created_at: 2025-11-27T18:05:03Z
updated_at: 2025-12-08T22:44:18Z
url: https://github.com/astral-sh/ruff/pull/21668
synced_at: 2026-01-10T16:42:11Z
```

# [ty] followup: add-import action for `reveal_type` too

---

_Pull request opened by @Gankra on 2025-11-27 18:05_

_No description provided._

---

_Review requested from @carljm by @Gankra on 2025-11-27 18:05_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-27 18:05_

---

_Label `ty` added by @Gankra on 2025-11-27 18:05_

---

_Review requested from @sharkdp by @Gankra on 2025-11-27 18:05_

---

_Review requested from @dcreager by @Gankra on 2025-11-27 18:05_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-27 18:05_

---

_Label `server` added by @Gankra on 2025-11-27 18:05_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 18:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 18:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-11-27 18:13_

This specific one probably _could_ be added as a `Fix`, right? There's no extra computation required for us to figure out where we need to import it from, so it wouldn't slow down type-checking for it to be added as a `Fix` when we emit the diagnostic.

Also, it's possible we shouldn't suggest this autofix when the Python version is <3.11. It was only added to the `typing` module on Python 3.11 -- on earlier versions, you can import it from `typing_extensions`, but we don't know whether the user actually has that installed as a dependency or not. (According to typeshed, typing_extensions is part of the stdlib, but don't believe typeshed's lies -- that's just so that typeshed can use new typing features in the stdlib stubs ;)

---

_Comment by @Gankra on 2025-11-27 18:20_

Oh that's an interesting wrinkle, we *absolutely* constantly suggest importing stuff from `typing_extensions` 😬 

---

_Comment by @AlexWaygood on 2025-11-27 18:23_

can you use the `resolve_module()` option you added that ignores stub files to check whether there is actually a version of `typing_extensions` installed into `site-packages` before suggesting a `typing_extensions` import?

---

_Comment by @Gankra on 2025-11-27 18:31_

Filed:

* https://github.com/astral-sh/ty/issues/1658

---

_Review comment by @MichaReiser on `crates/ty_ide/src/code_action.rs`:27 on 2025-11-28 08:04_

```suggestion
    if lint_id == LintId::of(&UNRESOLVED_REFERENCE) || lint_id == LintId::of(&UNDEFINED_REVEAL){
```

---

_@MichaReiser approved on 2025-11-28 08:06_

I'm inclined to exclude `typing_extensions` from our auto-import machinery for now, here. But I'd also be okay leaving as is, given that these are all still manual actions 

https://github.com/astral-sh/ruff/blob/054e32e644d790817d2ab88ea887bb8a3d7618cc/crates/ty_ide/src/completion.rs#L523-L526

I think I'd prefer making this a `Fix` on the `reveal_type` diagnostic as it then also becomes available on the CLI

---

_Comment by @Gankra on 2025-11-28 13:58_

Thinking about it more, just because it's Almost Definitely `typing.reveal_type` doesn't actually mean it is? It feels a little dubious to refuse to check if their project has its own `reveal_type`

---

_Comment by @MichaReiser on 2025-11-28 14:19_

> Thinking about it more, just because it's Almost Definitely typing.reveal_type doesn't actually mean it is? It feels a little dubious to refuse to check if their project has its own reveal_type

I think it's fine as an unsafe fix

---

_Review request for @carljm removed by @carljm on 2025-12-03 06:47_

---

_Comment by @Gankra on 2025-12-08 22:39_

I keep getting annoyed that this doesn't work so I'm just going to land this and file followups.

---

_Merged by @Gankra on 2025-12-08 22:44_

---

_Closed by @Gankra on 2025-12-08 22:44_

---

_Branch deleted on 2025-12-08 22:44_

---

```yaml
number: 20221
title: "[ty] Attribute access on top/bottom materializations"
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - ty
assignees: []
merged: true
base: main
head: topattr
created_at: 2025-09-03T23:12:42Z
updated_at: 2025-09-04T19:27:18Z
url: https://github.com/astral-sh/ruff/pull/20221
synced_at: 2026-01-12T15:56:57Z
```

# [ty] Attribute access on top/bottom materializations

---

_@JelleZijlstra_


## Summary

Part of astral-sh/ty#994. The goal of this PR was to add correct behavior for attribute access on the top and bottom materializations. This is necessary for the end goal of using the top materialization for narrowing generics (`isinstance(x, list)`): we want methods like `x.append` to work correctly in that case.

It turned out to be convenient to represent materialization as a TypeMapping, so it can be stashed in the `type_mappings` list of a function object. This also allowed me to remove most concrete `materialize` methods, since they usually just delegate to the subparts of the type, the same as other type mappings. That is why the net effect of this PR is to remove a few hundred lines.

## Test Plan

I added a few more tests. Much of this PR is refactoring and covered by existing tests.

## Followups

Assigning to attributes of top materializations is not yet covered. This seems less important so I'd like to defer it.

I noticed that the `materialize` implementation of `Parameters` was wrong; it did the same for the top and bottom materializations. This PR makes the bottom materialization slightly more reasonable, but implementing this correctly will require extending the struct.

---

_Review requested from @carljm by @JelleZijlstra on 2025-09-03 23:12_

---

_Review requested from @AlexWaygood by @JelleZijlstra on 2025-09-03 23:12_

---

_Review requested from @sharkdp by @JelleZijlstra on 2025-09-03 23:12_

---

_Review requested from @dcreager by @JelleZijlstra on 2025-09-03 23:12_

---

_Comment by @github-actions[bot] on 2025-09-03 23:14_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ty` added by @AlexWaygood on 2025-09-04 00:11_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:830 on 2025-09-04 00:22_

I see some `materialize` methods in this PR, and some `materialize_impl`, with the same signature. What determines whether you chose to rename a given method to `materialize_impl`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:614 on 2025-09-04 00:25_

So I am currently working on recursive type alias support, and as part of that I have a local branch where I'd already added a cycle-detection visitor to all materialize methods. I think this PR is an improvement, but it will probably mean I need all `materialize_impl` methods (at least any that can be called from an `apply_type_mapping_impl` method, such as in this spot) to accept and pass on an `ApplyTypeMappingVisitor`.

I think that should be fine, and I can handle it as a follow-up, just mentioning it in case you see any potential issues.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:734 on 2025-09-04 00:27_

Here I think I will need this `materialize_impl` to already accept the `ApplyTypeMappingVisitor`, and I'll need to pass it along here, and in all the variance arms above.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:450 on 2025-09-04 00:27_

I've seen this a couple places in this PR -- should it be a method on `TypeMapping`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1237 on 2025-09-04 00:34_

Aren't these reversed? I think the bottom and top materializations are counter-intuitive here due to contravariance of parameters. Isn't `Parameters::object(db)` the subtype of all materializations of the gradual parameter-signature (it is a valid substitute for any parameter-signature, because it can accept any call arguments)? And the top materialization (the super-type of all materializations, for which any parameter-signature is a valid substitute) is more difficult to represent. Or is it actually just `*args: Never, **kwargs: Never`, which accepts no calls, and which any other parameter signature is a subtype of?

---

_@carljm approved on 2025-09-04 00:36_

Looks good, thank you! A few comments, none blocking -- though the parameter materialization thing should probably be addressed, if I'm not wrong there.

---

_Comment by @carljm on 2025-09-04 00:41_

Oh, I just noticed the stack overflow in mypy-primer. So it may be that you actually do need to add some of the visitor-threading I mentioned above in this PR, to avoid regressing. (Though I'd recommend reproducing the stack overflow locally to verify the exact cyclic stack in lldb before trying to fix.)

---

_@JelleZijlstra reviewed on 2025-09-04 01:12_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/signatures.rs`:1237 on 2025-09-04 01:12_

I think that may not be right because we flip the variance earlier (in whatever struct contains Parameters).

> Or is it actually just `*args: Never, **kwargs: Never`, which accepts no calls, and which any other parameter signature is a subtype of?

That signature would accept a call with no arguments (`f()`), so a signature that has required arguments wouldn't be a subtype of it.

---

_Comment by @JelleZijlstra on 2025-09-04 01:13_

Yes, I'll need to look into the crash. Intuitively it's not clear why materialization would diverge, maybe it's for recursive aliases? If so would it make sense to wait for your more general fix?

---

_Comment by @carljm on 2025-09-04 01:46_

I think it most likely is recursive aliases (though it could also be recursive protocols). The fix isn't really a "general" one; part of it is ensuring that we propagate cycle detection visitors through all recursive type visits (a stack overflow is usually caused by failing to do this thoroughly). It seems like this PR is regressing our existing cycle detection coverage in some way; ideally that would be fixed in this PR. 

---

_@carljm reviewed on 2025-09-04 16:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1237 on 2025-09-04 16:19_

> we flip the variance earlier (in whatever struct contains Parameters)

Ah, that makes sense -- might be worth a comment. Or it might be worth changing this so we handle the variance flip in `Parameters`? To the extent we end up defining the "top" and "bottom" materializations of a `Parameters`, I think I would find it more intuitive for those to correspond with the actual top and bottom signature, not be reversed. (I know that in my previous comment I called it counter-intuitive 🤷 ). Not an important point.

> That signature would accept a call with no arguments (`f()`), so a signature that has required arguments wouldn't be a subtype of it.

Oh, yeah, of course. So we will need (not in this PR) a dedicated representation for "parameters which accept no call".

---

_@JelleZijlstra reviewed on 2025-09-04 16:49_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:830 on 2025-09-04 16:49_

I think a good rule is going to be that materialize calls apply_type_mapping(_impl), and apply_type_mapping calls materialize_impl. I believe I do that mostly consistently already but will double check.

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/signatures.rs`:450 on 2025-09-04 16:50_

Yeah maybe, I was struggling a bit with getting things to compile. I can give it another try.

---

_@JelleZijlstra reviewed on 2025-09-04 16:50_

---

_@JelleZijlstra reviewed on 2025-09-04 17:34_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/signatures.rs`:450 on 2025-09-04 17:34_

I couldn't get this to work, maybe I need to learn more Rust or something. The pattern only appears twice, so doesn't seem too bad to leave it duplicated for now.

---

_@JelleZijlstra reviewed on 2025-09-04 17:50_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/signatures.rs`:1237 on 2025-09-04 17:50_

I'll add a comment.

---

_@JelleZijlstra reviewed on 2025-09-04 18:09_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:734 on 2025-09-04 18:09_

Done

---

_Comment by @github-actions[bot] on 2025-09-04 18:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@JelleZijlstra reviewed on 2025-09-04 18:17_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types/generics.rs`:614 on 2025-09-04 18:17_

I made them all accept an ApplyTypeMappingVisitor, that fixes the hang seen in mypy-primer though I won't pretend to understand how it actually helps.

---

_@carljm approved on 2025-09-04 19:01_

Looks great, thank you!

---

_Merged by @carljm on 2025-09-04 19:01_

---

_Closed by @carljm on 2025-09-04 19:01_

---

_Branch deleted on 2025-09-04 19:27_

---

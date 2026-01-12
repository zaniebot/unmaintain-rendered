```yaml
number: 18299
title: "[ty] Add hint if async context manager is used in non-async with statement"
type: pull_request
state: merged
author: lipefree
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: info-async-with
created_at: 2025-05-25T20:06:42Z
updated_at: 2025-05-26T21:45:32Z
url: https://github.com/astral-sh/ruff/pull/18299
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Add hint if async context manager is used in non-async with statement

---

_@lipefree_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Implements https://github.com/astral-sh/ty/issues/508, considering the following code : 
```py
class Manager:
    async def __aenter__(self): ...
    async def __aexit__(self, *args): ...

# error: [invalid-context-manager] "Object of type `Manager` cannot be used with `with` because it does not implement `__enter__` and `__exit__`"
with Manager():
    ...
```
The error naturally happens since `__enter__` and `__exit__` are not implemented but we observe that `__aenter__` and `__aexit__` are implemented. Using this information, the heuristic is that the user probably wanted to use `async with` which is now mentioned in the subdiagnostics as : 
```
note: Objects of type `Manager` *can* be used as async context managers
note: Consider using `async with` here
```

## Test Plan

I have create a mdtest with snapshot to capture the additional subdiagnostics. I can create more tests if needed.


---

_Review requested from @carljm by @lipefree on 2025-05-25 20:06_

---

_Review requested from @AlexWaygood by @lipefree on 2025-05-25 20:06_

---

_Review requested from @sharkdp by @lipefree on 2025-05-25 20:06_

---

_Review requested from @dcreager by @lipefree on 2025-05-25 20:06_

---

_Comment by @github-actions[bot] on 2025-05-25 20:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-05-25 20:09_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-25 20:09_

---

_Comment by @lipefree on 2025-05-25 21:08_

When looking at the mdtest itself, it is hard to know that what is expected, is the additional information provided in the sub-diagnosis. I don't know if I can improve it so that it is clear how the sub-diagnosis should look like instead of having to look at the snap file.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6071 on 2025-05-26 05:48_

The CLI doesn't support markdown rendering. That's why I wouldn't use * for highlighting. 

---

_@MichaReiser reviewed on 2025-05-26 05:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6068 on 2025-05-26 11:18_

These `__aenter__` and `__aexit__` calls will fail if you modify/extend your test example and annotated the parameter types of these two methods on the `Manager` class accordingly. `None` will not generally be a valid type for all of the arguments.

We do not necessarily need to be very strict with the passed argument types here (since we're only using it to add a diagnostic hint, and it seems fine to emit it even if that call would fail when passing the wrong parameters). So I guess it's fine to pass `Type::unknown()` here instead? Another option would be to allow not just `Ok(_)` results, but also `Err(CallDunderError::CallError(..))`?

---

_@sharkdp reviewed on 2025-05-26 11:18_

---

_@lipefree reviewed on 2025-05-26 11:49_

---

_Review comment by @lipefree on `crates/ty_python_semantic/src/types.rs`:6068 on 2025-05-26 11:49_

Hey ! Thank you for the review ! If I understand correctly you want to add the sub-diagnosis event if the code was not annotating the types correctly ? 
Example : 

```py
class Foo:
    async def __aenter__(self):
        ...
    
    def __aexit__(self,
                 exc_type: str, # <= wrong type
                 exc_value: str,
                 traceback: str
                ):
        ...
        
with Foo():
    ...

```

But even in this case, we should provide the sub-diagnosis even if the provided typing is incorrect. I guess it makes sense. I am just too unexperienced yet, should I implement it to take care of this case ? But then should we also provide the sub-diagnosis if the user is passing the wrong number of arguments ? 

```py
class Foo:
    async def __aenter__(self):
        ...
    
    def __aexit__(self,
                 exc_type,
                 exc_value,
                 traceback,
                 extra_arg
                ):
        ...
        
with Foo():
    ...

```

---

_@sharkdp reviewed on 2025-05-26 12:06_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6068 on 2025-05-26 12:06_

> But then should we also provide the sub-diagnosis if the user is passing the wrong number of arguments ?

Probably? I think you could achieve this by following this route:

> Another option would be to allow not just Ok(_) results, but also Err(CallDunderError::CallError(..))?

I think we should pass `Type::unknown()` anyway. Passing `None` is just wrong.

---

_@lipefree reviewed on 2025-05-26 12:57_

---

_Review comment by @lipefree on `crates/ty_python_semantic/src/types.rs`:6068 on 2025-05-26 12:57_

I will address this and add more testing then !

---

_@sharkdp reviewed on 2025-05-26 17:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6068 on 2025-05-26 17:47_

Just to clarify, in case it was unintentional: you're still using `Type::none` in your latest commit, instead of `Type::unknown`.

---

_@lipefree reviewed on 2025-05-26 18:23_

---

_Review comment by @lipefree on `crates/ty_python_semantic/src/types.rs`:6068 on 2025-05-26 18:23_

Sorry I just changed it in the last commit. Thank you very much for bearing with me !

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6063 on 2025-05-26 19:20_

This can be simplified to
```suggestion
        if let (
            Ok(_) | Err(CallDunderError::CallError(..)),
            Ok(_) | Err(CallDunderError::CallError(..)),
        ) = (
```
I think.

---

_@sharkdp approved on 2025-05-26 19:27_

Thank you!

I pushed one commit with a code simplification and some minor rewordings.

---

_Renamed from "[ty] Add subdiagnostic hint if async context manager is used in non-async with statement" to "[ty] Add hint if async context manager is used in non-async with statement" by @sharkdp on 2025-05-26 19:32_

---

_Merged by @sharkdp on 2025-05-26 19:34_

---

_Closed by @sharkdp on 2025-05-26 19:34_

---

_@lipefree reviewed on 2025-05-26 21:45_

---

_Review comment by @lipefree on `crates/ty_python_semantic/src/types.rs`:6063 on 2025-05-26 21:45_

Thank you very much for all the reviews you gave, I really learned a lot during this PR about python, rust and ty !

---

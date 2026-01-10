```yaml
number: 20858
title: "[ty] Handle decorators which return unions of `Callable`s"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/handle-unions-of-callables
created_at: 2025-10-14T08:23:49Z
updated_at: 2025-10-14T09:47:51Z
url: https://github.com/astral-sh/ruff/pull/20858
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Handle decorators which return unions of `Callable`s

---

_Pull request opened by @sharkdp on 2025-10-14 08:23_

## Summary

If a function is decorated with a decorator that returns a union of `Callable`s, also treat it as a union of function-like `Callable`s.

Labeling as `internal`, since the previous change has not been released yet.

## Test Plan

New regression test.

---

_Review requested from @carljm by @sharkdp on 2025-10-14 08:23_

---

_Label `internal` added by @sharkdp on 2025-10-14 08:23_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-14 08:23_

---

_Review requested from @dcreager by @sharkdp on 2025-10-14 08:23_

---

_Label `ty` added by @sharkdp on 2025-10-14 08:23_

---

_Comment by @github-actions[bot] on 2025-10-14 08:25_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@sharkdp reviewed on 2025-10-14 08:27_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2223 on 2025-10-14 08:27_

Initially, I also expanded this "is the input function-like" check to handle unions. @AlexWaygood, I think that was also your original idea. I can do this, but I'm struggling to come up with a scenario where this would be relevant. You would need to have a union of function literals that is being `@decorated`. While it may be possible to construct this somehow, it doesn't feel like it could come up in practice?

---

_@sharkdp reviewed on 2025-10-14 08:28_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2216 on 2025-10-14 08:28_

I chose to not handle intersections, because that would require an explicit `Intersection` return type on the decorator.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2202 on 2025-10-14 08:30_

Surely this should be

```suggestion
                    fn as_function_like_callable<'d>(
```

According to your new naming convention ;)

---

_@AlexWaygood approved on 2025-10-14 08:30_

Thanks!

---

_@AlexWaygood reviewed on 2025-10-14 08:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2216 on 2025-10-14 08:31_

That makes sense... Maybe it's worth a comment in the code?

---

_Comment by @github-actions[bot] on 2025-10-14 08:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-10-14 09:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2202 on 2025-10-14 09:42_

Well... here, an actual transformation is taking place ;-)

---

_Merged by @sharkdp on 2025-10-14 09:47_

---

_Closed by @sharkdp on 2025-10-14 09:47_

---

_Branch deleted on 2025-10-14 09:47_

---

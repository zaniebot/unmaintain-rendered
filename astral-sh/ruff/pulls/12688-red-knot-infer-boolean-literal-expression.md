```yaml
number: 12688
title: "[red-knot] Infer boolean literal expression"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/infer-bool-literal
created_at: 2024-08-05T12:12:46Z
updated_at: 2025-05-07T15:23:57Z
url: https://github.com/astral-sh/ruff/pull/12688
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Infer boolean literal expression

---

_Pull request opened by @dhruvmanila on 2024-08-05 12:12_

## Summary

This PR implements type inference for boolean literal expressions.

## Test Plan

Add test cases for `True` and `False`.


---

_Label `red-knot` added by @dhruvmanila on 2024-08-05 12:12_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-05 12:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-05 12:12_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-05 12:12_

---

_Comment by @github-actions[bot] on 2024-08-05 12:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:180 on 2024-08-05 12:41_

Here we'll want to fall back to looking up attributes and methods on the `builtins.bool` class in our vendored typeshed stubs. But I think we can defer that for this PR.

---

_@AlexWaygood reviewed on 2024-08-05 12:59_

Nice. These changes all look good to me.

I have a concern about union types, though. How do we want a union of boolean literals to be represented internally? For an object that could be the `True` constant or could be the `False` constant, should we infer `Literal[True] | Literal[False]` (a union of literals), or should we eagerly normalize it to `<instance of bool>` (since we know that the `bool` class is special: there will only be two possible instance of `bool`).
 - If the former, I think we will want to modify the `Display` implementation for `DisplayUnionType` so that `Literal[True] | Literal[False]` is simplified to `Literal[True, False]`, the same as we do for numeric literals
 - If the latter, some larger changes will be required

Mypy appears never to normalize `Literal[True, False]` to `bool`; pyright appears to always do so. ([Link](https://mypy-play.net/?mypy=latest&python=3.12&gist=425775a11740556527e3fb397a289ae4), [link](https://pyright-play.net/?pyrightVersion=1.1.368&code=JYWwDg9gTgLgBFAhgOwCYRAKAGZQ3GATzGGQHM5RJY4AZYGAUyQBtNMAPALjoecRYBtACpQArowC6cALxxREzIR70mrQQDEBAZymy4WlrvbBsCFOhAA6AMYALCMBuMAFCPGMANAZ1SAlFyYcMFwAF76HJiMRoyBIWH6hOxQjABujAIA%2BkRgrqF%2B7KiMZtgu3LxqAu4S3oa6kgFBISnpWTmuHH5AA).) I think I weakly favour pyright's approach here, but not sure.

A separate question (which I think can almost certainly be deferred for now) is that @carljm added some fancy logic for int literals in `infer_binary_expression`. We could also add some understanding of `bool` literals to that method, since `True + True == True + 1 == 1 + 1 == 2`

---

_@AlexWaygood approved on 2024-08-05 13:04_

I actually think even the concern I have about union types can probably be deferred for now. So I'm happy with this landing as-is, as long as we keep track of these open questions somewhere.

---

_Comment by @dhruvmanila on 2024-08-05 14:23_

> Mypy appears never to normalize `Literal[True, False]` to `bool`; pyright appears to always do so. ([Link](https://mypy-play.net/?mypy=latest&python=3.12&gist=425775a11740556527e3fb397a289ae4), [link](https://pyright-play.net/?pyrightVersion=1.1.368&code=JYWwDg9gTgLgBFAhgOwCYRAKAGZQ3GATzGGQHM5RJY4AZYGAUyQBtNMAPALjoecRYBtACpQArowC6cALxxREzIR70mrQQDEBAZymy4WlrvbBsCFOhAA6AMYALCMBuMAFCPGMANAZ1SAlFyYcMFwAF76HJiMRoyBIWH6hOxQjABujAIA%2BkRgrqF%2B7KiMZtgu3LxqAu4S3oa6kgFBISnpWTmuHH5AA).) I think I weakly favour pyright's approach here, but not sure.

What's the benefit of the one or the other? I think I'd prefer Pyright's approach as well because it can be resolved.

> A separate question (which I think can almost certainly be deferred for now) is that @carljm added some fancy logic for int literals in `infer_binary_expression`. We could also add some understanding of `bool` literals to that method, since `True + True == True + 1 == 1 + 1 == 2`

Yes, I started playing around with binary expressions as well but thought to have it as it's own PR which would include all possible combinations.

---

_@dhruvmanila reviewed on 2024-08-05 14:27_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:180 on 2024-08-05 14:27_

What's the meaning of "member" in this context? From what I understand, it's the type of an attribute on the given type. So, for instance, `set.append` would be a function type. Is this understanding correct?

---

_@AlexWaygood reviewed on 2024-08-05 14:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:180 on 2024-08-05 14:28_

Yes, your understanding is correct there

---

_Comment by @AlexWaygood on 2024-08-05 14:35_

> What's the benefit of the one or the other? I think I'd prefer Pyright's approach as well because it can be resolved.

I think one "benefit" of mypy's approach is that you don't have to apply quite so much special casing to boolean literals specifically inside the type checker implementation when figuring out how unions of boolean literals resolve. `Literal[True] | Literal[False]` resolves to `Literal[True, False]` in exactly the same way that `Literal[1] | Literal[2]` resolves to `Literal[1, 2]` and `Literal["foo"] | Literal[True]` resolves to `Literal["foo", True]`.

One case where normalizing `bool` _back_ to `Literal[True, False]` will be necessary will be reachability analysis. In the following snippet, it's self evident that the third `case` branch is unreachable, and the type checker is not required to special-case boolean types in any way in order to understand this; it follows from its generalised understanding of `Literal` types:

```py
from typing import Literal

def f(x: Literal[True, False]):
    match x:
        case True:
            ...
        case False:
            ...
        case unreachable:
            ...
```

However: we'll need to apply that special casing to `bool` anyway. We'll also need to be able to understand types annotated with `bool` as being functionally equivalent to `Literal[True, False]`, or we won't understand the third branch in _this_ function as being unreachable:

```py
from typing import Literal

def f(x: bool):
    match x:
        case True:
            ...
        case False:
            ...
        case unreachable:
            ...
```

So leaving the union unnormalized as `Literal[True, False]` doesn't really get us out of having to special-case `bool`.

I also... wouldn't necessarily jump to the conclusion that this is a deliberate choice by mypy with an explicit motivation 😄 it's a very old type checker, and support for `Literal` types was added to mypy after mypy had already been around for many years. To me, pyright's approach also seems significantly better here.

---

_@carljm approved on 2024-08-05 18:30_

Looks great, thank you!

Enums are a similar case where we'll need to understand sealed types (i.e. types with a finite number of inhabitants). I'm fine with delaying handling of the equivalence of `bool` and `Literal[True, False]` for now. I added https://github.com/astral-sh/ruff/issues/12694 to track this issue.

---

_Merged by @carljm on 2024-08-05 18:30_

---

_Closed by @carljm on 2024-08-05 18:30_

---

_Branch deleted on 2024-08-05 18:30_

---

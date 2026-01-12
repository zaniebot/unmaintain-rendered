```yaml
number: 18112
title: "[ty] Fix normalization of unions containing instances parameterized with unions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/gradual-normalize
created_at: 2025-05-15T01:38:35Z
updated_at: 2025-05-15T12:16:30Z
url: https://github.com/astral-sh/ruff/pull/18112
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Fix normalization of unions containing instances parameterized with unions

---

_@AlexWaygood_

## Summary

- Recurse into the specialisations of specialised generics when normalizing them, in order to ensure that `Foo[int | str]` and `Foo[str | int]` have the same Salsa ID when normalized
- Normalize `Unknown`, `Todo` and `Any` all to `Any`. I'll be honest here: I still don't totally understand why this is necessary, but I _can_ say that it makes the nondeterministic behaviour on hydra-zen go away. And I think it may be a good thing to do anyway:
  - It makes these types easier to reason about when they appear in unions/intersections
  - It reflects the fact that these types are in fact gradually equivalent.

This is stacked on top of https://github.com/astral-sh/ruff/pull/18111, because otherwise it causes some small regressions in our `redundant-cast` mdtests because of the new normalisations of dynamic types.

Fixes https://github.com/astral-sh/ty/issues/369

## Test Plan

- Added mdtests for the issue with instances of specialised generics
- Repeatedly ran this branch on hydra-zen using the setup described in https://github.com/astral-sh/ty/issues/369#issue-3061167191 and observed that there was no longer any non-deterministic behaviour when checking hydra-zen (and I can reproduce the non-deterministic behaviour on `main` using that setup)


---

_Review requested from @carljm by @AlexWaygood on 2025-05-15 01:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-15 01:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-15 01:38_

---

_Label `ty` added by @AlexWaygood on 2025-05-15 01:38_

---

_Label `bug` added by @AlexWaygood on 2025-05-15 01:39_

---

_Comment by @github-actions[bot] on 2025-05-15 01:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
- error[type-assertion-failure] tests/annotations/declarations.py:969:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:980:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 645 diagnostics
+ Found 643 diagnostics

```
</details>


---

_@carljm approved on 2025-05-15 02:39_

---

_Closed by @AlexWaygood on 2025-05-15 02:43_

---

_Reopened by @AlexWaygood on 2025-05-15 02:43_

---

_Renamed from "[ty] Fix normalization of unions containing instances generic over unions" to "[ty] Fix normalization of unions containing instances parameterized with unions" by @AlexWaygood on 2025-05-15 02:47_

---

_Merged by @AlexWaygood on 2025-05-15 02:48_

---

_Closed by @AlexWaygood on 2025-05-15 02:48_

---

_Branch deleted on 2025-05-15 02:48_

---

_Comment by @sharkdp on 2025-05-15 10:25_

Thank you @AlexWaygood.

With this PR, you fixed one particular problem in normalization. I'm not trying to imply that my attempt in https://github.com/astral-sh/ruff/pull/18091 is the right solution, but I think I'm not happy with the current state. There is nothing that prevents us from introducing similar problems in the future. And there is no guarantee that other problems already exist. In fact, it's easy to find [other cases that are still problematic](https://play.ty.dev/04e56ba2-3912-4c4d-9158-1bef41d02ac9) by embedding unions in types that we don't have any special handling for, yet. A lot of `Type`-variants contain `Type`s further down in their structure, not just unions, intersections and tuples. Especially with the introduction of generics.

We have seen in https://github.com/astral-sh/ty/issues/369 that this can lead to extremely subtle bugs (maybe we'll be more aware next time, but it took me several hours to find the root cause). So I with there would be some way we could get help from the compiler to get this right. 

---

_Comment by @AlexWaygood on 2025-05-15 12:16_

@sharkdp yes, I agree! I think there are probably still other latent bugs with normalisation lurking in our codebase already. This PR fixes some specific issues with normalisation, but it doesn't fix the general problem that it's hard to get this approach right.

I'd also love to see if we can explore ways to make our design more robust. I liked the idea you mentioned on Discord the other day of having a generalised visitor pattern for recursing into types and applying transformations to all the sub-nodes — it's possible that might make this easier to get right?

---

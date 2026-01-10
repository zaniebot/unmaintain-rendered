```yaml
number: 20356
title: "[`ty`] Include `NamedTupleFallback` members in `NamedTuple` instance completions"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ide-namedtuple-fallback-instance-completions
created_at: 2025-09-11T20:02:54Z
updated_at: 2025-09-15T09:00:03Z
url: https://github.com/astral-sh/ruff/pull/20356
synced_at: 2026-01-10T17:40:28Z
```

# [`ty`] Include `NamedTupleFallback` members in `NamedTuple` instance completions

---

_Pull request opened by @TaKO8Ki on 2025-09-11 20:02_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes https://github.com/astral-sh/ty/issues/1161

Include `NamedTupleFallback` members in `NamedTuple` instance completions.

- Augment instance attribute completions when completing on NamedTuple instances by merging members from `_typeshed._type_checker_internals.NamedTupleFallback`

## Test Plan

<!-- How was it tested? -->

Adds a minimal completion test `namedtuple_fallback_instance_methods`


---

_Review requested from @carljm by @TaKO8Ki on 2025-09-11 20:02_

---

_Review requested from @AlexWaygood by @TaKO8Ki on 2025-09-11 20:02_

---

_Review requested from @sharkdp by @TaKO8Ki on 2025-09-11 20:02_

---

_Review requested from @dcreager by @TaKO8Ki on 2025-09-11 20:02_

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-09-11 20:02_

---

_Comment by @github-actions[bot] on 2025-09-11 20:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-11 20:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @ntBre on 2025-09-11 20:39_

---

_Label `server` added by @AlexWaygood on 2025-09-11 21:03_

---

_Review requested from @BurntSushi by @carljm on 2025-09-11 21:26_

---

_Review request for @carljm removed by @carljm on 2025-09-11 21:26_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:1757 on 2025-09-12 07:16_

Can we please add tests for accessing methods (and classmethods) on `P` directly as well? And also add an example with a generic `NamedTuple`? It looks to me like those cases are not yet handled (see what we do for `TypedDict`).

---

_@sharkdp reviewed on 2025-09-12 07:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:109 on 2025-09-12 07:19_

Is `extend_with_instance_members` correct here, or should it be `self.extend_with_type(db, KnownClass::NamedTupleFallback.to_instance(db));`? For example, is the classmethod `_make` available on instances of `NamedTuple`s? 

---

_@sharkdp reviewed on 2025-09-12 07:19_

---

_@sharkdp reviewed on 2025-09-12 07:20_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:101 on 2025-09-12 07:20_

Minor: maybe use `class_literal` instead of this abbreviation, like elsewhere in this method?

---

_@AlexWaygood reviewed on 2025-09-12 07:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:109 on 2025-09-12 07:28_

> is the classmethod `_make` available on instances of `NamedTuple`s?

Yes, it is. `NamedTuple` is pretty different from `TypedDict` in this regard — it's conceptually more similar to a dataclass. Inheriting from NamedTuple creates a tuple subclass that has a number of convenient methods monkey-patched onto it by the runtime, including `_make` etc. Instantiating that namedtuple class creates an instance of that namedtuple class just like instantiating any normal Python class would, so classmethods like `_make` are available on instances of namedtuple classes in the same way that classmethods are generally available on instances

---

_@sharkdp reviewed on 2025-09-12 07:30_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:109 on 2025-09-12 07:30_

> > is the classmethod `_make` available on instances of `NamedTuple`s?
> 
> Yes, it is.

Yeah, sorry. I meant to say: is `_make` available *as a completion* on instances of `NamedTuple`s (with the current implementation here; as it should be).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:109 on 2025-09-12 07:45_

Ohhh sorry! I totally misinterpreted that as a question about what happens at runtime; my apologies 😄

---

_@AlexWaygood reviewed on 2025-09-12 07:45_

---

_@TaKO8Ki reviewed on 2025-09-12 08:07_

---

_Review comment by @TaKO8Ki on `crates/ty_ide/src/completion.rs`:1757 on 2025-09-12 08:07_

Ok. Is it better to move these tests into the mdtest like `TypedDict` instead?

https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md

---

_@BurntSushi reviewed on 2025-09-12 12:42_

LGTM, but I defer to @sharkdp for approval here. :-)

---

_@sharkdp reviewed on 2025-09-12 13:35_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:1757 on 2025-09-12 13:35_

That might be a bit easier, and more consistent with what we already have, yes.

---

_@TaKO8Ki reviewed on 2025-09-12 17:23_

---

_Review comment by @TaKO8Ki on `crates/ty_python_semantic/src/types/ide_support.rs`:101 on 2025-09-12 17:23_

I've changed it to class_literal.

---

_@TaKO8Ki reviewed on 2025-09-12 17:30_

---

_Review comment by @TaKO8Ki on `crates/ty_ide/src/completion.rs`:1757 on 2025-09-12 17:30_

I've added a mdtest for NamedTuple including generic NamedTuple example.

---

_Comment by @TaKO8Ki on 2025-09-12 18:00_

Is the CI error related to the changes? It seems to happen in a different branch. https://github.com/astral-sh/ruff/actions/runs/17681728186

---

_Comment by @AlexWaygood on 2025-09-12 18:06_

> Is the CI error related to the changes? It seems to happen in a different branch. [astral-sh/ruff/actions/runs/17681728186](https://github.com/astral-sh/ruff/actions/runs/17681728186)

no, it's not related to your change. We're not yet sure why that's started happening on all PRs, but you don't need to worry about it.

---

_Review requested from @sharkdp by @TaKO8Ki on 2025-09-12 18:11_

---

_Comment by @carljm on 2025-09-12 20:52_

The CI error is now fixed in main, so a rebase and force-push (or merge from main) will fix it on this PR.

---

_@sharkdp approved on 2025-09-15 08:50_

Thank you. Support for `type[SomeNamedTuple]` was also missing. I added that myself.

---

_Merged by @sharkdp on 2025-09-15 09:00_

---

_Closed by @sharkdp on 2025-09-15 09:00_

---

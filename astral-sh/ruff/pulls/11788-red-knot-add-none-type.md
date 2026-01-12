```yaml
number: 11788
title: "[red-knot] add None type"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/none
created_at: 2024-06-06T22:52:14Z
updated_at: 2024-06-07T14:40:23Z
url: https://github.com/astral-sh/ruff/pull/11788
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] add None type

---

_@carljm_

Add type for None.

---

_Label `red-knot` added by @carljm on 2024-06-06 22:52_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-06 22:52_

---

_Review requested from @MichaReiser by @carljm on 2024-06-06 22:52_

---

_Comment by @github-actions[bot] on 2024-06-06 23:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-06-07 06:19_

---

_Comment by @MichaReiser on 2024-06-07 06:21_

Looks good. I encourage you to land PRs without review if you consider them trivial or feel very certain about them. It's not that I mind reviewing them, but it can help you to move quicker (a bit less rebasing etc)

---

_@AlexWaygood reviewed on 2024-06-07 12:29_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:31 on 2024-06-07 12:29_

I'm okay with this if it unblocks your work on type narrowing, but in the long term, I'm not sure we should represent `None` as a distinct kind of type. Mypy and pyright have both had a lot of bugs because of the extent to which they special-case `None` (pyright particularly), and I'd like us to think hard about whether we need to do so to the same extent that they've done.

What's actually special about `None`? Three things, I think:
1. When we see `None` in a type annotation, we should immediately desugar that to `Instance(types.NoneType)` (or [the typeshed backport](https://github.com/python/typeshed/blob/4da35728277ee15c16f578ecf62acfc71b8caadf/stdlib/_typeshed/__init__.pyi#L308-L311) for Python less than 3.10)
2. When printing the type to users in diagnostics, we should immediately resugar it from `Instance(types.NoneType)` to `None`
3. We need to understand internally that `NoneType` has exactly one instance, `None`, so that you can use identity checks such as `x is not None` for type narrowing

Well, I say "three things", but actually, the third point isn't unique at all! Other singletons exist in Python, such as `...`, `NotImplemented`, `typing.NoDefault` on py313+, etc. Mypy and pyright both support narrowing unions including `types.EllipsisType` with `x is not ...`, and I think we should too. And maybe it would be reasonable for a user to narrow a `bool | str` union via a `x is not True and x is not False` check.

So does `None` really need to be its own variant in the enum here? Or should we have a more generalised way of representing builtin types that only have 1 or 2 possible instances, and that can therefore be narrowed via identity checks?

As I say, I'm fine with this for now, though.

---

_@carljm reviewed on 2024-06-07 14:20_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types.rs`:31 on 2024-06-07 14:20_

This is a good question, and I debated the same thing while working on this PR.

I went with this for now basically because otherwise I would have had to wait for typeshed to be ready, and I needed some representation of `None` for type narrowing to work. But I think it will probably be better to switch to `Instance(types.NoneType)` once we have typeshed in place. I'll add a TODO for that.

---

_@AlexWaygood reviewed on 2024-06-07 14:22_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:31 on 2024-06-07 14:22_

Great, sounds like we're aligned. Thanks!

---

_@AlexWaygood reviewed on 2024-06-07 14:22_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:31 on 2024-06-07 14:22_

I'll have typeshed for you soon!!

---

_@AlexWaygood approved on 2024-06-07 14:22_

---

_Merged by @carljm on 2024-06-07 14:40_

---

_Closed by @carljm on 2024-06-07 14:40_

---

_Branch deleted on 2024-06-07 14:40_

---

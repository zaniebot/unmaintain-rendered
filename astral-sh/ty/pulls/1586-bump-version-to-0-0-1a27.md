```yaml
number: 1586
title: Bump version to 0.0.1a27
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/rel2
created_at: 2025-11-18T19:21:41Z
updated_at: 2025-11-18T19:56:48Z
url: https://github.com/astral-sh/ty/pull/1586
synced_at: 2026-01-12T15:54:27Z
```

# Bump version to 0.0.1a27

---

_@Gankra_

_No description provided._

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:12 on 2025-11-18 19:23_

```suggestion
- Fix global symbol lookup from eagerly executed scopes such as comprehensions and classes ([#21317](https://github.com/astral-sh/ruff/pull/21317))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2025-11-18 19:25_

```suggestion
- Fix false positive for instance attributes that are declared as `Final` in the class body but have their value assigned in the class's `__init__` method ([#21158](https://github.com/astral-sh/ruff/pull/21158))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2025-11-18 19:25_

```suggestion
- Add support for `NewType` ([#21157](https://github.com/astral-sh/ruff/pull/21157))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:22 on 2025-11-18 19:25_

```suggestion
- Precise inference for generator expressions ([#21437](https://github.com/astral-sh/ruff/pull/21437))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2025-11-18 19:27_

```suggestion
- Correctly infer the specialization of a non-invariant PEP-695 generic class that has an annotated `self` parameter in its `__init__` method ([#21325](https://github.com/astral-sh/ruff/pull/21325))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:36 on 2025-11-18 19:28_

```suggestion
- Improve use of type context when inferring the result of a generic constructor call ([#20933](https://github.com/astral-sh/ruff/pull/20933), [#21442](https://github.com/astral-sh/ruff/pull/21442))
- Improve use of type context when inferring the result of a generic call expression ([#21210](https://github.com/astral-sh/ruff/pull/21210))
```

---

_Review comment by @carljm on `CHANGELOG.md`:19 on 2025-11-18 19:29_

```suggestion
- Support `typing.NewType` ([#21157](https://github.com/astral-sh/ruff/pull/21157))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:37 on 2025-11-18 19:29_

```suggestion
- Improve heuristics used to decide when it is appropriate to "promote" a `Literal` type such as `Literal[42]` to its instance supertype (in this case, `int`) when solving type variables ([#21439](https://github.com/astral-sh/ruff/pull/21439))
```

---

_Review comment by @carljm on `CHANGELOG.md`:22 on 2025-11-18 19:29_

Other type checkers may support generator expressions, but only ty has support for the lesser-known "genererator expression"!

```suggestion
- Support generator expressions ([#21437](https://github.com/astral-sh/ruff/pull/21437))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2025-11-18 19:29_

```suggestion
- Improve use of type context to infer conditional expressions ([#21443](https://github.com/astral-sh/ruff/pull/21443))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:46 on 2025-11-18 19:30_

```suggestion
- Ensure annotation/type expressions in stub files are always deferred ([#21401](https://github.com/astral-sh/ruff/pull/21401), [#21456](https://github.com/astral-sh/ruff/pull/21456))
```

---

_Review comment by @carljm on `CHANGELOG.md`:63 on 2025-11-18 19:31_

```suggestion
- Suppress some trivial expression inlay hints ([#21367](https://github.com/astral-sh/ruff/pull/21367))
- Suppress inlay hints for `+1` and `-1` ([#21368](https://github.com/astral-sh/ruff/pull/21368))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:47 on 2025-11-18 19:31_

```suggestion
- Sync vendored typeshed stubs ([#21466](https://github.com/astral-sh/ruff/pull/21466)). Typeshed diff](https://github.com/python/typeshed/compare/bf7214784877c52638844c065360d4814fae4c65...f8cdc0bd526301e873cd952eb0d457bdf2554e57)
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:52 on 2025-11-18 19:31_

```suggestion
- Fix goto-definition for `float` and `complex` in type annotation positions ([#21388](https://github.com/astral-sh/ruff/pull/21388))
```

---

_Review comment by @carljm on `CHANGELOG.md`:60 on 2025-11-18 19:31_

Without looking at the PR, I'm not clear from this description what this actually does

---

_@carljm approved on 2025-11-18 19:32_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:55 on 2025-11-18 19:33_

```suggestion
- Only suggest the `import` keyword in autocompletions for `from <name> <CURSOR>` statements ([#21291](https://github.com/astral-sh/ruff/pull/21291))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:56 on 2025-11-18 19:33_

```suggestion
- Suppress completion suggestions following `as` tokens ([#21460](https://github.com/astral-sh/ruff/pull/21460))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:57 on 2025-11-18 19:33_

```suggestion
- Suppress invalid suggestions in `import` statements ([#21484](https://github.com/astral-sh/ruff/pull/21484))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:58 on 2025-11-18 19:34_

```suggestion
- Improve [semantic token](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_semanticTokens) classification for names ([#21399](https://github.com/astral-sh/ruff/pull/21399))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:59 on 2025-11-18 19:34_

```suggestion
- Classify parameter declarations as definitions when computing semantic tokens ([#21420](https://github.com/astral-sh/ruff/pull/21420))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:60 on 2025-11-18 19:35_

```suggestion
- Enable goto-definition when double-clicking on an inlay hint ([#20349](https://github.com/astral-sh/ruff/pull/20349))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:61 on 2025-11-18 19:35_

```suggestion
- Elide redundant inlay hints for function arguments ([#21365](https://github.com/astral-sh/ruff/pull/21365))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:63 on 2025-11-18 19:35_

```suggestion
- Supress some trivial inlay hints for expressions ([#21367](https://github.com/astral-sh/ruff/pull/21367), [#21368](https://github.com/astral-sh/ruff/pull/21368))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:68 on 2025-11-18 19:36_

this second one is actually a followup for the "subscript assignment diagnostics" bullet lower down (and I might group the two PRs into one bullet)

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:73 on 2025-11-18 19:38_

I'd move this one to the `Type inference` section -- we just didn't understand this pattern previously

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:78 on 2025-11-18 19:38_

I think we usually omit docs improvements from the changelog. But the second one I think deserves to be included in the `Diagnostics` section!

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:82 on 2025-11-18 19:38_

```suggestion
- Cache computation of dataclass/NamedTuple/TypedDict fields ([#21512](https://github.com/astral-sh/ruff/pull/21512))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:84 on 2025-11-18 19:39_

```suggestion
- Reduce memory allocations for string-literal types ([#21497](https://github.com/astral-sh/ruff/pull/21497))
```

---

_@AlexWaygood approved on 2025-11-18 19:39_

woah, so many features!!

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:45 on 2025-11-18 19:48_

sorry, my typo

```suggestion
- Sync vendored typeshed stubs ([#21466](https://github.com/astral-sh/ruff/pull/21466)). [Typeshed diff](https://github.com/python/typeshed/compare/bf7214784877c52638844c065360d4814fae4c65...f8cdc0bd526301e873cd952eb0d457bdf2554e57)
```

---

_@AlexWaygood reviewed on 2025-11-18 19:48_

---

_Review comment by @Gankra on `CHANGELOG.md`:45 on 2025-11-18 19:53_

Yeah it was easier to accept it and fixup locally haha

---

_@Gankra reviewed on 2025-11-18 19:53_

---

_Merged by @Gankra on 2025-11-18 19:56_

---

_Closed by @Gankra on 2025-11-18 19:56_

---

_Branch deleted on 2025-11-18 19:56_

---

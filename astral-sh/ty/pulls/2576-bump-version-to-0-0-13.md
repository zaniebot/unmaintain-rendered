```yaml
number: 2576
title: Bump version to 0.0.13
type: pull_request
state: open
author: MichaReiser
labels:
  - release
assignees: []
base: main
head: micha/bump-0.0.13
created_at: 2026-01-21T10:51:31Z
updated_at: 2026-01-21T11:04:03Z
url: https://github.com/astral-sh/ty/pull/2576
synced_at: 2026-01-21T11:58:27Z
```

# Bump version to 0.0.13

---

_@MichaReiser_

_No description provided._

---

_Label `release` added by @MichaReiser on 2026-01-21 10:51_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2026-01-21 10:57_

```suggestion
- Make special cases for subscript inference exhaustive, ensuring that the special casing for tuple subscripts is applied when a union of tuples or an alias to a tuple type is subscripted ([#22035](https://github.com/astral-sh/ruff/pull/22035))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:17 on 2026-01-21 10:57_

```suggestion
- Improve completion suggestions inside class definitions ([#22571](https://github.com/astral-sh/ruff/pull/22571))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2026-01-21 10:57_

```suggestion
- Remove completion suggestions for redundant re-exports that share the same top-most module ([#22581](https://github.com/astral-sh/ruff/pull/22581))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2026-01-21 10:58_

```suggestion
- Fix the return type for synthesized `NamedTuple.__new__` methods ([#22625](https://github.com/astral-sh/ruff/pull/22625))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:31 on 2026-01-21 10:59_

```suggestion
- Emit diagnostics for `NamedTuple`, `TypedDict`, `Enum` or `Protocol` classes decorated with `@dataclass` ([#22672](https://github.com/astral-sh/ruff/pull/22672))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2026-01-21 10:59_

```suggestion
- Emit `invalid-type-form` diagnostics for stringified annotations where the quoted expression is invalid ([#22752](https://github.com/astral-sh/ruff/pull/22752))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2026-01-21 10:59_

```suggestion
- Infer the implicit type of `cls` in `__new__` methods ([#22584](https://github.com/astral-sh/ruff/pull/22584))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:39 on 2026-01-21 11:00_

```suggestion
- Add bidirectional inference for comprehensions ([#22564](https://github.com/astral-sh/ruff/pull/22564))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:41 on 2026-01-21 11:00_

```suggestion
- Add right-hand-side narrowing for `if Foo is type(x)` expressions ([#22608](https://github.com/astral-sh/ruff/pull/22608))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:42 on 2026-01-21 11:00_

```suggestion
- Add simple syntactic validation for the right-hand side of PEP-613 type aliases ([#22652](https://github.com/astral-sh/ruff/pull/22652))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:43 on 2026-01-21 11:01_

```suggestion
- Add support for passing `typename` and `field_names` by keyword argument to `collections.namedtuple()` calls ([#22660](https://github.com/astral-sh/ruff/pull/22660))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:48 on 2026-01-21 11:01_

```suggestion
- Fix unary operators on `NewType`s of `float` or `complex` ([#22605](https://github.com/astral-sh/ruff/pull/22605))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:58 on 2026-01-21 11:02_

```suggestion
- Group `type[]` elements together when displaying union types ([#22592](https://github.com/astral-sh/ruff/pull/22592))
```

---

_@AlexWaygood approved on 2026-01-21 11:02_

---

_Comment by @AlexWaygood on 2026-01-21 11:02_

Thank you!

---

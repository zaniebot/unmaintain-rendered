```yaml
number: 1205
title: Bump version to 0.0.1-alpha.21
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: alpha-21
created_at: 2025-09-18T16:19:12Z
updated_at: 2025-09-19T06:42:24Z
url: https://github.com/astral-sh/ty/pull/1205
synced_at: 2026-01-12T15:54:27Z
```

# Bump version to 0.0.1-alpha.21

---

_@MichaReiser_

_No description provided._

---

_Label `release` added by @MichaReiser on 2025-09-18 16:19_

---

_Comment by @MichaReiser on 2025-09-18 16:28_

It's a bit late here already. Which is why I probably won't do the actual release today but I'll pick this up first thing tomorrow morning. I'd be great if I can get a review by then.

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2025-09-18 17:24_

```suggestion
- Fix inference of constructor calls to generic classes that have explicitly annotated `self` parameters in their `__init__` methods ([#20325](https://github.com/astral-sh/ruff/pull/20325))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2025-09-18 17:26_

```suggestion
- Add autocomplete suggestions for unimported symbols ([#20207](https://github.com/astral-sh/ruff/pull/20207), [#20439](https://github.com/astral-sh/ruff/pull/20439))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:14 on 2025-09-18 17:27_

```suggestion
- Include generated `NamedTuple` methods such as `_make`, `_asdict` and `_replace` in autocomplete suggestions ([#20356](https://github.com/astral-sh/ruff/pull/20356))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:18 on 2025-09-18 17:28_

```suggestion
- Automatically add `python/` to `environment.root` if a `python/` folder exists in the root of a repository ([#20263](https://github.com/astral-sh/ruff/pull/20263))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:28 on 2025-09-18 17:29_

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2025-09-18 17:29_

```suggestion
- Add support for generic [PEP-695 type aliases](https://peps.python.org/pep-0695/#generic-type-alias) ([#20219](https://github.com/astral-sh/ruff/pull/20219))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:36 on 2025-09-18 17:30_

```suggestion
- Bind `Self` type variables to the method, not the class ([#20366](https://github.com/astral-sh/ruff/pull/20366))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:44 on 2025-09-18 17:31_

```suggestion
- Make `TypeIs` invariant in its type argument ([#20428](https://github.com/astral-sh/ruff/pull/20428))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:45 on 2025-09-18 17:31_

```suggestion
- Narrow specialized generics using `isinstance()` ([#20256](https://github.com/astral-sh/ruff/pull/20256))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:46 on 2025-09-18 17:32_

I don't think this one has any user-facing impact yet (it just prepares the way for some other changes that will have user-facing impact)

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:49 on 2025-09-18 17:33_

Per @sharkdp in https://github.com/astral-sh/ruff/pull/20364#issue-3410422112:

> It has no observable effect, apparently.

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:50 on 2025-09-18 17:34_

```suggestion
- Overload evaluation: retry parameter matching for argument type expansion ([#20153](https://github.com/astral-sh/ruff/pull/20153))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:52 on 2025-09-18 17:34_

```suggestion
- Support "legacy" `typing.Self` in combination with [PEP-695](https://peps.python.org/pep-0695) generic contexts ([#20304](https://github.com/astral-sh/ruff/pull/20304))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:55 on 2025-09-18 17:34_

```suggestion
- `TypedDict`: Add support for `typing.ReadOnly` ([#20241](https://github.com/astral-sh/ruff/pull/20241))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:56 on 2025-09-18 17:35_

```suggestion
- Detect syntax errors stemming from `yield from` expressions inside async functions ([#20051](https://github.com/astral-sh/ruff/pull/20051))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:61 on 2025-09-18 17:36_

```suggestion
- Infer more precise types for the `name` and `value` properties on enum members ([#20311](https://github.com/astral-sh/ruff/pull/20311))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:63 on 2025-09-18 17:36_

```suggestion
- Improve type narrowing in situations involving nested functions ([#19932](https://github.com/astral-sh/ruff/pull/19932))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:64 on 2025-09-18 17:37_

```suggestion
- Fix many "too many cycle iterations" panics concerning recursive type aliases and/or recursive generics ([#20359](https://github.com/astral-sh/ruff/pull/20359))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:65 on 2025-09-18 17:38_

```suggestion
- Fix stack overflow involving subtype checks for recursive type aliases ([#20259](https://github.com/astral-sh/ruff/pull/20259))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:66 on 2025-09-18 17:38_

```suggestion
- Support type aliases in binary comparison inference ([#20445](https://github.com/astral-sh/ruff/pull/20445))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:67 on 2025-09-18 17:39_

```suggestion
- Fix panic when inferring the type of an infinitely-nested-tuple implicit instance attribute ([#20333](https://github.com/astral-sh/ruff/pull/20333))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:68 on 2025-09-18 17:41_

```suggestion
- Sync vendored typeshed stubs ([#20394](https://github.com/astral-sh/ruff/pull/20394)). [Typeshed diff](https://github.com/python/typeshed/compare/2480d7e7c74493a024eaf254c5d2c6f452c80ee2...47dbbd6c914a5190d54bc5bd498d1e6633d97db2)
```

---

_@AlexWaygood approved on 2025-09-18 17:43_

Wow, so many features in this release!

I think some of the fixes for panics could be moved from the `Typing semantics and features` to the `Bugfixes` section

---

_Merged by @MichaReiser on 2025-09-19 06:42_

---

_Closed by @MichaReiser on 2025-09-19 06:42_

---

_Branch deleted on 2025-09-19 06:42_

---

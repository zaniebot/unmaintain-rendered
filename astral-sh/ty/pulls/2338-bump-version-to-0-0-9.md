```yaml
number: 2338
title: Bump version to 0.0.9
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/0.0.9
created_at: 2026-01-05T09:45:24Z
updated_at: 2026-01-05T12:02:42Z
url: https://github.com/astral-sh/ty/pull/2338
synced_at: 2026-01-12T15:54:28Z
```

# Bump version to 0.0.9

---

_@MichaReiser_

_No description provided._

---

_Label `release` added by @MichaReiser on 2026-01-05 09:45_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:30 on 2026-01-05 09:55_

We usually add the typeshed diff here [ref](https://github.com/astral-sh/ty/blob/d63b0860f3355db6e0a441f19a308d1128712f77/CHANGELOG.md#005) which I think is just going from the previous commit of the first PR to the latest commit.

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:10 on 2026-01-05 09:56_

```suggestion
- Fix match exhaustiveness for `<enum> | None` unions ([#22290](https://github.com/astral-sh/ruff/pull/22290))
```

---

_@dhruvmanila approved on 2026-01-05 09:56_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2026-01-05 11:04_

```suggestion
- Emit a diagnostic if a class decorator is not a callable accepting a type ([#22375](https://github.com/astral-sh/ruff/pull/22375))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:10 on 2026-01-05 11:04_

```suggestion
- Fix exhaustiveness inference for unions that include enums ([#22290](https://github.com/astral-sh/ruff/pull/22290))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:16 on 2026-01-05 11:06_

```suggestion
- Don't expand type aliases via type mappings unless necessary. This means that the displayed signature of a bound methods will no longer eagerly expand type aliases into their aliased types ([#22241](https://github.com/astral-sh/ruff/pull/22241))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:18 on 2026-01-05 11:07_

This isn't a very user-friendly description but I'm not confident enough I understand what this PR did, so I don't know what to suggest instead 🙃

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2026-01-05 11:07_

```suggestion
- Narrow tagged unions of `TypedDict`s in `match` statements ([#22299](https://github.com/astral-sh/ruff/pull/22299))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:20 on 2026-01-05 11:08_

```suggestion
- Teach bidirectional inference about subtyping. This allows `x` to be inferred as `list[int]` for `x: Iterable[int] = [42]` ([#21930](https://github.com/astral-sh/ruff/pull/21930))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:21 on 2026-01-05 11:09_

```suggestion
- Support narrowing for tagged unions of tuples where one element of the tuple is a `Literal` type ([#22303](https://github.com/astral-sh/ruff/pull/22303))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:25 on 2026-01-05 11:09_

```suggestion
- Add autocomplete suggestions for keyword arguments in `class` statements ([#22110](https://github.com/astral-sh/ruff/pull/22110))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2026-01-05 11:09_

```suggestion
- Avoid showing misleading inlay hint for unpacked tuple arguments ([#22286](https://github.com/astral-sh/ruff/pull/22286))
```

---

_@AlexWaygood approved on 2026-01-05 11:09_

---

_Merged by @MichaReiser on 2026-01-05 11:15_

---

_Closed by @MichaReiser on 2026-01-05 11:15_

---

_Branch deleted on 2026-01-05 11:15_

---

```yaml
number: 2194
title: Bump version to 0.0.6
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2025-12-23T21:47:48Z
updated_at: 2025-12-23T22:00:41Z
url: https://github.com/astral-sh/ty/pull/2194
synced_at: 2026-01-12T15:54:28Z
```

# Bump version to 0.0.6

---

_@charliermarsh_

_No description provided._

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2025-12-23 21:49_

Should gesture at the fact that this fixed a panic, I think 

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:10 on 2025-12-23 21:51_

```suggestion
- Support `type[T]` where `T` is a type alias to a union of types ([#22115](https://github.com/astral-sh/ruff/pull/22115))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2025-12-23 21:52_

```suggestion
- Support `==` narrowing for tuples in unions with disjoint types ([#22129](https://github.com/astral-sh/ruff/pull/22129))
```

---

_@charliermarsh reviewed on 2025-12-23 21:52_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:32 on 2025-12-23 21:52_

Should arguably go in configuration section.

---

_@charliermarsh reviewed on 2025-12-23 21:52_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:37 on 2025-12-23 21:52_

Oh this is a bug fix.

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:20 on 2025-12-23 21:52_

```suggestion
- Use Markdown for completions documentation if the LSP client supports it ([#21752](https://github.com/astral-sh/ruff/pull/21752))
```

---

_@charliermarsh reviewed on 2025-12-23 21:52_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:37 on 2025-12-23 21:52_

These should all go in like... a "Typing" section.

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:24 on 2025-12-23 21:53_

```suggestion
- Abort printing diagnostics when pressing `Ctrl+C` ([#22083](https://github.com/astral-sh/ruff/pull/22083))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:35 on 2025-12-23 21:53_

```suggestion
- Synthesize a precise `_fields` attribute for NamedTuples ([#22163](https://github.com/astral-sh/ruff/pull/22163))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:36 on 2025-12-23 21:53_

```suggestion
- Synthesize a precise `_replace` method for NamedTuples ([#22153](https://github.com/astral-sh/ruff/pull/22153))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:37 on 2025-12-23 21:53_

```suggestion
- Narrow "tagged unions" of `TypedDict`s ([#22104](https://github.com/astral-sh/ruff/pull/22104))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2025-12-23 21:55_

Should gesture at the fact that this fixed a panic 

---

_@AlexWaygood approved on 2025-12-23 21:55_

---

_Review comment by @carljm on `CHANGELOG.md`:37 on 2025-12-23 21:56_

Sounds good to me -- let's just rename this section?

---

_@charliermarsh reviewed on 2025-12-23 21:57_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:13 on 2025-12-23 21:57_

```suggestion
- Fix panic from unstable union-type ordering in fixed-point iteration ([#22070](https://github.com/astral-sh/ruff/pull/22070))
```

---

_@charliermarsh reviewed on 2025-12-23 21:57_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:9 on 2025-12-23 21:57_

```suggestion
- FIx panic from unexpanded type aliases in implicit tuple aliases ([#22015](https://github.com/astral-sh/ruff/pull/22015))
```

---

_@carljm approved on 2025-12-23 21:59_

LGTM, thank you

---

_@carljm reviewed on 2025-12-23 22:00_

---

_Review comment by @carljm on `CHANGELOG.md`:9 on 2025-12-23 22:00_

```suggestion
- Fix panic from expanding type aliases in implicit tuple aliases ([#22015](https://github.com/astral-sh/ruff/pull/22015))
```

---

_Merged by @charliermarsh on 2025-12-23 22:00_

---

_Closed by @charliermarsh on 2025-12-23 22:00_

---

_Branch deleted on 2025-12-23 22:00_

---

```yaml
number: 1868
title: Bump version to 0.0.1-alpha.34
type: pull_request
state: merged
author: BurntSushi
labels:
  - release
assignees: []
merged: true
base: main
head: ag/bump-0-0-1a34
created_at: 2025-12-12T17:29:56Z
updated_at: 2025-12-12T18:11:21Z
url: https://github.com/astral-sh/ty/pull/1868
synced_at: 2026-01-10T02:34:11Z
```

# Bump version to 0.0.1-alpha.34

---

_Pull request opened by @BurntSushi on 2025-12-12 17:29_

_No description provided._

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-12 17:30_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-12 17:30_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-12 17:30_

---

_Label `release` added by @BurntSushi on 2025-12-12 17:30_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:10 on 2025-12-12 17:32_

I'd be inclined to move these two to the "LSP Server" section

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2025-12-12 17:34_

```suggestion
- Improve solving of a type variable with an upper bound when that type variable appears as one element in a union type ([#21893](https://github.com/astral-sh/ruff/pull/21893))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:12 on 2025-12-12 17:35_

```suggestion
- Accurately emulate runtime semantics for `kw_only=True` dataclasses such that only fields declared in the immediate class body are understood as being keyword-only ([#21820](https://github.com/astral-sh/ruff/pull/21820))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:14 on 2025-12-12 17:36_

```suggestion
- Fix logic used to determine whether two `@final` instance types are disjoint ([#21769](https://github.com/astral-sh/ruff/pull/21769))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:15 on 2025-12-12 17:36_

```suggestion
- Fix logic used to determine whether two `@final` `type[]` types are disjoint ([#21770](https://github.com/astral-sh/ruff/pull/21770))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:17 on 2025-12-12 17:38_

```suggestion
- Fix false-positive diagnostics that could arise when analysing cyclic types ([#21910](https://github.com/astral-sh/ruff/pull/21910), [#21909](https://github.com/astral-sh/ruff/pull/21909))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:23 on 2025-12-12 17:39_

```suggestion
- Don't show on-hover tooltips for expressions with no inferred type ([#21924](https://github.com/astral-sh/ruff/pull/21924))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2025-12-12 17:40_

```suggestion
- Support checking files without extensions ([#21867](https://github.com/astral-sh/ruff/pull/21867))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:31 on 2025-12-12 17:40_

```suggestion
- Improve performance and semantics by deferring inference of all parameter and return-type annotations ([#21906](https://github.com/astral-sh/ruff/pull/21906))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2025-12-12 17:42_

```suggestion
- Infer the implicit type of the `cls` parameter in `@classmethod` method bodies ([#21685](https://github.com/astral-sh/ruff/pull/21685))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:34 on 2025-12-12 17:42_

```suggestion
- Support the implicit type of the `cls` parameter in signatures of `@classmethod` methods ([#21771](https://github.com/astral-sh/ruff/pull/21771))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:36 on 2025-12-12 17:43_

```suggestion
- Implement the [equivalence relation](https://typing.python.org/en/latest/spec/glossary.html#term-equivalent) for `TypedDict`s ([#21784](https://github.com/astral-sh/ruff/pull/21784))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:37 on 2025-12-12 17:45_

```suggestion
- Ensure that the type of the class object `C` is always considered assignable to `type[C[Unknown]]` if `C` is a generic class ([#21883](https://github.com/astral-sh/ruff/pull/21883))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2025-12-12 17:45_

```suggestion
- Improve bad specialization results and error messages ([#21840](https://github.com/astral-sh/ruff/pull/21840))
```

---

_@AlexWaygood approved on 2025-12-12 17:45_

---

_@BurntSushi reviewed on 2025-12-12 17:52_

---

_Review comment by @BurntSushi on `CHANGELOG.md`:38 on 2025-12-12 17:52_

What have you got against the `&`!?!?! Haha

---

_Comment by @BurntSushi on 2025-12-12 17:52_

Thank you @AlexWaygood! I've applied all of your suggestions.

---

_Merged by @BurntSushi on 2025-12-12 18:11_

---

_Closed by @BurntSushi on 2025-12-12 18:11_

---

_Branch deleted on 2025-12-12 18:11_

---

```yaml
number: 890
title: Bump version to 0.0.1-alpha.16
type: pull_request
state: merged
author: sharkdp
labels: []
assignees: []
merged: true
base: main
head: david/bump-0-0-1a16
created_at: 2025-07-25T13:21:15Z
updated_at: 2025-07-25T13:46:29Z
url: https://github.com/astral-sh/ty/pull/890
synced_at: 2026-01-12T15:54:27Z
```

# Bump version to 0.0.1-alpha.16

---

_@sharkdp_

Changes which I chose not to include; let me know if one of these should be added:

```
- Add warning for unknown `TY_MEMORY_REPORT` value ([#19465](https://github.com/astral-sh/ruff/pull/19465))
- Add goto definition to playground ([#19425](https://github.com/astral-sh/ruff/pull/19425))
- Added support for "go to references" in ty playground. ([#19516](https://github.com/astral-sh/ruff/pull/19516))
- Fall back to `Unknown` if no type is stored for an expression ([#19517](https://github.com/astral-sh/ruff/pull/19517))
- Make `Module` a Salsa ingredient ([#19495](https://github.com/astral-sh/ruff/pull/19495))
- Return a tuple spec from the iterator protocol ([#19496](https://github.com/astral-sh/ruff/pull/19496))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:8 on 2025-07-25 13:28_

```suggestion
- `match` statements: Fix narrowing and reachability of class patterns with arguments ([#19512](https://github.com/astral-sh/ruff/pull/19512))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2025-07-25 13:29_

```suggestion
- Fix server panics when hovering over illegal `Literal[…]` annotations with inner subscript expressions ([#19489](https://github.com/astral-sh/ruff/pull/19489))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2025-07-25 13:29_

```suggestion
- Fix server panics when hovering over invalid syntax in `Callable` annotations ([#19517](https://github.com/astral-sh/ruff/pull/19517))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:17 on 2025-07-25 13:30_

since this section is title `### Server`:

```suggestion
- Add support for "document highlights" ([#19515](https://github.com/astral-sh/ruff/pull/19515))
- Add partial support for "find references" ([#19475](https://github.com/astral-sh/ruff/pull/19475))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:18 on 2025-07-25 13:31_

```suggestion
- Prefer the runtime definition, not the stub definition, on a go-to-definition request for a class or function. Currently this is only implemented for definitions originating outside of the stdlib. ([#19471](https://github.com/astral-sh/ruff/pull/19471))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2025-07-25 13:32_

```suggestion
- Add [semantic token](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_semanticTokens) support for more identifiers ([#19473](https://github.com/astral-sh/ruff/pull/19473))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:20 on 2025-07-25 13:33_

```suggestion
- Avoid rechecking the entire project when a file in the editor is opened or closed ([#19463](https://github.com/astral-sh/ruff/pull/19463))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2025-07-25 13:33_

```suggestion
- Add support for `@warnings.deprecated` and `typing_extensions.deprecated` ([#19376](https://github.com/astral-sh/ruff/pull/19376))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:37 on 2025-07-25 13:34_

```suggestion
- Infer single-valuedness for enums deriving from `int` or `str` ([#19510](https://github.com/astral-sh/ruff/pull/19510))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:43 on 2025-07-25 13:34_

I think the final version of this PR only impacted `static_assert`, which is an internal-only function

```suggestion
```

---

_@AlexWaygood approved on 2025-07-25 13:35_

---

_Merged by @sharkdp on 2025-07-25 13:46_

---

_Closed by @sharkdp on 2025-07-25 13:46_

---

_Branch deleted on 2025-07-25 13:46_

---

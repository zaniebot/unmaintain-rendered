```yaml
number: 1785
title: Bump version to 0.0.1-alpha.32
type: pull_request
state: merged
author: oconnor663
labels:
  - release
assignees: []
merged: true
base: main
head: release_0.0.1-alpha.32
created_at: 2025-12-05T19:22:40Z
updated_at: 2025-12-09T11:07:17Z
url: https://github.com/astral-sh/ty/pull/1785
synced_at: 2026-01-10T02:34:11Z
```

# Bump version to 0.0.1-alpha.32

---

_Pull request opened by @oconnor663 on 2025-12-05 19:22_

_No description provided._

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2025-12-05 19:58_

```suggestion
- Provide auto-import completion suggestions for modules in more situations ([#21799](https://github.com/astral-sh/ruff/pull/21799))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:10 on 2025-12-05 20:01_

```suggestion
- Always register the ty server as a [rename provider](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_rename) if the LSP client doesn't support dynamic registration ([#21789](https://github.com/astral-sh/ruff/pull/21789))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2025-12-05 20:01_

```suggestion
- Support auto-import of re-exported symbols in completion suggestions ([#21779](https://github.com/astral-sh/ruff/pull/21779))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:16 on 2025-12-05 20:03_

I would just say

```suggestion
- Support `ParamSpec` ([#21445](https://github.com/astral-sh/ruff/pull/21445))
```

since our support before now wasn't really there at all, but there are still some more advanced `ParamSpec` features such as `Concatenate` that we don't yet support

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:17 on 2025-12-05 20:03_

```suggestion
- Improve the accuracy of the inferred `Callable` supertype of generic classes ([#21798](https://github.com/astral-sh/ruff/pull/21798))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2025-12-05 20:05_

```suggestion
- Fix panics on mutually recursive generic protocols by normalizing the bounds/constraints of cyclic type variables ([#21800](https://github.com/astral-sh/ruff/pull/21800))
```

---

_@AlexWaygood approved on 2025-12-05 20:05_

Let's go 🚀

---

_Renamed from "Release 0.0.1-alpha.32" to "Bump version to 0.0.1-alpha.32" by @oconnor663 on 2025-12-05 20:44_

---

_Merged by @oconnor663 on 2025-12-05 20:45_

---

_Closed by @oconnor663 on 2025-12-05 20:45_

---

_Branch deleted on 2025-12-05 20:45_

---

_Label `release` added by @dhruvmanila on 2025-12-09 11:07_

---

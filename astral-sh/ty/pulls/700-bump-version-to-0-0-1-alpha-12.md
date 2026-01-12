```yaml
number: 700
title: Bump version to 0.0.1-alpha.12
type: pull_request
state: merged
author: AlexWaygood
labels:
  - release
assignees: []
merged: true
base: main
head: alex/release
created_at: 2025-06-25T11:15:29Z
updated_at: 2025-06-25T11:37:48Z
url: https://github.com/astral-sh/ty/pull/700
synced_at: 2026-01-12T15:54:27Z
```

# Bump version to 0.0.1-alpha.12

---

_@AlexWaygood_

_No description provided._

---

_Label `release` added by @AlexWaygood on 2025-06-25 11:15_

---

_@sharkdp reviewed on 2025-06-25 11:22_

---

_Review comment by @sharkdp on `CHANGELOG.md`:13 on 2025-06-25 11:22_

This PR also adds the ability to understand pure class attributes that are assigned in `@classmethod`s. This is probably the more interesting feature, the staticmethod thing is just a bugfix (that probably didn't affect anyone). So maybe
```suggestion
- Discover implicit class attribute assignments in `@classmethod`s ([#18587](https://github.com/astral-sh/ruff/pull/18587))
```

---

_Review comment by @sharkdp on `CHANGELOG.md`:25 on 2025-06-25 11:23_

Maybe
```suggestion
- Support type inference for subscript expressions on union types ([#18846](https://github.com/astral-sh/ruff/pull/18846))
```

---

_@sharkdp reviewed on 2025-06-25 11:23_

---

_Review comment by @sharkdp on `CHANGELOG.md`:26 on 2025-06-25 11:25_

Maybe
```suggestion
- Introduce a new subtyping framework in which gradual types do participate, allowing for more advanced union type simplification ([#18799](https://github.com/astral-sh/ruff/pull/18799))
```

---

_@sharkdp reviewed on 2025-06-25 11:25_

---

_@sharkdp approved on 2025-06-25 11:25_

Thank you

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2025-06-25 11:28_

thanks -- I think it is good to mention the staticmethod change, but I agree the classmethod change is more important! I applied a variant of this in https://github.com/astral-sh/ty/pull/700/commits/12a4194260759e4cb7fab128d400ea0ea94269ab

---

_@AlexWaygood reviewed on 2025-06-25 11:28_

---

_Merged by @AlexWaygood on 2025-06-25 11:37_

---

_Closed by @AlexWaygood on 2025-06-25 11:37_

---

_Branch deleted on 2025-06-25 11:37_

---

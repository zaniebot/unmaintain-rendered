```yaml
number: 2132
title: Bump version to 0.0.5
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2025-12-20T19:18:06Z
updated_at: 2025-12-20T19:33:24Z
url: https://github.com/astral-sh/ty/pull/2132
synced_at: 2026-01-10T02:34:11Z
```

# Bump version to 0.0.5

---

_Pull request opened by @charliermarsh on 2025-12-20 19:18_

_No description provided._

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:9 on 2025-12-20 19:23_

```suggestion
- Fix debug-mode server panic when a user typed a class definition by ensuring class arguments are visited in source order for semantic tokens ([#22063](https://github.com/astral-sh/ruff/pull/22063))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:23 on 2025-12-20 19:24_

```suggestion
- Speedup bidirectional type-checking involving large unions by avoiding narrowing on non-generic calls ([#22102](https://github.com/astral-sh/ruff/pull/22102))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:25 on 2025-12-20 19:25_

```suggestion
- Simplify inferred types by avoiding storing multi-inference attempts ([#22062](https://github.com/astral-sh/ruff/pull/22062), [#22103](https://github.com/astral-sh/ruff/pull/22103))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2025-12-20 19:26_

```suggestion
- Understand that the type of `X` on an enum class will be `int` if `X` is defined using `enum.nonmember` in the class definition ([#22025](https://github.com/astral-sh/ruff/pull/22025))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:31 on 2025-12-20 19:26_

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:29 on 2025-12-20 19:27_

```suggestion
- Sync vendored typeshed stubs ([#22091](https://github.com/astral-sh/ruff/pull/22091)). [Typeshed diff](https://github.com/python/typeshed/compare/ef2b90c67e5c668b91b3ae121baf00ee5165c30b...3c2dbb1fde8e8d1d59b10161c8bf5fd06c0011cd)
```

---

_@AlexWaygood approved on 2025-12-20 19:27_

---

_Merged by @charliermarsh on 2025-12-20 19:33_

---

_Closed by @charliermarsh on 2025-12-20 19:33_

---

_Branch deleted on 2025-12-20 19:33_

---

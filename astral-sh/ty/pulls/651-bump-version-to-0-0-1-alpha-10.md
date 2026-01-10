```yaml
number: 651
title: Bump version to 0.0.1-alpha.10
type: pull_request
state: merged
author: carljm
labels:
  - release
assignees: []
merged: true
base: main
head: cjm/alpha10
created_at: 2025-06-13T20:04:51Z
updated_at: 2025-06-13T20:19:50Z
url: https://github.com/astral-sh/ty/pull/651
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1-alpha.10

---

_Pull request opened by @carljm on 2025-06-13 20:04_

_No description provided._

---

_Label `release` added by @carljm on 2025-06-13 20:04_

---

_Review requested from @AlexWaygood by @carljm on 2025-06-13 20:04_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:16 on 2025-06-13 20:10_

```suggestion
- Delay computation of 'unbound' visibility for implicit instance attributes ([#18669](https://github.com/astral-sh/ruff/pull/18669)).
  This fixes a significant performance regression on version 0.0.1-alpha.9.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:20 on 2025-06-13 20:10_

```suggestion
- Support the `del` statement. Model implicit deletion of except handler names ([#18593](https://github.com/astral-sh/ruff/pull/18593))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:21 on 2025-06-13 20:11_

Is there a user-facing impact from this yet? I'd maybe leave it out for now, until we start using it in `Type::has_relation_to()`?

```suggestion
```

---

_@AlexWaygood approved on 2025-06-13 20:12_

---

_Merged by @carljm on 2025-06-13 20:19_

---

_Closed by @carljm on 2025-06-13 20:19_

---

_Branch deleted on 2025-06-13 20:19_

---

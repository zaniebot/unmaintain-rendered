```yaml
number: 1949
title: Bump version to 0.0.1 (beta release)
type: pull_request
state: merged
author: oconnor663
labels: []
assignees: []
merged: true
base: main
head: beta_release
created_at: 2025-12-16T17:41:54Z
updated_at: 2025-12-16T19:11:08Z
url: https://github.com/astral-sh/ty/pull/1949
synced_at: 2026-01-12T15:54:28Z
```

# Bump version to 0.0.1 (beta release)

---

_@oconnor663_

_No description provided._

---

_Review requested from @carljm by @oconnor663 on 2025-12-16 17:44_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-12-16 17:44_

---

_@charliermarsh approved on 2025-12-16 18:47_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2025-12-16 18:48_

```suggestion
- Improve syntax highlighting of constants ([#22006](https://github.com/astral-sh/ruff/pull/22006))
```

---

_Review comment by @carljm on `CHANGELOG.md`:16 on 2025-12-16 18:48_

This should probably move to a separate "Performance" category -- it has no impact on type checking semantics

---

_@MichaReiser reviewed on 2025-12-16 18:50_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:16 on 2025-12-16 18:50_

This is a bit too technical, and I also wouldn't consider this a core type-checking change. 

I'd move it into an "Other" section and just list it as a checking performance improvement

---

_@carljm approved on 2025-12-16 18:50_

Let's go!

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:12 on 2025-12-16 18:50_

this isn't just a server change, it also has a big impact on our diagnostics!

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:17 on 2025-12-16 18:51_

```suggestion
- Infer precise types for `isinstance(…)` calls involving type variables ([#21999](https://github.com/astral-sh/ruff/pull/21999))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:18 on 2025-12-16 18:51_

```suggestion
- Infer `TypeVar` specializations for `Callable` types ([#21551](https://github.com/astral-sh/ruff/pull/21551))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2025-12-16 18:52_

if this is the only "bugfix" this release, I'd lean towards merging it into the "Core type checking" section

---

_@AlexWaygood reviewed on 2025-12-16 18:52_

---

_Merged by @oconnor663 on 2025-12-16 19:11_

---

_Closed by @oconnor663 on 2025-12-16 19:11_

---

_Branch deleted on 2025-12-16 19:11_

---

```yaml
number: 2709
title: Bump version to 0.1.25
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - no-build
assignees: []
merged: true
base: main
head: zb/bump
created_at: 2024-03-28T14:37:11Z
updated_at: 2024-03-28T15:17:53Z
url: https://github.com/astral-sh/uv/pull/2709
synced_at: 2026-01-12T16:05:11Z
```

# Bump version to 0.1.25

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-03-28 14:37_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:14 on 2024-03-28 14:38_

AKA?

```suggestion
- Use PEP 517 to extract dynamic `pyproject.toml` metadata ([#2633](https://github.com/astral-sh/uv/pull/2633))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:23 on 2024-03-28 14:41_

```suggestion
- Disallow `pyproject.toml` from `pip uninstall -r` ([#2663](https://github.com/astral-sh/uv/pull/2663))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2024-03-28 14:41_

```suggestion
- Do not force-recompile `.pyc` files ([#2642](https://github.com/astral-sh/uv/pull/2642))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:31 on 2024-03-28 14:42_

```suggestion
- Read package metadata from `pyproject.toml` when it is statically defined ([#2676](https://github.com/astral-sh/uv/pull/2676))
```

---

_@AlexWaygood approved on 2024-03-28 14:42_

---

_Label `no-build` added by @zanieb on 2024-03-28 14:53_

---

_@charliermarsh approved on 2024-03-28 15:07_

I was initially hoping to include https://github.com/astral-sh/uv/pull/2702 but probably best to put that off for the next release anyway.

---

_Merged by @zanieb on 2024-03-28 15:17_

---

_Closed by @zanieb on 2024-03-28 15:17_

---

_Branch deleted on 2024-03-28 15:17_

---

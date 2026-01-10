---
number: 3956
title: PT001 implementation from ruff is opposite of PT001 from flake8-pytest-style
type: issue
state: closed
author: ssbarnea
labels: []
assignees: []
created_at: 2023-04-13T08:40:44Z
updated_at: 2023-04-13T08:44:11Z
url: https://github.com/astral-sh/ruff/issues/3956
synced_at: 2026-01-10T01:22:42Z
---

# PT001 implementation from ruff is opposite of PT001 from flake8-pytest-style

---

_Issue opened by @ssbarnea on 2023-04-13 08:40_

Apparently ruff implementation of PT001 is the exact opposite of flake8 one. Ruff auto-fix is breaking flake8 rule. See https://github.com/m-burst/flake8-pytest-style/blob/master/docs/rules/PT001.md

```
PT001 use @pytest.fixture over @pytest.fixture()
```

What should we do? Can we agree on a recommendation and modify either ruff or flake8-pytest-style default settings to avoid this conflict?

Related:
- https://github.com/m-burst/flake8-pytest-style/issues/4

---

_Comment by @ssbarnea on 2023-04-13 08:44_

Oops, I discovered that was caused by `pytest-fixture-no-parentheses = true` being inside my flake8 conflict, so a false bug report. I removed the customization and now they match.

---

_Closed by @ssbarnea on 2023-04-13 08:44_

---

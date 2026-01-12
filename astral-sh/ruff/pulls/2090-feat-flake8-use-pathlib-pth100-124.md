```yaml
number: 2090
title: "feat: flake8-use-pathlib PTH100-124"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: feat/flake8-use-pathlib
created_at: 2023-01-22T18:50:33Z
updated_at: 2023-01-30T11:52:49Z
url: https://github.com/astral-sh/ruff/pull/2090
synced_at: 2026-01-12T15:55:07Z
```

# feat: flake8-use-pathlib PTH100-124

---

_@sbrugman_

See: https://github.com/charliermarsh/ruff/issues/2060

For review: The checking if rules are enabled, and the mapping from call path => violation could probably be simplified, but I don't yet see how - suggestions welcome.

---

_@charliermarsh reviewed on 2023-01-22 18:53_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:2571 on 2023-01-22 18:53_

This is basically the pattern we use elsewhere. It's not great, but no major suggestions on how to change it right now, so all good.

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/helpers.rs`:104 on 2023-01-22 18:55_

Is it possible to get rid of all the `if checker.settings.rules.enabled` in the above `map` call, then here, create the `Diagnostic`, and do `settings.rules.enabled(diagnostic.kind.rule())` prior to pushing?

---

_@charliermarsh reviewed on 2023-01-22 18:55_

---

_@sbrugman reviewed on 2023-01-22 19:04_

---

_Review comment by @sbrugman on `src/rules/flake8_use_pathlib/helpers.rs`:104 on 2023-01-22 19:04_

Makes sense, pushed the changes. We already checked that any of them is enabled. We might even be able to get rid of `OsCall` altogether in a similar way

---

_@charliermarsh reviewed on 2023-01-22 19:21_

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/violations.rs`:24 on 2023-01-22 19:21_

Should this be `.chmod()` at the end, with parens?

---

_@charliermarsh reviewed on 2023-01-22 19:21_

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/violations.rs`:189 on 2023-01-22 19:21_

Is `.group` missing parens?

---

_@charliermarsh reviewed on 2023-01-22 19:22_

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/helpers.rs`:104 on 2023-01-22 19:22_

Yeah you could probably get rid of `OsCall` here?

---

_@charliermarsh reviewed on 2023-01-22 19:23_

---

_Review comment by @charliermarsh on `src/rules/flake8_use_pathlib/violations.rs`:24 on 2023-01-22 19:23_

(Genuinely asking, since I know some of these are actually properties, like `.parent`.)

---

_@sbrugman reviewed on 2023-01-22 19:26_

---

_Review comment by @sbrugman on `src/rules/flake8_use_pathlib/violations.rs`:24 on 2023-01-22 19:26_

Yes, was a mistake

---

_Renamed from "feat: flake8-use-pathlib PTH100-122" to "feat: flake8-use-pathlib PTH100-123" by @sbrugman on 2023-01-22 19:28_

---

_@sbrugman reviewed on 2023-01-22 19:28_

---

_Review comment by @sbrugman on `src/rules/flake8_use_pathlib/violations.rs`:189 on 2023-01-22 19:28_

Fixed

---

_Renamed from "feat: flake8-use-pathlib PTH100-123" to "feat: flake8-use-pathlib PTH100-124" by @sbrugman on 2023-01-22 20:14_

---

_Merged by @charliermarsh on 2023-01-22 20:17_

---

_Closed by @charliermarsh on 2023-01-22 20:17_

---

_@sbrugman reviewed on 2023-01-22 20:26_

---

_Review comment by @sbrugman on `src/rules/flake8_use_pathlib/helpers.rs`:104 on 2023-01-22 20:26_

Fixed in https://github.com/charliermarsh/ruff/pull/2091

---

_Branch deleted on 2023-01-30 11:52_

---

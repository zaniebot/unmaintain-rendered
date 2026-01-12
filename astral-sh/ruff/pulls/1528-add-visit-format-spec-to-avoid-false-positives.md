```yaml
number: 1528
title: "Add `visit_format_spec` to avoid false positives for F541 in f-string format specifier"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: add-visit_format_spec
created_at: 2023-01-01T09:54:08Z
updated_at: 2023-01-01T18:03:32Z
url: https://github.com/astral-sh/ruff/pull/1528
synced_at: 2026-01-12T05:36:31Z
```

# Add `visit_format_spec` to avoid false positives for F541 in f-string format specifier

---

_Pull request opened by @harupy on 2023-01-01 09:54_

See #1514 for details.

---

_@harupy reviewed on 2023-01-01 10:09_

---

_Review comment by @harupy on `resources/test/fixtures/pyflakes/F821_0.py`:93 on 2023-01-01 10:09_

Test cases to ensure we do visit child nodes in `format_spec`.

---

_@harupy reviewed on 2023-01-01 10:13_

---

_Review comment by @harupy on `src/ast/visitor.rs`:43 on 2023-01-01 10:13_

This is a special method to separate normal `JoinedStr` and format-spec `JoinedStr`.

---

_Comment by @charliermarsh on 2023-01-01 18:03_

Nice work.

---

_Merged by @charliermarsh on 2023-01-01 18:03_

---

_Closed by @charliermarsh on 2023-01-01 18:03_

---

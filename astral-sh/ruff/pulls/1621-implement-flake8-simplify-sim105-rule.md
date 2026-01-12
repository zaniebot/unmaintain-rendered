```yaml
number: 1621
title: Implement flake8-simplify SIM105 rule
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: flake8-simplify-sim105
created_at: 2023-01-04T03:11:16Z
updated_at: 2023-01-04T13:13:26Z
url: https://github.com/astral-sh/ruff/pull/1621
synced_at: 2026-01-12T05:36:32Z
```

# Implement flake8-simplify SIM105 rule

---

_Pull request opened by @messense on 2023-01-04 03:11_

Ref #998

---

_Comment by @messense on 2023-01-04 03:12_

Not sure how to implement autofix and proper error span yet, still learning😅

---

_@charliermarsh reviewed on 2023-01-04 04:50_

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/use_contextlib_suppress.rs`:32 on 2023-01-04 04:50_

This might be a hard one to autofix right now because it depends on `contextlib` being imported already.

---

_Marked ready for review by @messense on 2023-01-04 05:17_

---

_Comment by @charliermarsh on 2023-01-04 13:10_

Thank you!

---

_Merged by @charliermarsh on 2023-01-04 13:10_

---

_Closed by @charliermarsh on 2023-01-04 13:11_

---

_Branch deleted on 2023-01-04 13:13_

---

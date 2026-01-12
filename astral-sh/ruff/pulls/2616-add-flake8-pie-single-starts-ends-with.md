```yaml
number: 2616
title: Add flake8-pie single_starts_ends_with
type: pull_request
state: merged
author: sbdchd
labels:
  - rule
assignees: []
merged: true
base: main
head: flake8-pie-810
created_at: 2023-02-07T01:32:03Z
updated_at: 2023-02-07T02:27:57Z
url: https://github.com/astral-sh/ruff/pull/2616
synced_at: 2026-01-12T04:52:00Z
```

# Add flake8-pie single_starts_ends_with

---

_Pull request opened by @sbdchd on 2023-02-07 01:32_

Check for things like:

```py
url.startswith("http") or url.startswith("ftp")
```

which can be simplified to:

```py
url.startswith(("http", "ftp"))
```

rel: https://github.com/charliermarsh/ruff/issues/1543

---

_Label `rule` added by @charliermarsh on 2023-02-07 02:22_

---

_Merged by @charliermarsh on 2023-02-07 02:22_

---

_Closed by @charliermarsh on 2023-02-07 02:22_

---

_Comment by @charliermarsh on 2023-02-07 02:22_

Thanks!

---

_@sbdchd reviewed on 2023-02-07 02:27_

---

_Review comment by @sbdchd on `crates/ruff/src/rules/flake8_pie/rules.rs`:343 on 2023-02-07 02:27_

very clean!

---

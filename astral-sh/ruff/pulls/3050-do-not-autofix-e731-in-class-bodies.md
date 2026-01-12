```yaml
number: 3050
title: "Do not autofix `E731` in class bodies"
type: pull_request
state: merged
author: JoshKarpel
labels:
  - bug
assignees: []
merged: true
base: main
head: no-E731-in-class-bodies
created_at: 2023-02-20T05:22:42Z
updated_at: 2023-02-20T17:56:36Z
url: https://github.com/astral-sh/ruff/pull/3050
synced_at: 2026-01-12T15:55:12Z
```

# Do not autofix `E731` in class bodies

---

_@JoshKarpel_

Resolves #3046 

I tried to mirror how other not-always-fixable violations work, and I also tried to flatten out the conditional around the autofix itself.

---

_Review comment by @youknowone on `crates/ruff/src/rules/pycodestyle/rules/lambda_assignment.rs`:33 on 2023-02-20 11:41_

maybe you like this form
```suggestion
        self.fixable
            .then_some(|v| format!("Rewrite `{}` as a `def`", v.name))
```

---

_@youknowone reviewed on 2023-02-20 11:43_

---

_Review comment by @JoshKarpel on `crates/ruff/src/rules/pycodestyle/rules/lambda_assignment.rs`:33 on 2023-02-20 14:57_

I do!

---

_@JoshKarpel reviewed on 2023-02-20 14:57_

---

_Merged by @charliermarsh on 2023-02-20 17:38_

---

_Closed by @charliermarsh on 2023-02-20 17:38_

---

_Comment by @charliermarsh on 2023-02-20 17:38_

Looks great, no notes -- thank you so much!

---

_Label `bug` added by @charliermarsh on 2023-02-20 17:38_

---

_Branch deleted on 2023-02-20 17:56_

---

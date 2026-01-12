```yaml
number: 2141
title: Convert confusable violations to struct fields
type: pull_request
state: merged
author: sladyn98
labels: []
assignees: []
merged: true
base: main
head: struct
created_at: 2023-01-24T19:44:33Z
updated_at: 2023-01-27T16:31:40Z
url: https://github.com/astral-sh/ruff/pull/2141
synced_at: 2026-01-12T04:52:00Z
```

# Convert confusable violations to struct fields

---

_Pull request opened by @sladyn98 on 2023-01-24 19:44_

This PR adds converts confusable violations to struct fields.

---

_@charliermarsh reviewed on 2023-01-25 18:39_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:4475 on 2023-01-25 18:39_

Unfortunately this won't compile. Have you been able to set up a local development environment for Rust?

---

_@sladyn98 reviewed on 2023-01-26 00:14_

---

_Review comment by @sladyn98 on `src/checkers/ast.rs`:4475 on 2023-01-26 00:14_

@charliermarsh Yeah fixed the compile errors.

---

_Review requested from @charliermarsh by @sladyn98 on 2023-01-27 07:19_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:4475 on 2023-01-27 16:27_

I'll go ahead and do it in this case, but for the future, you'll need to run `cargo +nightly fmt` to get the code in good shape for CI.

---

_@charliermarsh reviewed on 2023-01-27 16:27_

---

_Merged by @charliermarsh on 2023-01-27 16:31_

---

_Closed by @charliermarsh on 2023-01-27 16:31_

---

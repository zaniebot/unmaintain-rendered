```yaml
number: 298
title: Automatically remove unused imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cst-import-autofix
created_at: 2022-10-02T04:48:01Z
updated_at: 2022-10-03T18:08:17Z
url: https://github.com/astral-sh/ruff/pull/298
synced_at: 2026-01-12T05:48:45Z
```

# Automatically remove unused imports

---

_Pull request opened by @charliermarsh on 2022-10-02 04:48_

Resolves #247.

---

_Marked ready for review by @charliermarsh on 2022-10-02 04:48_

---

_@Seamooo reviewed on 2022-10-02 13:43_

---

_Review comment by @Seamooo on `src/ast/types.rs`:64 on 2022-10-02 13:43_

very exciting!

---

_@charliermarsh reviewed on 2022-10-02 13:47_

---

_Review comment by @charliermarsh on `src/ast/types.rs`:64 on 2022-10-02 13:47_

I don't know if the spans will always be "correct" (that is: I can't guarantee they'll match the CPython semantics), so we may need to iterate on them a bit.

---

_@charliermarsh reviewed on 2022-10-02 23:57_

---

_Review comment by @charliermarsh on `src/ast/types.rs`:64 on 2022-10-02 23:57_

(These fields are now available on `Message` and `Check`.)

---

_Comment by @charliermarsh on 2022-10-03 01:27_

This works well but still requires a lot of bookkeeping around the AST manipulation, even with the help of LibCST.

---

_Merged by @charliermarsh on 2022-10-03 18:08_

---

_Closed by @charliermarsh on 2022-10-03 18:08_

---

_Branch deleted on 2022-10-03 18:08_

---

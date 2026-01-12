```yaml
number: 297
title: Enable LibCST-based autofixing for SPR001
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cst-autofix
created_at: 2022-10-02T02:25:39Z
updated_at: 2022-10-02T23:58:14Z
url: https://github.com/astral-sh/ruff/pull/297
synced_at: 2026-01-12T05:48:45Z
```

# Enable LibCST-based autofixing for SPR001

---

_Pull request opened by @charliermarsh on 2022-10-02 02:25_

# Summary

This PR demonstrates use of LibCST to autofix a specific lint error (rather than using it as an end-to-end parser solution).


---

_Comment by @charliermarsh on 2022-10-02 02:26_

\cc @sobolevn 

---

_@charliermarsh reviewed on 2022-10-02 02:26_

---

_Review comment by @charliermarsh on `src/autofix/fixes.rs`:142 on 2022-10-02 02:26_

Here's the actual logic: extract the `Call`, then zero-out the arguments.

---

_Marked ready for review by @charliermarsh on 2022-10-02 02:38_

---

_Review comment by @sobolevn on `resources/test/fixtures/SPR001.py`:18 on 2022-10-02 06:52_

This was on purpose: `ast.Attribute` vs `ast.Call`

---

_Review comment by @sobolevn on `src/snapshots/ruff__linter__tests__spr001.snap`:50 on 2022-10-02 06:56_

Should not it be `row: 19` now?

Basically, what it is says in the snapshop:

```python
super(


       ).method() # wrong
```

Or, on the other hand, we can leave this to black or similar tools to fix.

---

_@sobolevn reviewed on 2022-10-02 06:57_

Oh, good stuff!

---

_@charliermarsh reviewed on 2022-10-02 14:04_

---

_Review comment by @charliermarsh on `resources/test/fixtures/SPR001.py`:18 on 2022-10-02 14:04_

Oh right, will revert. (The code works fine w/ either, didn't realize it was intentional but makes total sense.)

---

_@charliermarsh reviewed on 2022-10-02 14:05_

---

_Review comment by @charliermarsh on `src/snapshots/ruff__linter__tests__spr001.snap`:50 on 2022-10-02 14:05_

This is the start and end of the lines in the _unfixed_ source code, so I think it's correct?

---

_@sobolevn reviewed on 2022-10-02 15:48_

---

_Review comment by @sobolevn on `src/snapshots/ruff__linter__tests__spr001.snap`:50 on 2022-10-02 15:48_

Oh, I see! 👍 

---

_Merged by @charliermarsh on 2022-10-02 23:58_

---

_Closed by @charliermarsh on 2022-10-02 23:58_

---

_Branch deleted on 2022-10-02 23:58_

---

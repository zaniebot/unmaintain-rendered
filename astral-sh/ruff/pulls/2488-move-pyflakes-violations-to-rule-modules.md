```yaml
number: 2488
title: Move pyflakes violations to rule modules
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: move-pyflakes-violations
created_at: 2023-02-02T18:32:48Z
updated_at: 2023-02-02T19:01:00Z
url: https://github.com/astral-sh/ruff/pull/2488
synced_at: 2026-01-12T15:55:08Z
```

# Move pyflakes violations to rule modules

---

_@akx_

I also tried refactoring some more of the checks in ast.rs to functions the pyflakes package, but that didn't quite pan out due to a dizzying borrow checker error... 😁 

---

_Renamed from "Move pyflakes violations" to "Move pyflakes violations to rule modules" by @akx on 2023-02-02 18:44_

---

_Marked ready for review by @akx on 2023-02-02 18:50_

---

_Comment by @charliermarsh on 2023-02-02 19:00_

Yeah the borrowing rules in `ast.rs` and related checks feel like a house of cards right now. At some point `Checker` needs a thoughtful refactor.


---

_Merged by @charliermarsh on 2023-02-02 19:01_

---

_Closed by @charliermarsh on 2023-02-02 19:01_

---

```yaml
number: 2719
title: Handle more functions that never return in RET503
type: pull_request
state: merged
author: ngnpope
labels: []
assignees: []
merged: true
base: main
head: ret503-improvements
created_at: 2023-02-10T17:03:31Z
updated_at: 2023-02-10T17:15:33Z
url: https://github.com/astral-sh/ruff/pull/2719
synced_at: 2026-01-12T15:55:10Z
```

# Handle more functions that never return in RET503

---

_@ngnpope_

Follow up to cda2ff0b180d311a24cf545c0339ba8977437db3.

---

_Comment by @charliermarsh on 2023-02-10 17:04_

Thanks!

---

_Comment by @ngnpope on 2023-02-10 17:05_

Picks up a few more cases in the standard library.

There are also the following, but they'd require checking types:
- `argparse.ArgumentParser.error()`
- `argparse.ArgumentParser.exit()`
- `unittest.case.TestCase.fail()`
- `unittest.case.TestCase.skipTest()`

---

_Merged by @charliermarsh on 2023-02-10 17:09_

---

_Closed by @charliermarsh on 2023-02-10 17:09_

---

_Branch deleted on 2023-02-10 17:15_

---

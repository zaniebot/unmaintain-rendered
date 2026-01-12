```yaml
number: 1045
title: Update RustPython
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rust-python
created_at: 2022-12-05T00:54:53Z
updated_at: 2022-12-05T01:09:31Z
url: https://github.com/astral-sh/ruff/pull/1045
synced_at: 2026-01-12T05:36:31Z
```

# Update RustPython

---

_Pull request opened by @charliermarsh on 2022-12-05 00:54_

_No description provided._

---

_Comment by @charliermarsh on 2022-12-05 00:54_

\cc @harupy 

---

_Comment by @harupy on 2022-12-05 01:06_

Nice!

```bash
$ git branch --show-current
charlie/rust-python

$ cargo run -- --select PLR1701 --show-source resources/test/fixtures/pylint/consider_merging_isinstance.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.09s
     Running `target/debug/ruff --select PLR1701 --show-source resources/test/fixtures/pylint/consider_merging_isinstance.py`
Found 6 error(s).
resources/test/fixtures/pylint/consider_merging_isinstance.py:15:8: PLR1701 Consider merging these isinstance calls: `isinstance(var[3], (float, int))`
   |
15 |     if isinstance(var[3], int) or isinstance(var[3], float) or isinstance(var[3], list) and True:  # [consider-merging-isinstance]
   |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLR1701
   |
resources/test/fixtures/pylint/consider_merging_isinstance.py:17:14: PLR1701 Consider merging these isinstance calls: `isinstance(var[4], (float, int))`
   |
17 |     result = isinstance(var[4], int) or isinstance(var[4], float) or isinstance(var[5], list) and False  # [consider-merging-isinstance]
   |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLR1701
   |
resources/test/fixtures/pylint/consider_merging_isinstance.py:19:14: PLR1701 Consider merging these isinstance calls: `isinstance(var[5], (float, int))`
   |
19 |     result = isinstance(var[5], int) or True or isinstance(var[5], float)  # [consider-merging-isinstance]
   |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLR1701
   |
resources/test/fixtures/pylint/consider_merging_isinstance.py:23:14: PLR1701 Consider merging these isinstance calls: `isinstance(var[10], (list, str))`
   |
23 |     result = isinstance(var[10], str) or isinstance(var[10], int) and var[8] * 14 or isinstance(var[10], float) and var[5] * 14.4 or isinstance(var[10], list)   # [consider-merging-isinstance]
   |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLR1701
   |
resources/test/fixtures/pylint/consider_merging_isinstance.py:24:14: PLR1701 Consider merging these isinstance calls: `isinstance(var[11], (float, int))`
   |
24 |     result = isinstance(var[11], int) or isinstance(var[11], int) or isinstance(var[11], float)   # [consider-merging-isinstance]
   |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLR1701
   |
resources/test/fixtures/pylint/consider_merging_isinstance.py:30:14: PLR1701 Consider merging these isinstance calls: `isinstance(var[12], (float, int, list))`
   |
30 |     result = isinstance(var[12], (int, float)) or isinstance(var[12], list)  # [consider-merging-isinstance]
   |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PLR1701
   |

```

---

_Merged by @charliermarsh on 2022-12-05 01:09_

---

_Closed by @charliermarsh on 2022-12-05 01:09_

---

_Branch deleted on 2022-12-05 01:09_

---

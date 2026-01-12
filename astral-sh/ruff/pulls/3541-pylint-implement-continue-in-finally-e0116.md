```yaml
number: 3541
title: "[`pylint`]: Implement `continue-in-finally` (`E0116`)"
type: pull_request
state: merged
author: latonis
labels: []
assignees: []
merged: true
base: main
head: continue-in-finally-pylint
created_at: 2023-03-15T16:37:22Z
updated_at: 2023-03-17T03:03:03Z
url: https://github.com/astral-sh/ruff/pull/3541
synced_at: 2026-01-12T04:39:45Z
```

# [`pylint`]: Implement `continue-in-finally` (`E0116`)

---

_Pull request opened by @latonis on 2023-03-15 16:37_

Rule reference: https://pycodequ.al/docs/pylint-messages/e0116-continue-in-finally.html

Implemented for Pylint parent issue: #970 

---

_Comment by @github-actions[bot] on 2023-03-15 16:47_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/pylint/continue_in_finally.py`:24 on 2023-03-15 16:55_

Can you add a test for nested statements?

```suggestion
    finally:
    	if True:
	        continue  # [continue-in-finally]
```

---

_@MichaReiser reviewed on 2023-03-15 16:55_

---

_Review comment by @latonis on `crates/ruff/resources/test/fixtures/pylint/continue_in_finally.py`:24 on 2023-03-15 17:03_

yup, actually realized that right after pushing, better implementation incoming :) 

---

_@latonis reviewed on 2023-03-15 17:03_

---

_Review requested from @MichaReiser by @latonis on 2023-03-15 17:39_

---

_Comment by @latonis on 2023-03-15 17:40_

I have fixed the nested catch @MichaReiser, apologies I didn't catch it initially 😸 

---

_Review comment by @r3m0t on `crates/ruff/src/rules/pylint/rules/continue_in_finally.rs`:65 on 2023-03-16 00:43_

search for `Vec<Stmt<U>>` in `StmtKind` source found some other cases to traverse-

`orelse`  in a couple of nodes, including `If`, `For`, `AsyncFor` and `While`

`body` in `Try`, `TryStar`

---

_@r3m0t reviewed on 2023-03-16 00:43_

---

_@latonis reviewed on 2023-03-16 01:08_

---

_Review comment by @latonis on `crates/ruff/src/rules/pylint/rules/continue_in_finally.rs`:65 on 2023-03-16 01:08_

I always forget those `for..else` and `while..else` statements exist 🫠

---

_Comment by @github-actions[bot] on 2023-03-16 13:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      7.6±0.01ms     5.4 MB/sec    1.00      7.6±0.02ms     5.4 MB/sec
linter/numpy/ctypeslib.py    1.00      2.7±0.01ms   128.5 MB/sec    1.00      2.7±0.01ms   128.3 MB/sec
linter/numpy/globals.py      1.00   1357.7±4.18µs   131.3 MB/sec    1.00  1360.6±17.02µs   131.0 MB/sec
linter/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.01     10.8±0.43ms     3.8 MB/sec    1.00     10.8±0.33ms     3.8 MB/sec
linter/numpy/ctypeslib.py    1.04      3.6±0.11ms    94.2 MB/sec    1.00      3.4±0.13ms    98.4 MB/sec
linter/numpy/globals.py      1.02  1838.9±61.07µs    96.9 MB/sec    1.00  1809.9±104.32µs    98.5 MB/sec
linter/pydantic/types.py     1.01      5.0±0.18ms     5.1 MB/sec    1.00      5.0±0.17ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-16 14:53_

---

_Renamed from "pylint: E0116 continue-in-finally" to "[`pylint`]: Implement `continue-in-finally` (`E0116`)" by @charliermarsh on 2023-03-17 02:43_

---

_Merged by @charliermarsh on 2023-03-17 02:47_

---

_Closed by @charliermarsh on 2023-03-17 02:47_

---

_Branch deleted on 2023-03-17 03:00_

---

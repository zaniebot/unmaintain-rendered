```yaml
number: 4697
title: Add docs to clarify project root heuristics
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/root
created_at: 2023-05-28T23:05:36Z
updated_at: 2023-05-30T01:54:40Z
url: https://github.com/astral-sh/ruff/pull/4697
synced_at: 2026-01-12T03:50:03Z
```

# Add docs to clarify project root heuristics

---

_Pull request opened by @charliermarsh on 2023-05-28 23:05_

Closes #4674.

---

_Comment by @charliermarsh on 2023-05-28 23:05_

@jamesbraza - Open to feedback!

---

_Label `documentation` added by @charliermarsh on 2023-05-28 23:05_

---

_Comment by @github-actions[bot] on 2023-05-28 23:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.12ms     2.9 MB/sec    1.00     14.1±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    428.2±4.89µs     6.9 MB/sec    1.00    423.3±0.81µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1495.8±2.07µs    11.1 MB/sec    1.01   1503.8±2.63µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.6±0.44µs    17.4 MB/sec    1.01    171.6±0.25µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.01      5.2±0.00ms     7.8 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1035.1±7.96µs    16.1 MB/sec    1.00   1022.7±0.53µs    16.3 MB/sec
parser/numpy/globals.py                    1.01    106.8±0.16µs    27.6 MB/sec    1.00    105.6±0.16µs    27.9 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.3 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.13ms     2.5 MB/sec    1.01     16.5±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.04ms     4.0 MB/sec    1.00      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.03    497.5±6.31µs     5.9 MB/sec    1.00    483.7±7.11µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.06ms     3.7 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     5.0 MB/sec    1.01      8.3±0.21ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1748.3±24.62µs     9.5 MB/sec    1.02  1788.3±97.57µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    202.5±4.07µs    14.6 MB/sec    1.00    200.7±9.53µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.3±0.06ms     6.4 MB/sec    1.02      6.4±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1184.5±25.62µs    14.1 MB/sec    1.03  1219.6±12.31µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    120.1±1.50µs    24.6 MB/sec    1.04    125.2±2.21µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.05ms     9.6 MB/sec    1.03      2.8±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @calumy on `docs/faq.md`:266 on 2023-05-29 01:02_

```suggestion
```tree
```

Hopefully this will fix the ci issue

---

_@calumy reviewed on 2023-05-29 01:02_

---

_Comment by @charliermarsh on 2023-05-29 02:49_

Merging but can always follow-up on any feedback.

---

_Merged by @charliermarsh on 2023-05-29 02:50_

---

_Closed by @charliermarsh on 2023-05-29 02:50_

---

_Branch deleted on 2023-05-29 02:50_

---

_@jamesbraza reviewed on 2023-05-30 01:54_

Looks great, thank you!

One trick I personally found useful was using `--verbose --no-cache`, so that `ruff` prints out why it makes 1st vs 3rd party decisions.  If you want, that can be added to the new FAQ section.

Otherwise, thanks again!

---

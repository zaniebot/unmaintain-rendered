```yaml
number: 4428
title: "docs: update contributing guide"
type: pull_request
state: merged
author: yanksyoon
labels: []
assignees: []
merged: true
base: main
head: docs/contributing
created_at: 2023-05-14T09:55:01Z
updated_at: 2023-05-15T02:40:29Z
url: https://github.com/astral-sh/ruff/pull/4428
synced_at: 2026-01-12T15:55:15Z
```

# docs: update contributing guide

---

_@yanksyoon_

This is a simple documentation update PR that updates `CONTRIBUTING.md` to match the file path for `mod.rs`.

---

_Renamed from "docs: update contributing path" to "docs: update contributing guide" by @yanksyoon on 2023-05-14 09:55_

---

_Comment by @github-actions[bot] on 2023-05-14 10:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.02ms     2.8 MB/sec    1.04     15.0±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.03      3.7±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.3±1.44µs     8.2 MB/sec    1.02    369.0±1.61µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.04      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.09      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1494.1±3.84µs    11.1 MB/sec    1.07   1595.0±6.31µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.4±0.72µs    18.2 MB/sec    1.04    169.5±3.60µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.07      3.4±0.01ms     7.4 MB/sec
parser/large/dataset.py                    1.01      5.9±0.00ms     6.9 MB/sec    1.00      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.01   1141.8±2.13µs    14.6 MB/sec    1.00   1130.8±5.50µs    14.7 MB/sec
parser/numpy/globals.py                    1.01    116.4±0.42µs    25.4 MB/sec    1.00    115.8±0.49µs    25.5 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.22ms     2.4 MB/sec    1.00     16.5±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.08ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    497.5±7.73µs     5.9 MB/sec    1.00    498.5±6.33µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.00      7.0±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.11ms     5.0 MB/sec    1.01      8.2±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1737.3±20.99µs     9.6 MB/sec    1.01  1757.9±23.75µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.4±3.50µs    14.9 MB/sec    1.02    201.2±6.71µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.02      3.8±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.02      6.7±0.06ms     6.1 MB/sec    1.00      6.6±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.03  1286.0±17.09µs    12.9 MB/sec    1.00  1254.1±11.10µs    13.3 MB/sec
parser/numpy/globals.py                    1.03    134.2±6.01µs    22.0 MB/sec    1.00    130.2±3.74µs    22.7 MB/sec
parser/pydantic/types.py                   1.03      2.9±0.07ms     8.8 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@JonathanPlasse reviewed on 2023-05-14 14:52_

---

_Review comment by @JonathanPlasse on `CONTRIBUTING.md`:114 on 2023-05-14 14:52_

Can you revert to only `1.` everywhere?
The markdown preview will not display only `1.` but increment for each bullet.
This is failing the pre-commit checks.

---

_Merged by @charliermarsh on 2023-05-15 02:21_

---

_Closed by @charliermarsh on 2023-05-15 02:21_

---

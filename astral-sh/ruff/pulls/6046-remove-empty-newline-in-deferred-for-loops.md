```yaml
number: 6046
title: "Remove empty newline in `deferred_for_loops`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/for
created_at: 2023-07-24T21:52:32Z
updated_at: 2023-07-24T22:19:09Z
url: https://github.com/astral-sh/ruff/pull/6046
synced_at: 2026-01-12T03:30:22Z
```

# Remove empty newline in `deferred_for_loops`

---

_Pull request opened by @charliermarsh on 2023-07-24 21:52_

Trivial change but none of the others have this empty newline.

---

_Renamed from "Remove empty newline in deferred_for_loops" to "Remove empty newline in `deferred_for_loops`" by @charliermarsh on 2023-07-24 21:52_

---

_Label `internal` added by @charliermarsh on 2023-07-24 21:52_

---

_Merged by @charliermarsh on 2023-07-24 21:59_

---

_Closed by @charliermarsh on 2023-07-24 21:59_

---

_Branch deleted on 2023-07-24 21:59_

---

_Comment by @github-actions[bot] on 2023-07-24 22:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.03ms     3.7 MB/sec    1.01     11.1±0.07ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.00ms     7.4 MB/sec    1.01      2.3±0.00ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    255.8±0.67µs    11.5 MB/sec    1.00    255.0±0.34µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.02ms     5.3 MB/sec    1.01      4.9±0.02ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.07ms     2.7 MB/sec    1.03     15.5±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.00ms     4.3 MB/sec    1.01      3.9±0.02ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    508.6±0.77µs     5.8 MB/sec    1.00    507.4±0.71µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.01ms     3.7 MB/sec    1.02      7.0±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.2 MB/sec    1.04      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1685.1±4.00µs     9.9 MB/sec    1.02   1715.7±3.02µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.8±0.49µs    15.6 MB/sec    1.01    190.9±0.40µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.2 MB/sec    1.02      3.7±0.02ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.18ms     3.7 MB/sec    1.00     10.9±0.16ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.06ms     7.6 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    245.8±7.56µs    12.0 MB/sec    1.02   250.2±11.30µs    11.8 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.06ms     5.3 MB/sec    1.00      4.8±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.20ms     2.7 MB/sec    1.01     15.4±0.21ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.06ms     4.2 MB/sec    1.00      4.0±0.06ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   484.1±14.19µs     6.1 MB/sec    1.00    482.3±9.05µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.12ms     3.6 MB/sec    1.01      7.0±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.13ms     4.9 MB/sec    1.00      8.1±0.15ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1703.1±37.33µs     9.8 MB/sec    1.00  1692.1±26.11µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.6±4.49µs    15.2 MB/sec    1.00    193.2±3.97µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

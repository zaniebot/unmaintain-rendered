```yaml
number: 4211
title: "[flake8-pyi] PYI020"
type: pull_request
state: merged
author: arya-k
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi_y020
created_at: 2023-05-03T15:43:46Z
updated_at: 2023-05-04T02:38:34Z
url: https://github.com/astral-sh/ruff/pull/4211
synced_at: 2026-01-12T15:55:14Z
```

# [flake8-pyi] PYI020

---

_@arya-k_

PYI020 bans quoted annotations in stub files, since they're always allowed to contain forward references.

I also included an auto fix, to remove the quotes.

rel: https://github.com/charliermarsh/ruff/issues/848

ps: This is my first contribution! Looking forward to feedback. Totally happy to hear extra nitpicks :)

---

_Comment by @github-actions[bot] on 2023-05-03 15:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.4±0.14ms     3.3 MB/sec    1.13     14.0±0.17ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.02ms     5.5 MB/sec    1.12      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.2±0.65µs     7.9 MB/sec    1.12    420.3±0.71µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.2±0.07ms     4.9 MB/sec    1.12      5.9±0.09ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.39ms     6.1 MB/sec    1.04      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1503.0±4.70µs    11.1 MB/sec    1.00   1491.9±2.74µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.8±0.45µs    17.6 MB/sec    1.00    168.3±4.70µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.03      5.6±0.01ms     7.2 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.03   1085.9±2.49µs    15.3 MB/sec    1.00   1056.0±1.20µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    108.4±5.60µs    27.2 MB/sec    1.00    107.9±0.38µs    27.3 MB/sec
parser/pydantic/types.py                   1.02      2.4±0.00ms    10.8 MB/sec    1.00      2.3±0.01ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.18ms     2.4 MB/sec    1.01     16.9±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.01      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.7±7.10µs     6.0 MB/sec    1.00    489.7±6.70µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.02      7.2±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.08ms     4.8 MB/sec    1.00      8.4±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1797.7±21.22µs     9.3 MB/sec    1.00  1774.8±19.52µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    200.5±4.21µs    14.7 MB/sec    1.00    199.3±5.69µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.02      6.8±0.07ms     5.9 MB/sec    1.00      6.7±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1309.7±19.04µs    12.7 MB/sec    1.00  1295.6±15.83µs    12.9 MB/sec
parser/numpy/globals.py                    1.01    132.9±1.85µs    22.2 MB/sec    1.00    132.0±1.92µs    22.3 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.8 MB/sec    1.00      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-04 02:37_

This looks great! I wouldn't change a thing. Thanks for getting involved with Ruff!

---

_Label `rule` added by @charliermarsh on 2023-05-04 02:37_

---

_Merged by @charliermarsh on 2023-05-04 02:37_

---

_Closed by @charliermarsh on 2023-05-04 02:37_

---

_Branch deleted on 2023-05-04 02:38_

---

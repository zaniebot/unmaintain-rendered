```yaml
number: 4087
title: "Add support for providing command-line arguments via `argfile`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/argfile
created_at: 2023-04-25T05:41:25Z
updated_at: 2023-04-25T23:58:26Z
url: https://github.com/astral-sh/ruff/pull/4087
synced_at: 2026-01-12T15:55:14Z
```

# Add support for providing command-line arguments via `argfile`

---

_@charliermarsh_

## Summary

This PR uses the [`argfile`](https://docs.rs/argfile/0.1.5/argfile/) crate to allow users to provide additional command-line arguments as a separate argument file.

For example, one might create file like `list.txt`:

```txt
foo.py
bar.py
```

Then, invoking `ruff @list.txt` would be equivalent to `ruff foo.py bar.py`.

Closes #4079.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-04-25 05:41_

---

_Comment by @github-actions[bot] on 2023-04-25 05:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.05ms     2.8 MB/sec    1.01     14.7±0.16ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    462.9±1.09µs     6.4 MB/sec    1.00    458.7±0.66µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1652.4±1.33µs    10.1 MB/sec    1.00   1639.6±6.62µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.1±0.50µs    16.3 MB/sec    1.00    180.8±1.27µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.03ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.00      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1146.5±0.68µs    14.5 MB/sec    1.01   1158.3±1.71µs    14.4 MB/sec
parser/numpy/globals.py                    1.00    112.8±0.20µs    26.2 MB/sec    1.01    113.8±0.28µs    25.9 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.01      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.24ms     2.3 MB/sec    1.00     17.3±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.07ms     3.8 MB/sec    1.00      4.5±0.09ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    541.3±9.25µs     5.5 MB/sec    1.01   544.7±15.07µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.25ms     3.5 MB/sec    1.00      7.3±0.14ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.07ms     4.6 MB/sec    1.01      9.0±0.10ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1931.1±20.09µs     8.6 MB/sec    1.02  1965.4±28.18µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    217.0±3.85µs    13.6 MB/sec    1.03    222.8±7.16µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.04ms     6.4 MB/sec    1.02      4.1±0.08ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.9±0.07ms     5.9 MB/sec    1.01      6.9±0.07ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1307.6±15.59µs    12.7 MB/sec    1.01  1317.0±18.68µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    132.2±2.91µs    22.3 MB/sec    1.00    132.0±2.20µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.05ms     8.6 MB/sec    1.00      3.0±0.03ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-04-25 13:30_

---

_Merged by @charliermarsh on 2023-04-25 23:58_

---

_Closed by @charliermarsh on 2023-04-25 23:58_

---

_Branch deleted on 2023-04-25 23:58_

---

_Label `configuration` added by @charliermarsh on 2023-04-25 23:58_

---

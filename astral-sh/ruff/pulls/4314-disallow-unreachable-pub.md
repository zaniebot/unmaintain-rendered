```yaml
number: 4314
title: Disallow unreachable_pub
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: disallow-unreachable-pub
created_at: 2023-05-09T16:33:58Z
updated_at: 2023-05-11T22:00:42Z
url: https://github.com/astral-sh/ruff/pull/4314
synced_at: 2026-01-12T03:56:39Z
```

# Disallow unreachable_pub

---

_Pull request opened by @JonathanPlasse on 2023-05-09 16:33_

Fixed with `cargo clippy --fix` and some manual fixes for IOError

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-09 16:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/text.rs`:117 on 2023-05-09 17:48_

Can it be avoided that it adds the pub(crate) if the type itself isn't visible? 

---

_@MichaReiser reviewed on 2023-05-09 17:50_

---

_@JonathanPlasse reviewed on 2023-05-09 18:07_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/message/text.rs`:117 on 2023-05-09 18:07_

Currently, it seems not.
I could not find an issue concerning this in rust-lang/rust.

---

_Comment by @github-actions[bot] on 2023-05-09 18:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.11     14.8±0.25ms     2.7 MB/sec    1.00     13.4±0.25ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.5±0.11ms     4.7 MB/sec    1.00      3.4±0.12ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   428.8±12.19µs     6.9 MB/sec    1.16   499.5±43.02µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.19ms     4.1 MB/sec    1.00      6.1±0.43ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.11      8.0±0.24ms     5.1 MB/sec    1.00      7.2±0.49ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09  1625.4±32.36µs    10.2 MB/sec    1.00  1494.5±65.40µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.8±4.84µs    16.1 MB/sec    1.00   182.2±13.50µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.5±0.07ms     7.4 MB/sec    1.00      3.3±0.18ms     7.8 MB/sec
parser/large/dataset.py                    1.02      5.4±0.15ms     7.6 MB/sec    1.00      5.2±0.12ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.03  1061.5±40.02µs    15.7 MB/sec    1.00  1026.9±22.85µs    16.2 MB/sec
parser/numpy/globals.py                    1.01    103.8±3.43µs    28.4 MB/sec    1.00    102.6±4.51µs    28.8 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.04ms    11.2 MB/sec    1.00      2.3±0.05ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.29ms     2.4 MB/sec    1.00     16.6±0.33ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.08ms     3.9 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    497.4±6.24µs     5.9 MB/sec    1.00    496.6±6.90µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.12ms     3.6 MB/sec    1.00      7.0±0.24ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.04      8.5±0.18ms     4.8 MB/sec    1.00      8.2±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1780.6±31.08µs     9.4 MB/sec    1.00  1760.7±26.01µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.9±7.20µs    14.6 MB/sec    1.01    203.2±8.09µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.09ms     6.8 MB/sec    1.00      3.7±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.11ms     6.0 MB/sec    1.02      6.8±0.11ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1267.6±20.85µs    13.1 MB/sec    1.01  1285.6±25.29µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    129.8±3.40µs    22.7 MB/sec    1.00    129.7±2.29µs    22.8 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.07ms     9.0 MB/sec    1.01      2.9±0.04ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-05-10 09:22_

Should I rebase?

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-11 07:46_

---

_Comment by @charliermarsh on 2023-05-11 18:50_

Yeah, I think I'm comfortable with this change, so feel free to rebase and I'll try to get it merged once it's up-to-date.

---

_Comment by @JonathanPlasse on 2023-05-11 21:24_

Just rebased!

---

_Merged by @charliermarsh on 2023-05-11 22:00_

---

_Closed by @charliermarsh on 2023-05-11 22:00_

---

_Branch deleted on 2023-05-11 22:00_

---

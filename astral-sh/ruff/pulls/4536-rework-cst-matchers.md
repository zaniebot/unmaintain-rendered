```yaml
number: 4536
title: Rework CST matchers
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: rework-cst-matchers
created_at: 2023-05-19T22:50:32Z
updated_at: 2023-05-23T07:06:49Z
url: https://github.com/astral-sh/ruff/pull/4536
synced_at: 2026-01-12T03:50:03Z
```

# Rework CST matchers

---

_Pull request opened by @JonathanPlasse on 2023-05-19 22:50_

- [x] Remove `match_expr()`
- [x] Remove most `match_module()`
- [x] Add `match_statement()`
- [x] Add several other matches

I think we should split other reworks into other PRs.

---

_Comment by @github-actions[bot] on 2023-05-19 23:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.9±0.68ms     2.4 MB/sec    1.00     16.4±0.67ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.15ms     4.0 MB/sec    1.00      4.1±0.21ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   514.6±18.96µs     5.7 MB/sec    1.00   513.4±26.62µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.3±0.15ms     3.5 MB/sec    1.00      7.0±0.31ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.22ms     5.2 MB/sec    1.00      7.9±0.24ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1678.9±78.86µs     9.9 MB/sec    1.01  1696.6±60.42µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   193.3±13.83µs    15.3 MB/sec    1.09    210.5±9.97µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.10ms     7.1 MB/sec    1.03      3.7±0.17ms     7.0 MB/sec
parser/large/dataset.py                    1.08      6.6±0.19ms     6.2 MB/sec    1.00      6.1±0.17ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.11  1322.4±40.83µs    12.6 MB/sec    1.00  1192.0±55.08µs    14.0 MB/sec
parser/numpy/globals.py                    1.00    127.7±4.00µs    23.1 MB/sec    1.00    127.7±6.30µs    23.1 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.08ms     9.1 MB/sec    1.00      2.8±0.08ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     23.8±0.84ms  1751.0 KB/sec     1.05     25.0±0.66ms  1666.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      6.2±0.53ms     2.7 MB/sec     1.00      6.2±0.27ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   691.0±33.15µs     4.3 MB/sec     1.00   684.3±33.37µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.01     10.5±0.41ms     2.4 MB/sec     1.00     10.5±0.31ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.00     12.4±0.51ms     3.3 MB/sec     1.00     12.3±0.30ms     3.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.14ms     7.0 MB/sec     1.01      2.4±0.09ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   282.5±18.69µs    10.4 MB/sec     1.01   284.2±18.91µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.2±0.18ms     4.9 MB/sec     1.02      5.3±0.19ms     4.8 MB/sec
parser/large/dataset.py                    1.00      9.6±0.36ms     4.2 MB/sec     1.27     12.2±0.33ms     3.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1794.9±156.17µs     9.3 MB/sec    1.21      2.2±0.12ms     7.6 MB/sec
parser/numpy/globals.py                    1.00   178.0±11.11µs    16.6 MB/sec     1.19   212.1±10.22µs    13.9 MB/sec
parser/pydantic/types.py                   1.00      4.0±0.10ms     6.5 MB/sec     1.24      4.9±0.22ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-20 02:40_

---

_Review comment by @MichaReiser on `crates/ruff/src/cst/matchers.rs`:23 on 2023-05-22 07:34_

Oh, I didn't notice that these are working on our `CST` and not on the AST...

---

_Review comment by @MichaReiser on `crates/ruff/src/cst/matchers.rs`:67 on 2023-05-22 07:35_

Can you explain why we want to borrow the box here instead of returning a `Result<&'a Call<'b>>`?

---

_@MichaReiser approved on 2023-05-22 07:36_

---

_@JonathanPlasse reviewed on 2023-05-22 17:54_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/cst/matchers.rs`:67 on 2023-05-22 17:54_

The code does not compile without it because this code:
https://github.com/charliermarsh/ruff/blob/3b8121379ddec98d759c3ed21e3ed65bef122e97/crates/ruff/src/rules/flake8_comprehensions/fixes.rs#L706-L722

---

_Review requested from @MichaReiser by @JonathanPlasse on 2023-05-22 19:35_

---

_@MichaReiser reviewed on 2023-05-23 06:14_

---

_Review comment by @MichaReiser on `crates/ruff/src/cst/matchers.rs`:67 on 2023-05-23 06:14_

I would have thought that there's a better way (feels very clumsy) but `Box::new((*inner_call).clone())` works.

---

_Merged by @MichaReiser on 2023-05-23 06:26_

---

_Closed by @MichaReiser on 2023-05-23 06:26_

---

_Branch deleted on 2023-05-23 07:06_

---

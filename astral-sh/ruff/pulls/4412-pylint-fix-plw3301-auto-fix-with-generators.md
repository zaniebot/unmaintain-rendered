```yaml
number: 4412
title: "[`pylint`] Fix `PLW3301` auto-fix with generators"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-plw3301-auto-fix-with-generators
created_at: 2023-05-13T13:30:16Z
updated_at: 2023-05-13T15:25:36Z
url: https://github.com/astral-sh/ruff/pull/4412
synced_at: 2026-01-12T03:50:02Z
```

# [`pylint`] Fix `PLW3301` auto-fix with generators

---

_Pull request opened by @JonathanPlasse on 2023-05-13 13:30_

- Close #4410

---

_@charliermarsh reviewed on 2023-05-13 13:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:80 on 2023-05-13 13:32_

I don't think this is quite sufficient, since `max(1, max(a))` will still be flagged, where `a` could be a generator. In fact, I think `a` _has_ to be an iterable, so `max(1, a)` will fail regardless?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLW3301_nested_min_max.py.snap`:230 on 2023-05-13 13:33_

Does this eagerly evaluate the generator?

---

_@charliermarsh reviewed on 2023-05-13 13:33_

---

_Comment by @github-actions[bot] on 2023-05-13 13:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.6±0.01ms     2.8 MB/sec    1.00     14.5±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    363.5±1.56µs     8.1 MB/sec    1.00    359.5±1.21µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.01ms     5.6 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1536.1±2.50µs    10.8 MB/sec    1.00   1504.8±5.56µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.0±0.26µs    17.9 MB/sec    1.00    162.7±0.54µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.8 MB/sec    1.00      3.2±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.03      6.0±0.01ms     6.8 MB/sec    1.00      5.8±0.00ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.04   1168.6±0.65µs    14.2 MB/sec    1.00   1125.9±1.40µs    14.8 MB/sec
parser/numpy/globals.py                    1.04    119.8±0.66µs    24.6 MB/sec    1.00    115.1±0.33µs    25.6 MB/sec
parser/pydantic/types.py                   1.04      2.6±0.00ms    10.0 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     21.4±0.81ms  1947.5 KB/sec     1.00     21.5±0.91ms  1938.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.6±0.28ms     3.0 MB/sec     1.00      5.5±0.25ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   621.2±29.08µs     4.7 MB/sec     1.05   651.2±43.31µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.38ms     2.9 MB/sec     1.01      9.0±0.39ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.36ms     4.1 MB/sec     1.04     10.4±0.37ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.7 MB/sec     1.01      2.2±0.11ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.7±13.30µs    12.0 MB/sec     1.01   247.6±15.86µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.18ms     5.6 MB/sec     1.01      4.6±0.23ms     5.5 MB/sec
parser/large/dataset.py                    1.00      8.5±0.27ms     4.8 MB/sec     1.02      8.7±0.29ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.02  1641.6±112.51µs    10.1 MB/sec    1.00  1614.5±70.90µs    10.3 MB/sec
parser/numpy/globals.py                    1.05   170.9±14.13µs    17.3 MB/sec     1.00    163.1±9.23µs    18.1 MB/sec
parser/pydantic/types.py                   1.00      3.7±0.17ms     7.0 MB/sec     1.00      3.7±0.24ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@JonathanPlasse reviewed on 2023-05-13 13:53_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLW3301_nested_min_max.py.snap`:230 on 2023-05-13 13:53_

This code
```python
def fn1():
    for i in range(10):
        print(i)
        yield i

def fn2():
    print('fn2')
    return 1

max(1, *fn1(), fn2())
```
prints this
```console
0
1
2
3
4
5
6
7
8
9
fn2
```
It is eager otherwise, `fn2` would be printed first.

---

_@JonathanPlasse reviewed on 2023-05-13 14:29_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pylint/rules/nested_min_max.rs`:80 on 2023-05-13 14:29_

Indeed, a single `min`/`max` argument has to be iterable according to typeshed.

---

_Renamed from "Fix PLW3301 auto-fix with generators" to "[`pylint`] Fix `PLW3301` auto-fix with generators" by @charliermarsh on 2023-05-13 15:17_

---

_Merged by @charliermarsh on 2023-05-13 15:17_

---

_Closed by @charliermarsh on 2023-05-13 15:17_

---

_Label `rule` added by @charliermarsh on 2023-05-13 15:17_

---

_Branch deleted on 2023-05-13 15:25_

---

```yaml
number: 4537
title: Include empty success test in JUnit output
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/junit
created_at: 2023-05-20T03:33:04Z
updated_at: 2023-05-20T04:06:21Z
url: https://github.com/astral-sh/ruff/pull/4537
synced_at: 2026-01-12T03:50:03Z
```

# Include empty success test in JUnit output

---

_Pull request opened by @charliermarsh on 2023-05-20 03:33_

## Summary

When Jenkins encounters a JUnit XML file that doesn't contain _any_ test results, by default, it errors. You can configure Jenkins to treat empty files as "successful", but that opens the door to silently ignoring valid errors in your configuration or test running.

This PR modifies the JUnit reporter to include a single successful test if Ruff doesn't find any valid diagnostics.

Closes #4502.

## Test Plan

Ran `cargo run -p ruff_cli -- check foo.py --format junit --select F` on an empty file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="ruff" tests="1" failures="0" errors="0">
    <testsuite name="ruff" tests="1" disabled="0" errors="0" failures="0" package="org.ruff">
        <testcase name="No errors found" classname="ruff">
        </testcase>
    </testsuite>
</testsuites>
```

I also uploaded this to a JUnit result visualizer online, and it looked reasonable:

<img width="1792" alt="Screen Shot 2023-05-19 at 11 34 15 PM" src="https://github.com/charliermarsh/ruff/assets/1309177/79d6c876-ec0d-473d-9c49-5675133f6801">


---

_Merged by @charliermarsh on 2023-05-20 03:38_

---

_Closed by @charliermarsh on 2023-05-20 03:38_

---

_Branch deleted on 2023-05-20 03:38_

---

_Comment by @github-actions[bot] on 2023-05-20 03:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.07ms     2.9 MB/sec    1.00     14.2±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.6±1.75µs     7.0 MB/sec    1.01    424.5±1.27µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.01      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1451.7±1.74µs    11.5 MB/sec    1.00   1453.2±1.49µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.0±0.73µs    18.3 MB/sec    1.00    161.5±1.28µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.1±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.6 MB/sec    1.21      6.5±0.01ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1062.6±0.76µs    15.7 MB/sec    1.16   1235.2±8.63µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    109.1±0.23µs    27.0 MB/sec    1.12    122.8±0.13µs    24.0 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.01ms    11.0 MB/sec    1.16      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.43ms     2.3 MB/sec    1.00     17.4±0.39ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.4±0.15ms     3.8 MB/sec    1.00      4.3±0.12ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   504.0±18.78µs     5.9 MB/sec    1.02   514.7±23.72µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.17ms     3.5 MB/sec    1.01      7.3±0.20ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.21ms     4.8 MB/sec    1.00      8.5±0.22ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1770.5±49.87µs     9.4 MB/sec    1.00  1770.7±36.91µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.5±6.84µs    14.9 MB/sec    1.04   206.1±13.03µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.09ms     6.7 MB/sec    1.00      3.8±0.08ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.9±0.11ms     5.9 MB/sec    1.00      6.8±0.10ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.02  1322.6±55.20µs    12.6 MB/sec    1.00  1299.3±33.89µs    12.8 MB/sec
parser/numpy/globals.py                    1.01    135.1±5.09µs    21.8 MB/sec    1.00    133.3±4.24µs    22.1 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.08ms     8.8 MB/sec    1.00      2.9±0.07ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

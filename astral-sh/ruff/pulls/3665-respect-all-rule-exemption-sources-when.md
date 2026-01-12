```yaml
number: 3665
title: Respect all rule-exemption sources when suppressing parser errors
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/exempt
created_at: 2023-03-22T04:08:30Z
updated_at: 2023-03-23T17:37:00Z
url: https://github.com/astral-sh/ruff/pull/3665
synced_at: 2026-01-12T04:39:45Z
```

# Respect all rule-exemption sources when suppressing parser errors

---

_Pull request opened by @charliermarsh on 2023-03-22 04:08_

## Summary

By default, if we hit a syntax error when parsing a file, we show the user a warning in addition to adding a diagnostic (if the user has `E999` enabled).

The relevant discussion is here: https://github.com/charliermarsh/ruff/pull/2505. In short, _only_ logging the diagnostic is insufficient, since if a user does `ruff check --select F`, and the file contains a syntax error, it would lead to a _silent_ failure. However, if a user has explicitly ignored a syntax error, it's tedious to have it reported on every invocation. So this is the workaround.

However, there's a bug in the current solution, which is that it only recognizes inline `noqa` comments and not file-wide exemptions (e.g., `# ruff: noqa` at the top of the file), which leads to a really subtle bug in which we _think_ we've introduced a syntax error after removing the unused `noqa` comment on a syntax-error-causing line.

This PR fixes _that_ bug (without changing behavior) by respecting all forms of error suppression when considering whether to report a warning back to the user.

Closes #3622.

---

_Comment by @github-actions[bot] on 2023-03-22 04:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.8±0.08ms     2.9 MB/sec    1.00     13.7±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    495.7±1.58µs     6.0 MB/sec    1.00    490.4±2.14µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.05ms     4.2 MB/sec    1.00      6.0±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.4±0.04ms     5.5 MB/sec    1.00      7.2±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1633.0±3.84µs    10.2 MB/sec    1.00   1616.3±4.43µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    183.3±0.63µs    16.1 MB/sec    1.00    182.2±0.54µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.02ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.7±0.61ms     2.3 MB/sec    1.01     17.9±0.30ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.11ms     3.5 MB/sec    1.01      4.8±0.11ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   606.7±20.18µs     4.9 MB/sec    1.02   621.5±18.59µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.20ms     3.3 MB/sec    1.03      7.9±0.13ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.43ms     4.2 MB/sec    1.05     10.1±0.23ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     8.1 MB/sec    1.05      2.2±0.05ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    227.9±7.46µs    12.9 MB/sec    1.04    237.9±9.59µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.15ms     5.7 MB/sec    1.04      4.6±0.12ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-22 18:14_

---

_Marked ready for review by @charliermarsh on 2023-03-22 18:14_

---

_@MichaReiser approved on 2023-03-23 08:54_

The change looks reasonable to me. I'm a bit uncertain if it should be possible to suppress syntax errors because they'll ultimately result in a runtime error and other ruff analysis not running (more potential runtime errors). 

---

_Comment by @charliermarsh on 2023-03-23 17:36_

Yeah I'm not sure either. This _was_ important when we knowingly didn't support all valid syntax, and users needed a workaround. Now I'm not so sure.

---

_Merged by @charliermarsh on 2023-03-23 17:36_

---

_Closed by @charliermarsh on 2023-03-23 17:36_

---

_Branch deleted on 2023-03-23 17:36_

---

_Comment by @charliermarsh on 2023-03-23 17:37_

(But, as long as we have this behavior, I think this change is correct.)

---

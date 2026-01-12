```yaml
number: 6376
title: "Use `parenthesized_with_dangling_comments` in arguments formatter"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/arguments-dangling
created_at: 2023-08-06T15:44:06Z
updated_at: 2023-08-07T13:43:59Z
url: https://github.com/astral-sh/ruff/pull/6376
synced_at: 2026-01-12T02:52:04Z
```

# Use `parenthesized_with_dangling_comments` in arguments formatter

---

_Pull request opened by @charliermarsh on 2023-08-06 15:44_

## Summary

Fixes an instability whereby this:

```python
def get_recent_deployments(threshold_days: int) -> Set[str]:
    # Returns a list of deployments not older than threshold days
    # including `/root/zulip` directory if it exists.
    recent = set()
    threshold_date = datetime.datetime.now() - datetime.timedelta(  # noqa: DTZ005
        days=threshold_days
    )
```

Was being formatted as:

```python
def get_recent_deployments(threshold_days: int) -> Set[str]:
    # Returns a list of deployments not older than threshold days
    # including `/root/zulip` directory if it exists.
    recent = set()
    threshold_date = (
        datetime.datetime.now()
        - datetime.timedelta(days=threshold_days)  # noqa: DTZ005
    )
```

Which was in turn being formatted as:

```python
def get_recent_deployments(threshold_days: int) -> Set[str]:
    # Returns a list of deployments not older than threshold days
    # including `/root/zulip` directory if it exists.
    recent = set()
    threshold_date = (
        datetime.datetime.now() - datetime.timedelta(days=threshold_days)  # noqa: DTZ005
    )
```

The second-to-third formattings still differs from Black because we aren't taking the line suffix into account when splitting (https://github.com/astral-sh/ruff/issues/6377), but the first formatting is correct and should be unchanged (i.e., the first-to-second formattings is incorrect, and fixed here).

## Test Plan

`cargo run --bin ruff_dev -- format-dev --stability-check ../zulip`


---

_Review requested from @konstin by @charliermarsh on 2023-08-06 15:44_

---

_Label `formatter` added by @charliermarsh on 2023-08-06 15:45_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/arguments.rs`:103 on 2023-08-06 15:49_

@konstin - Is this still applicable? I can't tell what it applies to.

---

_@charliermarsh reviewed on 2023-08-06 15:49_

---

_Comment by @github-actions[bot] on 2023-08-06 16:14_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.0±0.31ms     4.1 MB/sec    1.00      9.6±0.29ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.02  1964.7±58.68µs     8.5 MB/sec    1.00  1920.5±69.43µs     8.7 MB/sec
formatter/numpy/globals.py                 1.14   248.9±30.77µs    11.9 MB/sec    1.00   218.8±12.44µs    13.5 MB/sec
formatter/pydantic/types.py                1.04      4.2±0.17ms     6.1 MB/sec    1.00      4.1±0.13ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.05     13.1±0.28ms     3.1 MB/sec    1.00     12.5±0.35ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.5±0.10ms     4.8 MB/sec    1.00      3.3±0.10ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   479.9±23.80µs     6.1 MB/sec    1.00   479.1±32.34µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.16ms     4.4 MB/sec    1.00      5.8±0.20ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.04      6.8±0.18ms     6.0 MB/sec    1.00      6.5±0.24ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1455.3±33.06µs    11.4 MB/sec    1.00  1364.5±43.67µs    12.2 MB/sec
linter/default-rules/numpy/globals.py      1.03    174.9±5.25µs    16.9 MB/sec    1.00    169.5±6.86µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.0±0.08ms     8.5 MB/sec    1.00      2.8±0.10ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.16ms     4.1 MB/sec    1.00      9.8±0.09ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1891.7±16.66µs     8.8 MB/sec    1.01  1901.2±39.51µs     8.8 MB/sec
formatter/numpy/globals.py                 1.01    216.0±7.03µs    13.7 MB/sec    1.00    212.9±4.75µs    13.9 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.2 MB/sec    1.01      4.1±0.06ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.4±0.09ms     3.3 MB/sec    1.00     12.4±0.10ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.6±5.13µs     6.9 MB/sec    1.00    427.1±6.47µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.05ms     4.4 MB/sec    1.00      5.7±0.06ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.10ms     6.0 MB/sec    1.00      6.6±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1390.9±14.61µs    12.0 MB/sec    1.00  1380.9±13.75µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    161.7±9.39µs    18.2 MB/sec    1.00    160.1±2.04µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.03ms     8.5 MB/sec    1.00      3.0±0.03ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-07 02:48_

The next stability failure (i.e., the reason this PR is still failing) is fixed in https://github.com/astral-sh/ruff/pull/6380.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:109 on 2023-08-07 07:55_

Nit: Can we change the builder to `parenthesized("(", &group(&all_arguments)), ")").with_dangling_comments(dangling_comments)` to be more in line with how other builders work?

---

_@MichaReiser approved on 2023-08-07 07:56_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/arguments.rs`:103 on 2023-08-07 08:20_

i don't think think so

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/arguments.rs`:109 on 2023-08-07 08:22_

i'm thinking about `parenthesized` and `parenthesized_without_dangling_comments` since the latter should be the exception

---

_@konstin approved on 2023-08-07 08:22_

---

_@charliermarsh reviewed on 2023-08-07 13:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/arguments.rs`:109 on 2023-08-07 13:43_

I will handle separately since it would be unrelated to this change.

---

_Merged by @charliermarsh on 2023-08-07 13:43_

---

_Closed by @charliermarsh on 2023-08-07 13:43_

---

_Branch deleted on 2023-08-07 13:43_

---

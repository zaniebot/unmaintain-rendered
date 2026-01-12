```yaml
number: 6072
title: "Fix `SIM102` to handle indented `elif`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-SIM102-indented-elif
created_at: 2023-07-25T15:14:57Z
updated_at: 2023-07-26T12:37:33Z
url: https://github.com/astral-sh/ruff/pull/6072
synced_at: 2026-01-12T03:30:22Z
```

# Fix `SIM102` to handle indented `elif`

---

_Pull request opened by @harupy on 2023-07-25 15:14_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The `SIM102` auto-fix fails if `elif` is indented like this:

## Example

```python
def f():
    # SIM102
    if a:
        pass
    elif b:
        if c:
            d
```

```
> cargo run -p ruff_cli -- check --select SIM102 --fix a.py
...
error: Failed to fix nested if: Failed to extract statement from source
a.py:5:5: SIM102 Use a single `if` statement instead of nested `if` statements
Found 1 error.
```

## Test Plan

<!-- How was it tested? -->

New test

---

_Renamed from "Fix `SIM102` to handle indented elif" to "Fix `SIM102` to handle indented `elif`" by @harupy on 2023-07-25 15:15_

---

_Review requested from @konstin by @charliermarsh on 2023-07-25 15:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/fix_if.rs`:48 on 2023-07-25 15:18_

@konstin - Can we avoid this now, after the elif refactor?

---

_@charliermarsh reviewed on 2023-07-25 15:18_

---

_Comment by @github-actions[bot] on 2023-07-25 15:24_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -3, 0 error(s))

<details><summary>airflow (+2, -2)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6b113816f509980ce5cd9389305a66b4203d8018/airflow/providers/slack/hooks/slack_webhook.py#L261'>airflow/providers/slack/hooks/slack_webhook.py:261:9:</a> SIM102 Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/6b113816f509980ce5cd9389305a66b4203d8018/airflow/providers/slack/hooks/slack_webhook.py#L261'>airflow/providers/slack/hooks/slack_webhook.py:261:9:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
- <a href='https://github.com/apache/airflow/blob/6b113816f509980ce5cd9389305a66b4203d8018/airflow/ti_deps/deps/trigger_rule_dep.py#L252'>airflow/ti_deps/deps/trigger_rule_dep.py:252:17:</a> SIM102 Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/apache/airflow/blob/6b113816f509980ce5cd9389305a66b4203d8018/airflow/ti_deps/deps/trigger_rule_dep.py#L252'>airflow/ti_deps/deps/trigger_rule_dep.py:252:17:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/core/validation/check.py#L215'>src/bokeh/core/validation/check.py:215:5:</a> SIM102 Use a single `if` statement instead of nested `if` statements
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/core/validation/check.py#L215'>src/bokeh/core/validation/check.py:215:5:</a> SIM102 [*] Use a single `if` statement instead of nested `if` statements
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM102 | 6 | 3 | 3 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.0±0.24ms     4.5 MB/sec    1.08      9.8±0.25ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1752.8±33.93µs     9.5 MB/sec    1.12  1964.8±37.84µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00   198.8±10.15µs    14.8 MB/sec    1.23   243.7±20.53µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.06ms     6.8 MB/sec    1.13      4.3±0.10ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.03     14.2±0.31ms     2.9 MB/sec    1.00     13.8±0.27ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.7±0.08ms     4.5 MB/sec    1.00      3.5±0.09ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   470.5±16.01µs     6.3 MB/sec    1.02   477.7±15.39µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.24ms     4.0 MB/sec    1.00      6.3±0.11ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.21ms     6.0 MB/sec    1.03      7.0±0.08ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1449.4±45.69µs    11.5 MB/sec    1.05  1525.2±33.55µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.0±6.54µs    18.3 MB/sec    1.12    180.8±4.00µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.09ms     9.0 MB/sec    1.15      3.3±0.09ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.11ms     4.0 MB/sec    1.04     10.7±0.09ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1964.5±30.58µs     8.5 MB/sec    1.08      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    215.3±5.11µs    13.7 MB/sec    1.13   243.7±10.00µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     5.9 MB/sec    1.09      4.7±0.08ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.18ms     2.8 MB/sec    1.03     14.8±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.03ms     4.4 MB/sec    1.05      3.9±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    448.5±6.24µs     6.6 MB/sec    1.06    475.4±5.95µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.06ms     4.0 MB/sec    1.06      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.07ms     5.6 MB/sec    1.09      7.9±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1479.0±14.78µs    11.3 MB/sec    1.12  1663.1±15.63µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.8±2.29µs    17.7 MB/sec    1.15    192.2±2.49µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     8.1 MB/sec    1.12      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin reviewed on 2023-07-26 09:35_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/fix_if.rs`:48 on 2023-07-26 09:35_

yep, the following patch avoid looking into the string for the elif: https://gist.github.com/konstin/4baa8825689d7e41ae74d0a4b796f4a5

---

_Comment by @konstin on 2023-07-26 09:36_

@harupy Good catch with the missing autofix! What would thing about using https://gist.github.com/konstin/4baa8825689d7e41ae74d0a4b796f4a5 as implementation instead to avoid checking the source for the `elif`?

---

_Comment by @harupy on 2023-07-26 10:35_

@konstin Looks good to me :) Can we use that patch?

---

_Comment by @konstin on 2023-07-26 10:39_

Yep! Do you want to update your PR or should i make a separate PR?

---

_Comment by @harupy on 2023-07-26 10:58_

@konstin Updated!

---

_@konstin approved on 2023-07-26 12:37_

thanks!

---

_Merged by @konstin on 2023-07-26 12:37_

---

_Closed by @konstin on 2023-07-26 12:37_

---

```yaml
number: 4607
title: Make B007 fix relevance stricter
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/after
created_at: 2023-05-23T15:32:49Z
updated_at: 2023-05-23T16:07:29Z
url: https://github.com/astral-sh/ruff/pull/4607
synced_at: 2026-01-12T03:50:03Z
```

# Make B007 fix relevance stricter

---

_Pull request opened by @charliermarsh on 2023-05-23 15:32_

## Summary

We already avoid fixing B007 if the loop variable appears to be used beyond the loop body. This PR makes the logic a bit more conservative, also avoiding the fix for usages of any shadowed bindings (since we can't know for certain whether the shadowing binding _actually_ took affect, as in the `else: bar = 1` case seen in the linked issue).

Closes #4599.


---

_Comment by @charliermarsh on 2023-05-23 15:33_

I'd like to make this a manual fix, but it doesn't really "fix" the issue as long as manual fixes are still applied.

---

_Merged by @charliermarsh on 2023-05-23 15:43_

---

_Closed by @charliermarsh on 2023-05-23 15:44_

---

_Branch deleted on 2023-05-23 15:44_

---

_Comment by @github-actions[bot] on 2023-05-23 15:48_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -6, 0 error(s))

<details><summary>airflow (+2, -2)</summary>
<p>

```diff
+ tests/integration/providers/apache/kafka/operators/test_consume.py:128:13: B007 Loop control variable `x` not used within loop body
- tests/integration/providers/apache/kafka/operators/test_consume.py:128:13: B007 [*] Loop control variable `x` not used within loop body
+ tests/integration/providers/apache/kafka/triggers/test_await_message.py:68:13: B007 Loop control variable `x` not used within loop body
- tests/integration/providers/apache/kafka/triggers/test_await_message.py:68:13: B007 [*] Loop control variable `x` not used within loop body
```

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

```diff
+ src/bokeh/core/has_props.py:718:16: B007 Loop control variable `v` not used within loop body
- src/bokeh/core/has_props.py:718:16: B007 [*] Loop control variable `v` not used within loop body
```

</p>
</details>
<details><summary>zulip (+3, -3)</summary>
<p>

```diff
+ analytics/lib/fixtures.py:46:13: B007 Loop control variable `i` not used within loop body
- analytics/lib/fixtures.py:46:13: B007 [*] Loop control variable `i` not used within loop body
+ analytics/lib/fixtures.py:67:9: B007 Loop control variable `i` not used within loop body
- analytics/lib/fixtures.py:67:9: B007 [*] Loop control variable `i` not used within loop body
+ zerver/data_import/gitter.py:192:9: B007 Loop control variable `stream` not used within loop body
- zerver/data_import/gitter.py:192:9: B007 [*] Loop control variable `stream` not used within loop body
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B007 | 12 | 6 | 6 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.12ms     2.4 MB/sec    1.01     16.9±0.15ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec    1.02      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.0±8.60µs     6.0 MB/sec    1.00    493.8±3.41µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.2±0.34ms     3.5 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.04ms     5.1 MB/sec    1.00      7.9±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1698.7±9.71µs     9.8 MB/sec    1.01  1717.0±32.48µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.03   196.9±12.61µs    15.0 MB/sec    1.00    191.6±3.98µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.02ms     7.2 MB/sec    1.01      3.6±0.02ms     7.2 MB/sec
parser/large/dataset.py                    1.02      6.2±0.03ms     6.5 MB/sec    1.00      6.1±0.11ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1226.7±7.11µs    13.6 MB/sec    1.01   1233.9±4.55µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    126.8±2.19µs    23.3 MB/sec    1.00    126.2±0.86µs    23.4 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.00ms     9.5 MB/sec    1.00      2.7±0.04ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.1±1.08ms     2.0 MB/sec    1.09     21.8±0.96ms  1912.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.22ms     3.2 MB/sec    1.04      5.4±0.24ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   645.8±47.31µs     4.6 MB/sec    1.01   655.2±25.99µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.40ms     2.8 MB/sec    1.00      9.2±0.37ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.58ms     4.2 MB/sec    1.12     10.8±0.62ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.2±0.12ms     7.5 MB/sec    1.00      2.1±0.15ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.04   272.8±23.67µs    10.8 MB/sec    1.00   261.7±21.56µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.09      4.9±0.31ms     5.2 MB/sec    1.00      4.5±0.31ms     5.7 MB/sec
parser/large/dataset.py                    1.02      8.7±0.29ms     4.7 MB/sec    1.00      8.5±0.44ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.09  1660.3±58.04µs    10.0 MB/sec    1.00  1525.5±74.30µs    10.9 MB/sec
parser/numpy/globals.py                    1.00    168.8±9.42µs    17.5 MB/sec    1.02    173.0±7.49µs    17.1 MB/sec
parser/pydantic/types.py                   1.00      3.7±0.14ms     7.0 MB/sec    1.00      3.7±0.15ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

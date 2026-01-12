```yaml
number: 5797
title: "Remove `Identifier` usages for isolating exception names"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/except-handler-name
created_at: 2023-07-16T04:37:02Z
updated_at: 2023-07-16T05:16:38Z
url: https://github.com/astral-sh/ruff/pull/5797
synced_at: 2026-01-12T03:30:21Z
```

# Remove `Identifier` usages for isolating exception names

---

_Pull request opened by @charliermarsh on 2023-07-16 04:37_

## Summary

The motivating change here is to remove `let range = except_handler.try_identifier().unwrap();` and instead just do `name.range()`, since exception names now have ranges attached to them by the parse. This also required some refactors (which are improvements) to the built-in attribute shadowing rules, since at least one invocation relied on passing in the exception handler and calling `.try_identifier()`. Now that we have easy access to identifiers, we can remove the whole `AnyShadowing` abstraction.

---

_Label `internal` added by @charliermarsh on 2023-07-16 04:37_

---

_Merged by @charliermarsh on 2023-07-16 04:49_

---

_Closed by @charliermarsh on 2023-07-16 04:49_

---

_Branch deleted on 2023-07-16 04:49_

---

_Comment by @github-actions[bot] on 2023-07-16 04:54_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -1, 0 error(s))

<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/callbacks.py#L25'>src/bokeh/models/callbacks.py:25:1:</a> A001 Variable `any` is shadowing a Python builtin
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/models/callbacks.py#L25'>src/bokeh/models/callbacks.py:25:42:</a> A001 Variable `any` is shadowing a Python builtin
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| A001 | 2 | 1 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.36ms     3.6 MB/sec    1.10     12.3±0.06ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.01ms     7.5 MB/sec    1.08      2.4±0.01ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00    249.9±2.51µs    11.8 MB/sec    1.05    263.4±2.10µs    11.2 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.02ms     5.3 MB/sec    1.07      5.2±0.02ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.09ms     2.6 MB/sec    1.00     16.0±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    539.9±2.29µs     5.5 MB/sec    1.01    542.6±1.44µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.03ms     3.5 MB/sec    1.00      7.2±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.03ms     5.1 MB/sec    1.01      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1763.2±5.06µs     9.4 MB/sec    1.01   1778.4±6.30µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.8±1.30µs    14.5 MB/sec    1.01    204.9±0.84µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.01      3.7±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.11ms     3.7 MB/sec    1.09     12.0±1.82ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.06ms     7.6 MB/sec    1.01      2.2±0.03ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    245.0±5.41µs    12.0 MB/sec    1.02    250.4±9.68µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.07ms     5.4 MB/sec    1.01      4.8±0.05ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     16.0±0.33ms     2.5 MB/sec    1.00     15.8±0.17ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.09ms     4.0 MB/sec    1.01      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    504.3±8.30µs     5.9 MB/sec    1.01    509.4±7.36µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.12ms     3.6 MB/sec    1.00      7.2±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     5.0 MB/sec    1.00      8.1±0.11ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1737.8±22.66µs     9.6 MB/sec    1.00  1735.3±18.00µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.5±4.69µs    14.4 MB/sec    1.07   218.1±75.19µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.02      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

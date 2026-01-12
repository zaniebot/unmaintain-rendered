```yaml
number: 5651
title: "[`pylint`] Implement Pylint `typevar-name-incorrect-variance` (`C0105`) "
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: incorrect-variance
created_at: 2023-07-10T14:25:24Z
updated_at: 2023-07-10T17:12:55Z
url: https://github.com/astral-sh/ruff/pull/5651
synced_at: 2026-01-12T15:55:19Z
```

# [`pylint`] Implement Pylint `typevar-name-incorrect-variance` (`C0105`) 

---

_@tjkuson_

## Summary

Implement Pylint `typevar-name-incorrect-variance` (`C0105`)  as `type-name-incorrect-variance` (`PLC0105`). Includes documentation. Related to #970.

The Pylint implementation checks only `TypeVar`, but this PR checks `ParamSpec` as well.

## Test Plan

Added test fixture.

`cargo test`


---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-10 14:25_

---

_Label `rule` added by @charliermarsh on 2023-07-10 14:25_

---

_Comment by @charliermarsh on 2023-07-10 15:20_

Thanks!

---

_Comment by @charliermarsh on 2023-07-10 15:21_

A nice addition in a follow-up PR could be to include the desired name in the diagnostic, like: `Type variable name does not reflect variance. "T" is covariant, use "T_co" instead`. (Pylint omits that part if the variable is both covariant and contravariant, I think.)

---

_Comment by @github-actions[bot] on 2023-07-10 16:02_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>disnake (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/ext/commands/base_core.py#L39'>disnake/ext/commands/base_core.py:39:38:</a> PLC0105 `TypeVar` name "ApplicationCommandInteractionT" does not match variance
+ <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/i18n.py#L53'>disnake/i18n.py:53:11:</a> PLC0105 `TypeVar` name "StringT" does not match variance
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLC0105 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.9±0.35ms     4.1 MB/sec     1.06     10.5±0.51ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.11ms     7.5 MB/sec     1.01      2.2±0.16ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   243.2±13.37µs    12.1 MB/sec     1.05   254.6±15.30µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.21ms     5.2 MB/sec     1.02      5.0±0.24ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.10     19.0±0.98ms     2.1 MB/sec     1.00     17.3±0.70ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.8±0.24ms     3.5 MB/sec     1.00      4.6±0.27ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.03   579.5±24.34µs     5.1 MB/sec     1.00   564.4±28.48µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.05      8.1±0.40ms     3.1 MB/sec     1.00      7.8±0.24ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.33ms     4.8 MB/sec     1.03      8.7±0.35ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1869.9±115.43µs     8.9 MB/sec    1.02  1904.7±111.91µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   229.5±12.87µs    12.9 MB/sec     1.01   230.7±12.75µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.15ms     6.9 MB/sec     1.07      4.0±0.21ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.21     14.2±0.50ms     2.9 MB/sec    1.00     11.8±0.48ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.17      3.0±0.17ms     5.6 MB/sec    1.00      2.5±0.13ms     6.6 MB/sec
formatter/numpy/globals.py                 1.08   323.4±26.13µs     9.1 MB/sec    1.00   300.3±19.03µs     9.8 MB/sec
formatter/pydantic/types.py                1.18      6.6±0.28ms     3.9 MB/sec    1.00      5.5±0.28ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.00     19.7±0.56ms     2.1 MB/sec    1.01     19.8±0.66ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.23ms     3.2 MB/sec    1.00      5.2±0.24ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   621.4±29.07µs     4.7 MB/sec    1.12   692.9±76.47µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.37ms     2.9 MB/sec    1.00      8.8±0.35ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.04     10.5±0.37ms     3.9 MB/sec    1.00     10.0±0.30ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.09ms     7.7 MB/sec    1.00      2.1±0.11ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.06   272.5±16.38µs    10.8 MB/sec    1.00   257.6±17.41µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.6±0.19ms     5.5 MB/sec    1.00      4.6±0.25ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @tjkuson on 2023-07-10 16:05_

> A nice addition in a follow-up PR could be to include the desired name in the diagnostic, like: `Type variable name does not reflect variance. "T" is covariant, use "T_co" instead`. (Pylint omits that part if the variable is both covariant and contravariant, I think.)

I could probably do that.

---

_Merged by @charliermarsh on 2023-07-10 16:28_

---

_Closed by @charliermarsh on 2023-07-10 16:28_

---

_Branch deleted on 2023-07-10 17:12_

---

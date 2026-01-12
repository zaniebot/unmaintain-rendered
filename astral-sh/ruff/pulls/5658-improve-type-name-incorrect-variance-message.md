```yaml
number: 5658
title: "Improve `type-name-incorrect-variance` message"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: better-type-name-message
created_at: 2023-07-10T16:57:29Z
updated_at: 2023-07-16T11:52:11Z
url: https://github.com/astral-sh/ruff/pull/5658
synced_at: 2026-01-12T15:55:19Z
```

# Improve `type-name-incorrect-variance` message

---

_@tjkuson_

## Summary

Change the `type-name-incorrect-variance` diagnostic message to include the detected variance and a name change recommendation. For example,

```
`TypeVar` name "T_co" does not reflect its contravariance; consider renaming it to "T_contra"
```


Related to #5651.

## Test Plan

`cargo test`


---

_Comment by @tjkuson on 2023-07-10 17:05_

I think something went wrong with GitHub Actions.

---

_Comment by @charliermarsh on 2023-07-10 17:21_

What happens in the event that a variable is marked as both covariant and contravariant?

---

_Comment by @github-actions[bot] on 2023-07-10 17:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -2, 0 error(s))

<details><summary>disnake (+2, -2)</summary>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/ext/commands/base_core.py#L39'>disnake/ext/commands/base_core.py:39:38:</a> PLC0105 `TypeVar` name "ApplicationCommandInteractionT" does not match variance
+ <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/ext/commands/base_core.py#L39'>disnake/ext/commands/base_core.py:39:38:</a> PLC0105 `TypeVar` name "ApplicationCommandInteractionT" does not reflect its covariance; consider renaming it to "ApplicationCommandInteractionT_co"
- <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/i18n.py#L53'>disnake/i18n.py:53:11:</a> PLC0105 `TypeVar` name "StringT" does not match variance
+ <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/i18n.py#L53'>disnake/i18n.py:53:11:</a> PLC0105 `TypeVar` name "StringT" does not reflect its covariance; consider renaming it to "StringT_co"
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLC0105 | 4 | 2 | 2 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.07ms     4.7 MB/sec    1.00      8.5±0.08ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01   1822.7±2.95µs     9.1 MB/sec    1.00  1804.7±13.68µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    200.1±3.10µs    14.7 MB/sec    1.00    199.3±0.25µs    14.8 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.2 MB/sec    1.00      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     14.3±0.07ms     2.9 MB/sec    1.00     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.0±1.03µs     8.2 MB/sec    1.00    362.8±0.84µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.00      6.3±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1459.6±1.88µs    11.4 MB/sec    1.01  1467.7±10.88µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.8±0.38µs    18.9 MB/sec    1.00    156.5±0.31µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.5±0.52ms     3.5 MB/sec    1.00     11.4±0.43ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.5±0.10ms     6.6 MB/sec    1.00      2.4±0.09ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00   292.3±18.26µs    10.1 MB/sec    1.01   293.8±27.65µs    10.0 MB/sec
formatter/pydantic/types.py                1.02      5.6±0.22ms     4.6 MB/sec    1.00      5.5±0.18ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.00     19.6±0.76ms     2.1 MB/sec    1.01     19.7±0.58ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.18ms     3.3 MB/sec    1.02      5.1±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   615.3±21.05µs     4.8 MB/sec    1.00   604.8±25.36µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.32ms     2.9 MB/sec    1.01      8.8±0.37ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01      9.7±0.24ms     4.2 MB/sec    1.00      9.6±0.26ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     8.1 MB/sec    1.00      2.0±0.08ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   248.4±18.05µs    11.9 MB/sec    1.00   244.8±23.42µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.5±0.19ms     5.7 MB/sec    1.00      4.3±0.14ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @tjkuson on 2023-07-10 17:31_

> What happens in the event that a variable is marked as both covariant and contravariant?

Currently, it ignores anything that is both covariant and contravariant. However, looking at the Pylint implementation, it emits

```
Type variable name does not reflect variance
```

It should be simple to emit a similar generic message. Do you think it's better to ignore invalid bivariant types or to emit a generic message instead?

---

_Comment by @charliermarsh on 2023-07-10 17:33_

Hmm, I think ignoring them is fine actually, since this diagnostic can't actually be satisfied by renaming the variable.

---

_Merged by @charliermarsh on 2023-07-10 17:33_

---

_Closed by @charliermarsh on 2023-07-10 17:33_

---

_Branch deleted on 2023-07-16 11:52_

---

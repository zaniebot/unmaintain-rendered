```yaml
number: 5795
title: "Use `semantic().global()` to power `global-statement` rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/global
created_at: 2023-07-16T04:16:23Z
updated_at: 2023-07-16T04:47:39Z
url: https://github.com/astral-sh/ruff/pull/5795
synced_at: 2026-01-12T03:30:21Z
```

# Use `semantic().global()` to power `global-statement` rule

---

_Pull request opened by @charliermarsh on 2023-07-16 04:16_

## Summary

The intent of this rule is to always flag the `global` declaration, not the usage. The current implementation does the wrong thing if a global is assigned multiple times. Using `semantic().global()` is also more efficient.

---

_Renamed from "Use `semantic().global()` to power global-statement rule" to "Use `semantic().global()` to power `global-statement` rule" by @charliermarsh on 2023-07-16 04:16_

---

_Label `internal` added by @charliermarsh on 2023-07-16 04:16_

---

_Comment by @github-actions[bot] on 2023-07-16 04:29_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+9, -8, 0 error(s))

<details><summary>airflow (+4, -4)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/airflow/configuration.py#L1483'>airflow/configuration.py:1483:5:</a> PLW0603 Using the global statement to update `FERNET_KEY` is discouraged
- <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/airflow/configuration.py#L1497'>airflow/configuration.py:1497:13:</a> PLW0603 Using the global statement to update `FERNET_KEY` is discouraged
+ <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/airflow/models/crypto.py#L75'>airflow/models/crypto.py:75:5:</a> PLW0603 Using the global statement to update `_fernet` is discouraged
- <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/airflow/models/crypto.py#L84'>airflow/models/crypto.py:84:13:</a> PLW0603 Using the global statement to update `_fernet` is discouraged
+ <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/airflow/serialization/serialized_objects.py#L1476'>airflow/serialization/serialized_objects.py:1476:5:</a> PLW0603 Using the global statement to update `HAS_KUBERNETES` is discouraged
- <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/airflow/serialization/serialized_objects.py#L1491'>airflow/serialization/serialized_objects.py:1491:9:</a> PLW0603 Using the global statement to update `HAS_KUBERNETES` is discouraged
+ <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/dev/breeze/src/airflow_breeze/utils/ci_group.py#L39'>dev/breeze/src/airflow_breeze/utils/ci_group.py:39:5:</a> PLW0603 Using the global statement to update `_in_ci_group` is discouraged
- <a href='https://github.com/apache/airflow/blob/c6594480e2722513fd082a6c65e30e2504698ba2/dev/breeze/src/airflow_breeze/utils/ci_group.py#L50'>dev/breeze/src/airflow_breeze/utils/ci_group.py:50:5:</a> PLW0603 Using the global statement to update `_in_ci_group` is discouraged
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/compiler.py#L424'>src/bokeh/util/compiler.py:424:5:</a> PLW0603 Using the global statement to update `_npmjs` is discouraged
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/util/compiler.py#L426'>src/bokeh/util/compiler.py:426:9:</a> PLW0603 Using the global statement to update `_npmjs` is discouraged
</pre>

</p>
</details>
<details><summary>disnake (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/1c52fe0baab788043d88fea183cc33be9c60b187/disnake/opus.py#L237'>disnake/opus.py:237:47:</a> RUF100 [*] Unused `noqa` directive (unused: `PLW0603`)
+ <a href='https://github.com/DisnakeDev/disnake/blob/1c52fe0baab788043d88fea183cc33be9c60b187/disnake/opus.py#L242'>disnake/opus.py:242:42:</a> RUF100 [*] Unused `noqa` directive (unused: `PLW0603`)
</pre>

</p>
</details>
<details><summary>zulip (+2, -3)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1cd587d24be1d668fcf6d136172bfec69e35cb75/zerver/lib/cache.py#L115'>zerver/lib/cache.py:115:5:</a> PLW0603 Using the global statement to update `KEY_PREFIX` is discouraged
- <a href='https://github.com/zulip/zulip/blob/1cd587d24be1d668fcf6d136172bfec69e35cb75/zerver/lib/cache.py#L116'>zerver/lib/cache.py:116:5:</a> PLW0603 Using the global statement to update `KEY_PREFIX` is discouraged
+ <a href='https://github.com/zulip/zulip/blob/1cd587d24be1d668fcf6d136172bfec69e35cb75/zerver/lib/templates.py#L107'>zerver/lib/templates.py:107:5:</a> PLW0603 Using the global statement to update `md_macro_extension` is discouraged
- <a href='https://github.com/zulip/zulip/blob/1cd587d24be1d668fcf6d136172bfec69e35cb75/zerver/lib/templates.py#L147'>zerver/lib/templates.py:147:9:</a> PLW0603 Using the global statement to update `md_macro_extension` is discouraged
- <a href='https://github.com/zulip/zulip/blob/1cd587d24be1d668fcf6d136172bfec69e35cb75/zerver/lib/templates.py#L151'>zerver/lib/templates.py:151:9:</a> PLW0603 Using the global statement to update `md_macro_extension` is discouraged
</pre>

</p>
</details>
Rules changed: 2

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLW0603 | 15 | 7 | 8 |
| RUF100 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.11ms     3.7 MB/sec    1.01     11.0±0.09ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.6 MB/sec    1.01      2.2±0.02ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   260.2±19.51µs    11.3 MB/sec    1.00   259.2±11.56µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.06ms     5.4 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.16ms     2.6 MB/sec    1.01     15.9±0.39ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.1 MB/sec    1.01      4.1±0.16ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    530.5±5.84µs     5.6 MB/sec    1.01    535.1±3.88µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.00      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.06ms     5.1 MB/sec    1.02      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1727.0±15.65µs     9.6 MB/sec    1.03  1773.9±14.37µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   214.7±14.04µs    13.7 MB/sec    1.01    217.0±8.79µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.2 MB/sec    1.06      3.8±0.10ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.06ms     3.7 MB/sec    1.01     11.1±0.18ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.01ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    228.8±1.94µs    12.9 MB/sec    1.00    228.9±2.15µs    12.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.03ms     5.4 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.07ms     2.6 MB/sec    1.01     15.5±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.01      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.8±5.04µs     6.9 MB/sec    1.01    430.9±5.18µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.6 MB/sec    1.00      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1661.3±30.86µs    10.0 MB/sec    1.00  1657.3±14.78µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.7±1.07µs    16.4 MB/sec    1.00    179.9±1.97µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.05ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-16 04:34_

---

_Closed by @charliermarsh on 2023-07-16 04:34_

---

_Branch deleted on 2023-07-16 04:34_

---

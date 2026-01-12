```yaml
number: 3969
title: "Implement `unnecessary-literal-within-dict-call` (`C418`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/C418
created_at: 2023-04-13T23:38:54Z
updated_at: 2023-04-14T01:50:18Z
url: https://github.com/astral-sh/ruff/pull/3969
synced_at: 2026-01-12T15:55:14Z
```

# Implement `unnecessary-literal-within-dict-call` (`C418`)

---

_@charliermarsh_

Closes #3964.

---

_Label `rule` added by @charliermarsh on 2023-04-13 23:38_

---

_Comment by @github-actions[bot] on 2023-04-14 00:06_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>airflow (+3, -0)</summary>
<p>

```diff
+ tests/serialization/test_serde.py:224:16: C418 [*] Unnecessary `dict` literal passed to `dict()` (remove the outer call to `dict()`)
+ tests/serialization/test_serde.py:234:16: C418 [*] Unnecessary `dict` literal passed to `dict()` (remove the outer call to `dict()`)
+ tests/serialization/test_serde.py:55:16: C418 [*] Unnecessary `dict` literal passed to `dict()` (remove the outer call to `dict()`)
```

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

```diff
+ zerver/lib/scim.py:100:13: C418 [*] Unnecessary `dict` literal passed to `dict()` (remove the outer call to `dict()`)
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.05ms     2.9 MB/sec    1.00     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.1±4.60µs     6.4 MB/sec    1.00    458.4±1.49µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.01ms     5.6 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1617.0±1.89µs    10.3 MB/sec    1.00   1623.8±1.51µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.5±0.38µs    16.4 MB/sec    1.01    181.7±2.06µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.33ms     2.1 MB/sec    1.03     20.2±0.42ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.16ms     3.2 MB/sec    1.01      5.2±0.12ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   614.2±18.41µs     4.8 MB/sec    1.03   630.6±24.88µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.21ms     3.0 MB/sec    1.01      8.6±0.25ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.19ms     4.0 MB/sec    1.03     10.4±0.22ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.5 MB/sec    1.02      2.3±0.06ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    257.2±9.81µs    11.5 MB/sec    1.02   262.1±15.03µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.09ms     5.5 MB/sec    1.02      4.8±0.10ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @dhruvmanila on 2023-04-14 01:22_

Got to re-generate the schema :)

---

_Merged by @charliermarsh on 2023-04-14 01:39_

---

_Closed by @charliermarsh on 2023-04-14 01:39_

---

_Branch deleted on 2023-04-14 01:39_

---

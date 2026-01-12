```yaml
number: 3517
title: Avoid C1901 violations within subscripts
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/equals-str
created_at: 2023-03-14T18:01:29Z
updated_at: 2023-03-17T03:12:51Z
url: https://github.com/astral-sh/ruff/pull/3517
synced_at: 2026-01-12T04:39:45Z
```

# Avoid C1901 violations within subscripts

---

_Pull request opened by @charliermarsh on 2023-03-14 18:01_

## Summary

I'm not sold on merging this, but... this PR turns off "compare-to-empty-string" violations when such comparisons are used within a subscript, like `df[x == ""]`. This is a common idiom in Pandas especially, where `x == ""` is an elementwise comparison, and can't be replaced with `x` or `bool(x)` or whatever.

The downside here is that we trade off some false positives for false negatives.

Closes #3494.


---

_Review requested from @konstin by @charliermarsh on 2023-03-14 18:01_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-14 18:01_

---

_Comment by @charliermarsh on 2023-03-14 18:02_

Open to feedback on how we should approach these kinds of issues. We don't have enough typing information to know for certain that the comparison is "valid", but my guess is that it's also pretty unlikely in practice that this kind of comparison within brackets would be used for anything else.

---

_Comment by @github-actions[bot] on 2023-03-14 18:14_

ℹ️ ecosystem check **detected changes**. (+0, -2, 0 error(s))

<details><summary>airflow (+0, -2)</summary>
<p>

```diff
- airflow/providers/apache/hive/transfers/mssql_to_hive.py:118:64: PLC1901 `field[0] == ""` can be simplified to `not field[0]` as an empty string is falsey
- airflow/providers/apache/hive/transfers/vertica_to_hive.py:122:56: PLC1901 `field[0] == ""` can be simplified to `not field[0]` as an empty string is falsey
```

</p>
</details>

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:255 on 2023-03-15 08:29_

Nit: Call it `expr_ancestors` to clarify that it doesn't iterate over the expressions but the ancestor tree?

---

_@MichaReiser approved on 2023-03-15 08:29_

Is this another use case where the concept of dialects or knowing about the used frameworks could help?

---

_Merged by @charliermarsh on 2023-03-17 02:52_

---

_Closed by @charliermarsh on 2023-03-17 02:52_

---

_Branch deleted on 2023-03-17 02:52_

---

_Comment by @github-actions[bot] on 2023-03-17 02:57_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -2, 0 error(s))

<details><summary>airflow (+0, -2)</summary>
<p>

```diff
- airflow/providers/apache/hive/transfers/mssql_to_hive.py:118:64: PLC1901 `field[0] == ""` can be simplified to `not field[0]` as an empty string is falsey
- airflow/providers/apache/hive/transfers/vertica_to_hive.py:122:56: PLC1901 `field[0] == ""` can be simplified to `not field[0]` as an empty string is falsey
```

</p>
</details>
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      7.6±0.01ms     5.3 MB/sec    1.01      7.7±0.01ms     5.3 MB/sec
linter/numpy/ctypeslib.py    1.00      2.6±0.00ms   132.9 MB/sec    1.00      2.6±0.00ms   133.1 MB/sec
linter/numpy/globals.py      1.00   1324.4±0.99µs   134.6 MB/sec    1.00   1327.5±2.92µs   134.3 MB/sec
linter/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.01      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                        main                                    pr
-----                        ----                                    --
linter/large/dataset.py      1.00     10.6±0.77ms     3.8 MB/sec     1.01     10.8±0.38ms     3.8 MB/sec
linter/numpy/ctypeslib.py    1.00      3.5±0.22ms    97.8 MB/sec     1.01      3.6±0.16ms    96.7 MB/sec
linter/numpy/globals.py      1.00  1809.0±107.48µs    98.5 MB/sec    1.04  1880.8±78.61µs    94.8 MB/sec
linter/pydantic/types.py     1.00      4.9±0.26ms     5.2 MB/sec     1.00      4.9±0.21ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

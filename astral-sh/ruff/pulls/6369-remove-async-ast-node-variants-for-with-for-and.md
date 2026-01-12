```yaml
number: 6369
title: "Remove async AST node variants for `with`, `for`, and `def`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/async
created_at: 2023-08-05T22:56:30Z
updated_at: 2023-08-07T16:54:10Z
url: https://github.com/astral-sh/ruff/pull/6369
synced_at: 2026-01-12T15:55:21Z
```

# Remove async AST node variants for `with`, `for`, and `def`

---

_@charliermarsh_

## Summary

Per the suggestion in https://github.com/astral-sh/ruff/discussions/6183, this PR removes `AsyncWith`, `AsyncFor`, and `AsyncFunctionDef`, replacing them with an `is_async` field on the non-async variants of those structs. Unlike an interpreter, we _generally_ have identical handling for these nodes, so separating them into distinct variants adds complexity from which we don't really benefit. This can be seen below, where we get to remove a _ton_ of code related to adding generic `Any*` wrappers, and a ton of duplicate branches  for these cases.

## Test Plan

`cargo test` is unchanged, apart from parser snapshots.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-05 22:56_

---

_Comment by @charliermarsh on 2023-08-05 22:57_

You love to see it:

<img width="146" alt="Screen Shot 2023-08-05 at 6 56 58 PM" src="https://github.com/astral-sh/ruff/assets/1309177/d53637d3-552a-4f0e-bcdd-4b01efc8be7c">


---

_Comment by @github-actions[bot] on 2023-08-05 23:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>disnake (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/634f25f3b92f0507b0b7a088b55ba891dd2fd758/disnake/ext/commands/context.py#L293'>disnake/ext/commands/context.py:293:9:</a> D402 First line should not be the function's signature
</pre>

</p>
</details>
<details><summary>airflow (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/20e5c8a001a8b4b88c01fb9edd4a41f2eb9c5c5d/airflow/providers/http/hooks/http.py#L297'>airflow/providers/http/hooks/http.py:297:15:</a> PLR0915 Too many statements (56 > 50)
+ <a href='https://github.com/apache/airflow/blob/20e5c8a001a8b4b88c01fb9edd4a41f2eb9c5c5d/tests/providers/telegram/hooks/test_telegram.py#L35'>tests/providers/telegram/hooks/test_telegram.py:35:16:</a> UP008 [*] Use `super()` instead of `super(__class__, self)`
</pre>

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/db3b9e4742d508a9e8e9561115972028f58312bd/zerver/management/commands/runtornado.py#L47'>zerver/management/commands/runtornado.py:47:9:</a> PLR0915 Too many statements (59 > 50)
</pre>

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR0915 | 2 | 2 | 0 |
| D402 | 1 | 1 | 0 |
| UP008 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.04ms     4.0 MB/sec    1.00     10.1±0.06ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.04ms     8.3 MB/sec    1.00      2.0±0.01ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    226.4±2.63µs    13.0 MB/sec    1.00    227.1±3.18µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.02ms     6.0 MB/sec    1.01      4.3±0.04ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.02ms     3.2 MB/sec    1.00     12.6±0.03ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    471.7±0.81µs     6.3 MB/sec    1.00    469.2±2.37µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.03ms     6.2 MB/sec    1.01      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1396.1±2.09µs    11.9 MB/sec    1.01   1410.6±6.54µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.8±0.95µs    18.3 MB/sec    1.00    160.7±0.78µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.0±0.25ms     3.4 MB/sec    1.02     12.3±0.23ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.07ms     7.2 MB/sec    1.00      2.3±0.04ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00    251.5±5.57µs    11.7 MB/sec    1.02   257.3±13.26µs    11.5 MB/sec
formatter/pydantic/types.py                1.01      5.1±0.11ms     5.0 MB/sec    1.00      5.1±0.09ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.27ms     2.6 MB/sec    1.02     15.7±0.28ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.0 MB/sec    1.03      4.3±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    506.0±8.76µs     5.8 MB/sec    1.02    514.9±8.31µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.12ms     3.7 MB/sec    1.04      7.2±0.14ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.17ms     4.9 MB/sec    1.01      8.3±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1695.6±29.45µs     9.8 MB/sec    1.01  1718.9±36.06µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.5±4.06µs    15.2 MB/sec    1.01    196.7±6.77µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-06 15:11_

(The formatter stability error is happening on `main`.)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/analyze/statement.rs`:146 on 2023-08-07 06:50_

Nit: Could we change these rules to accept the function statement to avoid passing standalone boolean values (OK as a separate PR)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:81 on 2023-08-07 06:54_

Nit: I always get the impression that some part is missing from variables named `with_`. Why not `with_statement`?

```suggestion
    with_statetment &ast::StmtWith,
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/fix_with.rs`:17 on 2023-08-07 06:55_

Nit:
```suggestion
    with_stmt: &ast::StmtWith,
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/redefined_loop_name.rs`:160 on 2023-08-07 06:57_

Should this be gated to sync `With` statements only?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/redefined_loop_name.rs`:216 on 2023-08-07 06:57_

Should this be gated to sync `FunctionDef`s only?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/yield_from_in_async_function.rs`:40 on 2023-08-07 06:59_

Is the change from `is_async_function` to `in_async_context` intentional`? 

Should we rename the function to `yield_from_async_context`?

---

_@MichaReiser approved on 2023-08-07 07:03_

Nice, so much deleted code. 

How did you ensure we use `async: false` in all places that should only apply for sync functions/with/for? 

---

_Label `internal` added by @MichaReiser on 2023-08-07 07:04_

---

_@konstin approved on 2023-08-07 07:38_

:100:

And the changes are much less invasive that i feared

---

_@charliermarsh reviewed on 2023-08-07 16:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/statement.rs`:146 on 2023-08-07 16:12_

Yes, I will.

---

_@charliermarsh reviewed on 2023-08-07 16:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/redefined_loop_name.rs`:216 on 2023-08-07 16:13_

No, this was just wrong before. I suspect we had this mistake in a bunch of places.

---

_Comment by @charliermarsh on 2023-08-07 16:14_

> How did you ensure we use async: false in all places that should only apply for sync functions/with/for?

I'm relying on our test coverage for this. It's definitely possible that we now accept async functions in some code paths in which we previously didn't. In some cases, I bet that's correct, and was previously incorrect; in some cases, it might be the opposite. The good news is that, at least for lint rules, we used the same match branch for the async and sync variants, and there are only a few specific rules that had special-casing for async-vs.-sync.


---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/yield_from_in_async_function.rs`:40 on 2023-08-07 16:24_

Going to revert, either _could_ be correct but I think limiting to async functions is closer to the original rule.

---

_@charliermarsh reviewed on 2023-08-07 16:24_

---

_Merged by @charliermarsh on 2023-08-07 16:36_

---

_Closed by @charliermarsh on 2023-08-07 16:36_

---

_Branch deleted on 2023-08-07 16:36_

---

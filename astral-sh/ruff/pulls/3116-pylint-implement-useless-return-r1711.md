```yaml
number: 3116
title: "[`pylint`] Implement `useless-return` (`R1711`)"
type: pull_request
state: merged
author: tomecki
labels:
  - rule
assignees: []
merged: true
base: main
head: useless_return
created_at: 2023-02-22T08:41:44Z
updated_at: 2023-03-17T22:31:37Z
url: https://github.com/astral-sh/ruff/pull/3116
synced_at: 2026-01-12T04:39:44Z
```

# [`pylint`] Implement `useless-return` (`R1711`)

---

_Pull request opened by @tomecki on 2023-02-22 08:41_

This PR implements https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/useless-return.html , including autofixing.

Impl change from original approach: the check locates the actual useless return statements, in contrast to pylint's implemnentation which flags the _functions_ which have useless return statements.

---

_Comment by @tomecki on 2023-02-22 08:46_

Root ticket for pylint coverage: https://github.com/charliermarsh/ruff/issues/970

---

_Review comment by @youknowone on `crates/ruff/src/rules/pylint/rules/useless_return.rs`:85 on 2023-02-25 02:00_

returning here might fit better with another one in line 57-59
```suggestion
    if !is_bare_return_or_none {
        return;
    }
    let mut diagnostic = Diagnostic::new(UselessReturn, Range::from_located(stmt));
    if !checker.patch(diagnostic.kind.rule()) {
        match delete_stmt(
            stmt,
            None,
            &[],
            checker.locator,
            checker.indexer,
            checker.stylist,
        ) {
            Ok(fix) => {
                diagnostic.amend(fix);
            }
            Err(e) => {
                error!("Failed to delete `return` statement: {}", e);
            }
        };
    }
    checker.diagnostics.push(diagnostic);
```

---

_@youknowone reviewed on 2023-02-25 02:01_

---

_Review requested from @youknowone by @tomecki on 2023-02-25 12:00_

---

_Comment by @charliermarsh on 2023-02-25 17:33_

I think the one issue with this rule is that it may conflict with some of the `flake8-return` rules, e.g. those that _require_ an explicit `return None` at the end of the function, like `RET502` and `RET503`. I'm hesitant to introduce rules that are in direct conflict. An alternative would be to make the `flake8-return` rules parameterizable such that they can enforce this behavior if desired, in lieu of enforcing the opposite.


---

_Comment by @r3m0t on 2023-03-16 00:57_

Looks like in pylint the rule ignores functions which can return values.  https://github.com/PyCQA/pylint/blob/1eef2273aee4c5a2e287f7470d6da3a3ae7d0760/pylint/checkers/refactoring/refactoring_checker.py#L2022

Which makes it the same as/very similar to R501 - https://pypi.org/project/flake8-return/

---

_Comment by @charliermarsh on 2023-03-16 02:41_

Oh, in that case, it may actually be different than any existing checks? Since none of those tell you to remove a `return` entirely, only that the return should be implicit or explicit in various cases. (`R501` only tells you to change `return None` to `return` if all returns are `None`, not to drop a `return` / `return None` at the end of a method if it's the only `return`.)

---

_Label `rule` added by @charliermarsh on 2023-03-17 03:51_

---

_Review request for @youknowone removed by @charliermarsh on 2023-03-17 03:51_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-03-17 03:51_

---

_Comment by @charliermarsh on 2023-03-17 18:35_

But we need to adjust this such that it only warns if this is the _only_ return in the function.

---

_Comment by @github-actions[bot] on 2023-03-17 21:19_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+18, -0, 0 error(s))

<details><summary>zulip (+2, -0)</summary>
<p>

```diff
+ zerver/lib/streams.py:243:5: PLR1711 [*] Useless `return` statement at end of function
+ zerver/lib/test_helpers.py:122:9: PLR1711 [*] Useless `return` statement at end of function
```

</p>
</details>
<details><summary>bokeh (+10, -0)</summary>
<p>

```diff
+ src/bokeh/application/application.py:236:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/application/application.py:249:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/client/connection.py:281:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/contexts.py:301:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/contexts.py:321:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/tornado.py:715:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:223:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:262:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:288:9: PLR1711 [*] Useless `return` statement at end of function
+ src/bokeh/server/views/ws.py:335:9: PLR1711 [*] Useless `return` statement at end of function
```

</p>
</details>
<details><summary>airflow (+6, -0)</summary>
<p>

```diff
+ airflow/providers/google/cloud/operators/dataflow.py:1378:9: PLR1711 [*] Useless `return` statement at end of function
+ airflow/providers/microsoft/azure/sensors/data_factory.py:134:9: PLR1711 [*] Useless `return` statement at end of function
+ airflow/utils/db.py:1475:5: PLR1711 [*] Useless `return` statement at end of function
+ dev/check_files.py:223:5: PLR1711 [*] Useless `return` statement at end of function
+ dev/check_files.py:237:5: PLR1711 [*] Useless `return` statement at end of function
+ tests/jobs/test_scheduler_job.py:1924:17: PLR1711 [*] Useless `return` statement at end of function
```

</p>
</details>
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.0±1.03ms     2.3 MB/sec    1.03     18.6±0.89ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.16ms     3.7 MB/sec    1.05      4.7±0.17ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   637.2±21.11µs     4.6 MB/sec    1.02   652.7±24.48µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.39ms     3.2 MB/sec    1.05      8.3±0.26ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.44ms     4.1 MB/sec    1.01     10.0±0.50ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.2±0.11ms     7.6 MB/sec    1.00      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    260.4±7.24µs    11.3 MB/sec    1.02   265.4±22.20µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.13ms     5.7 MB/sec    1.04      4.7±0.17ms     5.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.3±0.33ms     2.2 MB/sec    1.02     18.6±0.40ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.17ms     3.3 MB/sec    1.00      5.0±0.12ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   637.8±21.98µs     4.6 MB/sec    1.01   647.3±33.93µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.28ms     3.1 MB/sec    1.00      8.3±0.19ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.19ms     4.0 MB/sec    1.04     10.6±0.22ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.5 MB/sec    1.04      2.3±0.08ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    248.4±9.31µs    11.9 MB/sec    1.04   257.7±11.80µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.17ms     5.4 MB/sec    1.01      4.8±0.12ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-03-17 21:31_

---

_Merged by @charliermarsh on 2023-03-17 22:30_

---

_Closed by @charliermarsh on 2023-03-17 22:30_

---

_Renamed from "[pylint] R1711: useless-return" to "[`pylint`] Implement `useless-return` (`R1711`)" by @charliermarsh on 2023-03-17 22:30_

---

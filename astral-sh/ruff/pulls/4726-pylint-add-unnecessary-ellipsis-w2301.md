```yaml
number: 4726
title: "[`pylint`] Add `unnecessary_ellipsis` (`W2301`)"
type: pull_request
state: closed
author: hoel-bagard
labels: []
assignees: []
base: main
head: add_W2301
created_at: 2023-05-30T15:17:45Z
updated_at: 2023-11-30T22:14:05Z
url: https://github.com/astral-sh/ruff/pull/4726
synced_at: 2026-01-10T23:40:55Z
```

# [`pylint`] Add `unnecessary_ellipsis` (`W2301`)

---

_Pull request opened by @hoel-bagard on 2023-05-30 15:17_

## Summary

This PR is part of https://github.com/charliermarsh/ruff/issues/970, it adds the `PLW2301` warning rule along with its fix.

## Test Plan

The test fixture uses the one from pylint, whit a few added tests.

---

_Comment by @github-actions[bot] on 2023-05-30 15:27_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+27, -0, 0 error(s))

<details><summary>airflow (+20, -0)</summary>
<p>

```diff
+ airflow/hooks/base.py:158:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/hooks/base.py:178:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/jobs/scheduler_job_runner.py:1542:13: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/metrics/protocols.py:40:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/metrics/protocols.py:44:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/notifications/basenotifier.py:80:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/batch_client.py:119:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/batch_client.py:131:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/batch_client.py:62:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/batch_client.py:88:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/ecs.py:230:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/ecs.py:234:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/ecs.py:238:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/ecs.py:242:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/ecs.py:246:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/providers/amazon/aws/hooks/ecs.py:250:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/serialization/json_schema.py:43:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/serialization/json_schema.py:47:9: PLW2301 [*] Unnecessary ellipsis constant.
+ airflow/serialization/json_schema.py:51:9: PLW2301 [*] Unnecessary ellipsis constant.
+ docs/exts/substitution_extensions.py:118:9: PLW2301 [*] Unnecessary ellipsis constant.
```

</p>
</details>
<details><summary>disnake (+2, -0)</summary>
<p>

```diff
+ examples/interactions/injections.py:96:5: PLW2301 [*] Unnecessary ellipsis constant.
+ tests/test_utils.py:66:9: PLW2301 [*] Unnecessary ellipsis constant.
```

</p>
</details>
<details><summary>scikit-build-core (+5, -0)</summary>
<p>

```diff
+ src/scikit_build_core/settings/sources.py:161:9: PLW2301 [*] Unnecessary ellipsis constant.
+ src/scikit_build_core/settings/sources.py:168:9: PLW2301 [*] Unnecessary ellipsis constant.
+ src/scikit_build_core/settings/sources.py:176:9: PLW2301 [*] Unnecessary ellipsis constant.
+ src/scikit_build_core/settings/sources.py:184:9: PLW2301 [*] Unnecessary ellipsis constant.
+ src/scikit_build_core/settings/sources.py:191:9: PLW2301 [*] Unnecessary ellipsis constant.
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLW2301 | 27 | 27 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.0±0.50ms     2.5 MB/sec    1.04     16.7±0.22ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.10ms     4.3 MB/sec    1.05      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.0±7.89µs     5.9 MB/sec    1.03   515.2±30.74µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.26ms     3.8 MB/sec    1.02      6.9±0.22ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.18ms     5.2 MB/sec    1.01      7.9±0.23ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1746.9±32.62µs     9.5 MB/sec    1.00  1740.7±35.10µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.2±6.94µs    15.4 MB/sec    1.05    200.8±2.85µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.11ms     7.2 MB/sec    1.01      3.6±0.05ms     7.1 MB/sec
parser/large/dataset.py                    1.00      5.9±0.18ms     7.0 MB/sec    1.05      6.1±0.10ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.01  1194.0±25.92µs    13.9 MB/sec    1.00  1182.9±26.71µs    14.1 MB/sec
parser/numpy/globals.py                    1.00    122.5±2.94µs    24.1 MB/sec    1.02    125.3±1.13µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.05ms     9.8 MB/sec    1.02      2.6±0.02ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.05ms     2.7 MB/sec    1.01     15.2±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    390.4±1.44µs     7.6 MB/sec    1.00    390.6±3.66µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.03ms     5.4 MB/sec    1.00      7.6±0.03ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1545.4±5.70µs    10.8 MB/sec    1.00   1535.9±5.01µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.0±0.67µs    17.2 MB/sec    1.00    171.9±1.92µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.12      7.0±0.02ms     5.8 MB/sec    1.00      6.2±0.03ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.11   1277.6±5.90µs    13.0 MB/sec    1.00   1147.9±5.58µs    14.5 MB/sec
parser/numpy/globals.py                    1.09    127.5±0.50µs    23.2 MB/sec    1.00    117.4±0.79µs    25.1 MB/sec
parser/pydantic/types.py                   1.11      2.9±0.01ms     8.9 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs`:151 on 2023-05-30 15:56_

Nit: I think it would be clearer to write this as a `match` instead. It emphasizes that the branches are mutually exclusive: 

```suggestion
		match stmt {
			Stmt::FunctionDef(ast::StmtFunctionDef { body, .. })
	        | Stmt::AsyncFunctionDef(ast::StmtAsyncFunctionDef { body, .. })
	        | Stmt::ClassDef(ast::StmtClassDef { body, .. })
	        | Stmt::AsyncFor(ast::StmtAsyncFor { body, .. })
	        | Stmt::While(ast::StmtWhile { body, .. })
	        | Stmt::With(ast::StmtWith { body, .. })
	        | Stmt::AsyncWith(ast::StmtAsyncWith { body, .. })
	        | Stmt::Try(ast::StmtTry { body, .. })
	        | Stmt::TryStar(ast::StmtTryStar { body, .. }) => process_body(checker, stmt, body, expr),
	        | Stmt::If(ast::StmtIf { body, orelse, .. }) => {
	        	process_body(checker, stmt, body, expr)
	        	process_body(checker, stmt, orelse, expr);	
	        },
	        Stmt::Match(ast::StmtMatch { cases, .. }) => {
	            for case in cases {
	                let ast::MatchCase { body, .. } = case;
	                process_body(checker, stmt, body, expr);
	            }
	        },
	        Stmt::Try(ast::StmtTry { body, handlers ..} => {
	        	process_body(checker, stmt, body, expr)
				for handler in handlers {
	                let Excepthandler::ExceptHandler(ast::ExcepthandlerExceptHandler { body, .. }) =
	                    handler;
	                process_body(checker, stmt, body, expr);
            }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs`:160 on 2023-05-30 15:57_

Do we need to handle the `orelse` and `finally` bodies too?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs`:120 on 2023-05-30 16:00_

I wonder how expensive  `stmt_parent` is when called for every stmt. I rescheduled the benchmarks to see if it is a one-off or is indeed a perf regression.

---

_@MichaReiser reviewed on 2023-05-30 16:01_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs`:151 on 2023-05-30 16:16_

I've tried replacing with the following `match`:
```rust
        match stmt {
            Stmt::FunctionDef(ast::StmtFunctionDef { body, .. })
            | Stmt::AsyncFunctionDef(ast::StmtAsyncFunctionDef { body, .. })
            | Stmt::ClassDef(ast::StmtClassDef { body, .. })
            | Stmt::AsyncFor(ast::StmtAsyncFor { body, .. })
            | Stmt::While(ast::StmtWhile { body, .. })
            | Stmt::With(ast::StmtWith { body, .. })
            | Stmt::AsyncWith(ast::StmtAsyncWith { body, .. })
            | Stmt::If(ast::StmtIf { body, .. })
            | Stmt::Try(ast::StmtTry { body, .. })
            | Stmt::TryStar(ast::StmtTryStar { body, .. }) => {
                process_body(checker, stmt, body, expr);
            }
            Stmt::If(ast::StmtIf { orelse, .. }) => {
                process_body(checker, stmt, orelse, expr);
            }
            Stmt::Match(ast::StmtMatch { cases, .. }) => {
                for case in cases {
                    let ast::MatchCase { body, .. } = case;
                    process_body(checker, stmt, body, expr);
                }
            }
            Stmt::Try(ast::StmtTry { handlers, .. }) => {
                for handler in handlers {
                    let Excepthandler::ExceptHandler(ast::ExcepthandlerExceptHandler {
                        body, ..
                    }) = handler;
                    process_body(checker, stmt, body, expr);
                }
            }
            _ => {}
        }
```

However this results in
```
error: unreachable pattern
   --> crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs:134:13
    |
134 |             Stmt::If(ast::StmtIf { orelse, .. }) => {
    |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
    = note: `-D unreachable-patterns` implied by `-D warnings`

error: unreachable pattern
   --> crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs:143:13
    |
143 |             Stmt::Try(ast::StmtTry { handlers, .. }) => {
    |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

```

---

_@hoel-bagard reviewed on 2023-05-30 16:16_

---

_@hoel-bagard reviewed on 2023-05-30 16:28_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs`:160 on 2023-05-30 16:28_

Fixed in c8050c2.

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pylint/rules/unnecessary_ellipsis.rs`:120 on 2023-05-31 13:16_

@MichaReiser Is it too slow to be merged ?

---

_@hoel-bagard reviewed on 2023-05-31 13:16_

---

_Comment by @charliermarsh on 2023-06-02 03:00_

It looks reasonable though I'm not sure how to deal with the overlap between this rule and `rules/flake8_pyi/rules/ellipsis_in_non_empty_class_body.rs` (which is similar but slightly different in that it only affects class bodies and only in stub files).

---

_Comment by @Skylion007 on 2023-06-02 05:20_

@charliermarsh It would be great if a single rule could alias multiple more specific rules.

---

_Comment by @hoel-bagard on 2023-06-02 06:39_

~~What about introducing a `flake8_pyi` rule for unnecessary ellipsis in function, and have `W2301` be used only for non-stubs files ?  (if that's possible)~~

---

_Comment by @hoel-bagard on 2023-06-17 07:46_

Are there other rules with partial overlap that could be used as reference/precedent ?

---

_Comment by @zanieb on 2023-11-30 22:14_

I think this is superseded by https://github.com/astral-sh/ruff/pull/8641 now. Sorry this one got a little lost :/ let me know if you have any questions or concerns!

---

_Closed by @zanieb on 2023-11-30 22:14_

---

```yaml
number: 15799
title: "[`flake8-logging`] `.exception()` and `exc_info=` outside exception handlers (`LOG004`, `LOG014`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: LOG004
created_at: 2025-01-29T00:46:36Z
updated_at: 2025-02-04T20:22:02Z
url: https://github.com/astral-sh/ruff/pull/15799
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-logging`] `.exception()` and `exc_info=` outside exception handlers (`LOG004`, `LOG014`)

---

_@InSyncWithFoo_

## Summary

Part of #7248.

This change adds two new rules: [`LOG004`](https://github.com/adamchainz/flake8-logging/blob/dbe88e76947450a7c4c1ebfb7a7a44875004f410/README.rst#log004-avoid-exception-outside-of-exception-handlers) and [`LOG014`](https://github.com/adamchainz/flake8-logging/blob/main/README.rst#log014-avoid-exc_infotrue-outside-of-exception-handlers). They are closely related and thus share the same handlers detecting logic. Tests and documentation are adapted from that of [the upstream linter](https://github.com/adamchainz/flake8-logging/tree/dbe88e76947450a7c4c1ebfb7a7a44875004f410).

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-29 00:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+38 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/edge/src/airflow/providers/edge/worker_api/auth.py#L59'>providers/edge/src/airflow/providers/edge/worker_api/auth.py:59:5:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/edge/src/airflow/providers/edge/worker_api/routes/_v2_routes.py#L56'>providers/edge/src/airflow/providers/edge/worker_api/routes/_v2_routes.py:56:5:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/src/airflow/providers/cncf/kubernetes/operators/pod.py#L1290'>providers/src/airflow/providers/cncf/kubernetes/operators/pod.py:1290:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/src/airflow/providers/google/cloud/operators/cloud_sql.py#L1053'>providers/src/airflow/providers/google/cloud/operators/cloud_sql.py:1053:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L1754'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:1754:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L1915'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:1915:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L2379'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:2379:17:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/providers/src/airflow/providers/google/cloud/operators/kubernetes_engine.py#L305'>providers/src/airflow/providers/google/cloud/operators/kubernetes_engine.py:305:13:</a> LOG004 `.exception()` call outside exception handlers
... 4 additional changes omitted for rule LOG004
+ <a href='https://github.com/apache/airflow/blob/930f9d247f55406304f317f92e2d37ecc630e359/task_sdk/src/airflow/sdk/execution_time/supervisor.py#L741'>task_sdk/src/airflow/sdk/execution_time/supervisor.py:741:13:</a> LOG014 `exc_info=` outside exception handlers
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/commands/report/base.py#L106'>superset/commands/report/base.py:106:75:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/db_engine_specs/clickhouse.py#L181'>superset/db_engine_specs/clickhouse.py:181:17:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/db_engine_specs/hive.py#L592'>superset/db_engine_specs/hive.py:592:13:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/sql_lab.py#L137'>superset/sql_lab.py:137:69:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/sql_lab.py#L141'>superset/sql_lab.py:141:75:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/tasks/cache.py#L298'>superset/tasks/cache.py:298:31:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/utils/core.py#L572'>superset/utils/core.py:572:43:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/utils/decorators.py#L231'>superset/utils/decorators.py:231:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/views/error_handling.py#L140'>superset/views/error_handling.py:140:50:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/views/error_handling.py#L145'>superset/views/error_handling.py:145:51:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/views/error_handling.py#L151'>superset/views/error_handling.py:151:52:</a> LOG014 `exc_info=` outside exception handlers
... 4 additional changes omitted for rule LOG014
+ <a href='https://github.com/apache/superset/blob/8984f88a3e1c2843dea00230263ea6323ef83308/superset/views/error_handling.py#L210'>superset/views/error_handling.py:210:9:</a> LOG004 `.exception()` call outside exception handlers
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/7d0952a885cb119ae063516d856a3ce9ec047c57/rotkehlchen/api/server.py#L438'>rotkehlchen/api/server.py:438:13:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/rotki/rotki/blob/7d0952a885cb119ae063516d856a3ce9ec047c57/rotkehlchen/api/server.py#L441'>rotkehlchen/api/server.py:441:13:</a> LOG014 `exc_info=` outside exception handlers
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/f644c23e8403b8de8f71a8232b917598acbd7e26/zerver/decorator.py#L333'>zerver/decorator.py:333:5:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/f644c23e8403b8de8f71a8232b917598acbd7e26/zerver/decorator.py#L342'>zerver/decorator.py:342:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/f644c23e8403b8de8f71a8232b917598acbd7e26/zerver/decorator.py#L344'>zerver/decorator.py:344:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/f644c23e8403b8de8f71a8232b917598acbd7e26/zerver/decorator.py#L346'>zerver/decorator.py:346:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/f644c23e8403b8de8f71a8232b917598acbd7e26/zerver/lib/push_notifications.py#L1334'>zerver/lib/push_notifications.py:1334:21:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/f644c23e8403b8de8f71a8232b917598acbd7e26/zerver/lib/remote_server.py#L291'>zerver/lib/remote_server.py:291:9:</a> LOG004 `.exception()` call outside exception handlers
+ <a href='https://github.com/zulip/zulip/blob/f644c23e8403b8de8f71a8232b917598acbd7e26/zerver/worker/base.py#L265'>zerver/worker/base.py:265:17:</a> LOG004 `.exception()` call outside exception handlers
... 3 additional changes omitted for rule LOG004
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG004 | 22 | 22 | 0 | 0 | 0 |
| LOG014 | 16 | 16 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2025-01-29 00:59_

They could have been defined in the same module/function, but I was worried that there might be too much coupling between them. I'll merge them if that is indeed the better approach.

---

_Comment by @InSyncWithFoo on 2025-01-29 01:21_

I just realized #14682 exists (which, unfortunately, hasn't been active for more than a month). I'm willing to remove my `LOG014` in favor of that and rewrite `LOG004` accordingly if that is desirable.

---

_Comment by @bluetech on 2025-01-29 16:33_

I see that the code does handle it, but I think it would be good to test that the lint *doesn't* trigger for `exc_info=False`, `exc_info=exc`.

---

_Comment by @InSyncWithFoo on 2025-01-29 16:41_

@bluetech Done for the former and already done for the latter.

---

_Label `rule` added by @dylwil3 on 2025-01-30 13:58_

---

_Label `preview` added by @dylwil3 on 2025-01-30 13:59_

---

_@MichaReiser reviewed on 2025-02-03 08:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/mod.rs`:32 on 2025-02-03 08:11_

New rule tests can be added to the regular `rules` test block. It is only necessary to add tests to the `preview_rules` block if the rule changes behavior based on `settings.preview_mode`. 

Adding the tests directly to `rules` eases promoting the rule to stable because it isn't required to rename the snapshot files/move the tests.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/helpers.rs`:9 on 2025-02-03 08:13_

Should we also stop if we see a lambda expression?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exception_call_outside_handlers.rs`:75 on 2025-02-03 08:14_

```suggestion
    let fix = match &*call.func {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exception_call_outside_handlers.rs`:69 on 2025-02-03 08:20_

The logger module is the wild west... Some rules test for `semantic.seen_module(Modules::LOGGING)` while others don't. Some test for `is_logger_candidate` while others dont... 

I'm not a 100% sure but I feel like we should either have one or the other because the rule would still get skipped becaues of the module check even if I add e.g. `mylogging.Logger` to logger objects. Wdyt?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exception_call_outside_handlers.rs`:92 on 2025-02-03 08:21_

```suggestion
        func @ Expr::Name(_) => {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_handlers.rs`:19 on 2025-02-03 08:24_

```suggestion
/// attaches `None` as the exception information, leading to confusing messages:
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_handlers.rs`:46 on 2025-02-03 08:25_

```suggestion
/// The fix is always marked as unsafe, as it changes runtime behavior.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_handlers.rs`:66 on 2025-02-03 08:26_

I think we should remove the seen module check but I'm not a 100% sure, see my comment for the other rules. Let's add a test for a custom logger object to verify whether it is possible to short-circuit here or not.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_handlers.rs`:79 on 2025-02-03 08:26_

```suggestion
        func @ Expr::Name(_) => {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_handlers.rs`:72 on 2025-02-03 08:26_

```suggestion
    match &*call.func {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_handlers.rs`:101 on 2025-02-03 08:28_

Nit: 

```suggestion
    if truthiness.into_bool() != Some(true) {
        return;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_handlers.rs`:110 on 2025-02-03 08:29_

We shouldn't use `unreachable` here because the code here is reachable when generating the fix fails. We should, instead, use the `try_fix` method on `diagnostic`. 

---

_@MichaReiser requested changes on 2025-02-03 08:30_

Thanks. This overall looks good. We should add some tests with custom logger objects (in dedicated files) to see if the `seen_module` short-circuit check is valid.

---

_Comment by @MichaReiser on 2025-02-03 08:35_

The ecosystem check also report a few false-positives e.g: 

https://github.com/apache/airflow/blob/41403c1221025f5ffe737f1a8e2c488212b3c493/providers/edge/src/airflow/providers/edge/worker_api/auth.py#L59

This was discussed in your linked PR, see https://github.com/astral-sh/ruff/pull/14682#discussion_r1863794624 and https://github.com/astral-sh/ruff/pull/14682#issuecomment-2521227958

I'm interested in your opinion on this. 

I'm somewhat leaning towards using noqa comments for such functions because:

The function should document that it should only be called from inside a `except `handler and calling it outside of it may lead to incorrect result. This is similar to documenting `unsafe` in Rust. 


Did you review the other ecosystem checks? Considering that there are more LOG014, maybe consider splitting the PR into two?

---

_Comment by @InSyncWithFoo on 2025-02-03 14:31_

> Did you review the other ecosystem checks?

I'm aware of them. The original rule [also intentionally reports them](https://github.com/adamchainz/flake8-logging/blob/dbe88e76947450a7c4c1ebfb7a7a44875004f410/tests/test_flake8_logging.py#L487), so I think it's fine.

As for splitting, I'm not sure. They are closely related enough.

---

_@InSyncWithFoo reviewed on 2025-02-03 14:31_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_logging/rules/helpers.rs`:9 on 2025-02-03 14:31_

The original rule doesn't check for lambdas. Besides, you can't have a `try`/`except` in a lambda, and violations within functions are reported anyway.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1128 on 2025-02-03 19:07_

Nit: How about `LogExceptionOutsideExceptHandler` and `ExcInfoOutsideExceptHandler`

---

_Comment by @MichaReiser on 2025-02-03 19:12_

Huh, this one is interesting. Would you mind taking a look at why it is flagged

https://github.com/apache/airflow/blob/3004da95e97ba79eba2ab6b743a75e3f3f8dc170/tests/core/test_settings.py#L168

---

_@MichaReiser approved on 2025-02-03 19:15_

---

_Comment by @InSyncWithFoo on 2025-02-03 22:23_

@MichaReiser Fixed. The method name should also have been checked.

---

_Comment by @MichaReiser on 2025-02-04 08:52_

Thanks

---

_Merged by @MichaReiser on 2025-02-04 08:52_

---

_Closed by @MichaReiser on 2025-02-04 08:52_

---

_Branch deleted on 2025-02-04 20:22_

---

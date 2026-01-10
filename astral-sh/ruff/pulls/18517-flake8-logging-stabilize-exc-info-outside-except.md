```yaml
number: 18517
title: "[`flake8-logging`] Stabilize `exc-info-outside-except-handler` (`LOG014`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: brent/stabilize-log014
created_at: 2025-06-06T22:26:16Z
updated_at: 2025-06-10T20:11:56Z
url: https://github.com/astral-sh/ruff/pull/18517
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-logging`] Stabilize `exc-info-outside-except-handler` (`LOG014`)

---

_Pull request opened by @ntBre on 2025-06-06 22:26_

## Summary
- Stabilizes LOG014 (exc-info-outside-except-handler) rule by changing it from Preview to Stable

## Test plan
- ✅ Rule is already tested in main test function, no migration needed
- ✅ `make check` passes
- ✅ `make test` passes

## Rule Documentation
- [Test file](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_logging/mod.rs#L22-L23)
- [Rule documentation](https://docs.astral.sh/ruff/rules/exc-info-outside-except-handler/)

---

_Comment by @github-actions[bot] on 2025-06-06 22:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+16 -1 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/src/airflow/api_fastapi/execution_api/app.py#L187'>airflow-core/src/airflow/api_fastapi/execution_api/app.py:187:55:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/task-sdk/src/airflow/sdk/execution_time/supervisor.py#L1027'>task-sdk/src/airflow/sdk/execution_time/supervisor.py:1027:13:</a> LOG014 `exc_info=` outside exception handlers
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/commands/report/base.py#L106'>superset/commands/report/base.py:106:75:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/db_engine_specs/clickhouse.py#L181'>superset/db_engine_specs/clickhouse.py:181:17:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/db_engine_specs/hive.py#L563'>superset/db_engine_specs/hive.py:563:13:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/sql_lab.py#L138'>superset/sql_lab.py:138:69:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/sql_lab.py#L142'>superset/sql_lab.py:142:75:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/tasks/cache.py#L298'>superset/tasks/cache.py:298:31:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/utils/core.py#L574'>superset/utils/core.py:574:43:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/views/error_handling.py#L140'>superset/views/error_handling.py:140:50:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/views/error_handling.py#L145'>superset/views/error_handling.py:145:51:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/views/error_handling.py#L151'>superset/views/error_handling.py:151:52:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/views/error_handling.py#L160'>superset/views/error_handling.py:160:41:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/views/error_handling.py#L187'>superset/views/error_handling.py:187:44:</a> LOG014 `exc_info=` outside exception handlers
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/views/error_handling.py#L209'>superset/views/error_handling.py:209:37:</a> LOG014 `exc_info=` outside exception handlers
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/6fc06cfcc5434e984aa676b51896ab2c0fe433c4/rotkehlchen/api/server.py#L439'>rotkehlchen/api/server.py:439:29:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `LOG014`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/push_notifications.py#L1334'>zerver/lib/push_notifications.py:1334:21:</a> LOG014 `exc_info=` outside exception handlers
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG014 | 16 | 16 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-06-06 22:32_

---

_Comment by @ntBre on 2025-06-06 22:42_

I'm kind of on the fence about the ecosystem results. Some of them seem like true positives, like the zulip case, but many of the others are in functions annotated with some kind of `@app.exception_handler` decorator. I'm not sure there's much we can do about this, though, as the type of `app` and the name of the decorator varies across packages. It might be rare enough that `noqa` is the answer, but I'm open to other thoughts here. In general it seems difficult to detect whether or not we're in an "exception handler," as the RUF100 case in rotki also demonstrates.

---

_Comment by @ntBre on 2025-06-09 22:14_

I think stabilizing this is in line with other rules we've stabilized, and 16 ecosystem hits is not much overall. LOG004 and LOG014 are the only 2/7 LOG rules in preview still. I gave LOG004 a `Stabilize*` on Notion because of https://github.com/astral-sh/ruff/issues/18044, which I think we could help with a documentation fix and finish stabilizing the set.

---

_Review requested from @dylwil3 by @ntBre on 2025-06-09 22:15_

---

_@dylwil3 approved on 2025-06-09 22:32_

I think it's okay to stabilize this for the reasons you give. Codebases with lots of custom exception handlers may choose not to enable the rule at all if it's noisy - it seems relatively niche to me.

---

_Renamed from "Stabilize LOG014 (exc-info-outside-except-handler)" to "[`flake8-loggin`] Stabilize `exc-info-outside-except-handler` (`LOG014`)" by @ntBre on 2025-06-10 01:55_

---

_Renamed from "[`flake8-loggin`] Stabilize `exc-info-outside-except-handler` (`LOG014`)" to "[`flake8-logging`] Stabilize `exc-info-outside-except-handler` (`LOG014`)" by @ntBre on 2025-06-10 01:55_

---

_Merged by @ntBre on 2025-06-10 01:56_

---

_Closed by @ntBre on 2025-06-10 01:56_

---

_Branch deleted on 2025-06-10 01:56_

---

_Added to milestone `v0.12` by @ntBre on 2025-06-10 20:11_

---

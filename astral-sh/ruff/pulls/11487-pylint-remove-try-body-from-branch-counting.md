```yaml
number: 11487
title: "[`pylint`] Remove `try` body from branch counting"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/branch
created_at: 2024-05-22T03:08:50Z
updated_at: 2024-05-22T03:38:52Z
url: https://github.com/astral-sh/ruff/pull/11487
synced_at: 2026-01-12T15:55:38Z
```

# [`pylint`] Remove `try` body from branch counting

---

_@charliermarsh_

## Summary

Matching Pylint, we now omit the `try` body itself from branch counting. Each `except` counts as a branch, as does the `else` and the `finally`.

Closes https://github.com/astral-sh/ruff/issues/11205.


---

_Label `bug` added by @charliermarsh on 2024-05-22 03:08_

---

_Comment by @github-actions[bot] on 2024-05-22 03:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+120 -144 violations, +0 -0 fixes in 4 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/534b8e960050488db4c9e2977bc4142388afc153/src/plasmapy/particles/_parsing.py#L152'>src/plasmapy/particles/_parsing.py:152:32:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR0912`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/534b8e960050488db4c9e2977bc4142388afc153/src/plasmapy/utils/decorators/validators.py#L289'>src/plasmapy/utils/decorators/validators.py:289:30:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR0912`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+73 -87 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api/common/airflow_health.py#L29'>airflow/api/common/airflow_health.py:29:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api_connexion/endpoints/log_endpoint.py#L45'>airflow/api_connexion/endpoints/log_endpoint.py:45:5:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api_connexion/endpoints/task_instance_endpoint.py#L158'>airflow/api_connexion/endpoints/task_instance_endpoint.py:158:5:</a> PLR0912 Too many branches (15 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api_connexion/endpoints/task_instance_endpoint.py#L158'>airflow/api_connexion/endpoints/task_instance_endpoint.py:158:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (18 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (19 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/db_command.py#L98'>airflow/cli/commands/db_command.py:98:5:</a> PLR0912 Too many branches (15 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/db_command.py#L98'>airflow/cli/commands/db_command.py:98:5:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L403'>airflow/cli/commands/task_command.py:403:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L403'>airflow/cli/commands/task_command.py:403:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L630'>airflow/cli/commands/task_command.py:630:5:</a> PLR0912 Too many branches (17 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L630'>airflow/cli/commands/task_command.py:630:5:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/configuration.py#L1343'>airflow/configuration.py:1343:9:</a> PLR0912 Too many branches (13 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/configuration.py#L1343'>airflow/configuration.py:1343:9:</a> PLR0912 Too many branches (15 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/manager.py#L1122'>airflow/dag_processing/manager.py:1122:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/manager.py#L543'>airflow/dag_processing/manager.py:543:9:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/manager.py#L543'>airflow/dag_processing/manager.py:543:9:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/processor.py#L421'>airflow/dag_processing/processor.py:421:9:</a> PLR0912 Too many branches (27 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/processor.py#L421'>airflow/dag_processing/processor.py:421:9:</a> PLR0912 Too many branches (30 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/datasets/__init__.py#L52'>airflow/datasets/__init__.py:52:5:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L465'>airflow/jobs/backfill_job_runner.py:465:9:</a> PLR0912 Too many branches (23 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L465'>airflow/jobs/backfill_job_runner.py:465:9:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L497'>airflow/jobs/backfill_job_runner.py:497:17:</a> PLR0912 Too many branches (25 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L497'>airflow/jobs/backfill_job_runner.py:497:17:</a> PLR0912 Too many branches (26 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L899'>airflow/jobs/backfill_job_runner.py:899:9:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L899'>airflow/jobs/backfill_job_runner.py:899:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L298'>airflow/jobs/scheduler_job_runner.py:298:9:</a> PLR0912 Too many branches (29 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L298'>airflow/jobs/scheduler_job_runner.py:298:9:</a> PLR0912 Too many branches (30 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L804'>airflow/jobs/scheduler_job_runner.py:804:9:</a> PLR0912 Too many branches (13 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L804'>airflow/jobs/scheduler_job_runner.py:804:9:</a> PLR0912 Too many branches (16 > 12)
... 130 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L615'>src/bokeh/core/has_props.py:615:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L342'>src/bokeh/core/property/bases.py:342:9:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L342'>src/bokeh/core/property/bases.py:342:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L110'>src/bokeh/core/query.py:110:5:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L110'>src/bokeh/core/query.py:110:5:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L54'>src/bokeh/plotting/_graph.py:54:5:</a> PLR0912 Too many branches (17 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L54'>src/bokeh/plotting/_graph.py:54:5:</a> PLR0912 Too many branches (19 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/server.py#L351'>src/bokeh/server/server.py:351:9:</a> PLR0912 Too many branches (13 > 12)
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+40 -50 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/stripe.py#L5376'>corporate/lib/stripe.py:5376:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/stripe.py#L5376'>corporate/lib/stripe.py:5376:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/support.py#L227'>corporate/lib/support.py:227:5:</a> PLR0912 Too many branches (13 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/support.py#L227'>corporate/lib/support.py:227:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/installation_activity.py#L92'>corporate/views/installation_activity.py:92:5:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:5:</a> PLR0912 Too many branches (18 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:5:</a> PLR0912 Too many branches (21 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L335'>corporate/views/support.py:335:5:</a> PLR0912 Too many branches (31 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L335'>corporate/views/support.py:335:5:</a> PLR0912 Too many branches (34 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L629'>corporate/views/support.py:629:5:</a> PLR0912 Too many branches (32 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L629'>corporate/views/support.py:629:5:</a> PLR0912 Too many branches (37 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/webhook.py#L22'>corporate/views/webhook.py:22:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision.py#L342'>tools/lib/provision.py:342:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision.py#L342'>tools/lib/provision.py:342:5:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision_inner.py#L216'>tools/lib/provision_inner.py:216:5:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision_inner.py#L216'>tools/lib/provision_inner.py:216:5:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/template_parser.py#L54'>tools/lib/template_parser.py:54:5:</a> PLR0912 Too many branches (36 > 12)
... 73 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0912 | 262 | 118 | 144 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+120 -144 violations, +0 -0 fixes in 4 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/534b8e960050488db4c9e2977bc4142388afc153/src/plasmapy/particles/_parsing.py#L152'>src/plasmapy/particles/_parsing.py:152:32:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR0912`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/534b8e960050488db4c9e2977bc4142388afc153/src/plasmapy/utils/decorators/validators.py#L289'>src/plasmapy/utils/decorators/validators.py:289:30:</a> RUF100 [*] Unused `noqa` directive (unused: `PLR0912`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+73 -87 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api/common/airflow_health.py#L29'>airflow/api/common/airflow_health.py:29:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api_connexion/endpoints/log_endpoint.py#L45'>airflow/api_connexion/endpoints/log_endpoint.py:45:5:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api_connexion/endpoints/task_instance_endpoint.py#L158'>airflow/api_connexion/endpoints/task_instance_endpoint.py:158:5:</a> PLR0912 Too many branches (15 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/api_connexion/endpoints/task_instance_endpoint.py#L158'>airflow/api_connexion/endpoints/task_instance_endpoint.py:158:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (18 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/connection_command.py#L209'>airflow/cli/commands/connection_command.py:209:5:</a> PLR0912 Too many branches (19 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/db_command.py#L98'>airflow/cli/commands/db_command.py:98:5:</a> PLR0912 Too many branches (15 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/db_command.py#L98'>airflow/cli/commands/db_command.py:98:5:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L403'>airflow/cli/commands/task_command.py:403:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L403'>airflow/cli/commands/task_command.py:403:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L630'>airflow/cli/commands/task_command.py:630:5:</a> PLR0912 Too many branches (17 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/cli/commands/task_command.py#L630'>airflow/cli/commands/task_command.py:630:5:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/configuration.py#L1343'>airflow/configuration.py:1343:9:</a> PLR0912 Too many branches (13 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/configuration.py#L1343'>airflow/configuration.py:1343:9:</a> PLR0912 Too many branches (15 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/manager.py#L1122'>airflow/dag_processing/manager.py:1122:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/manager.py#L543'>airflow/dag_processing/manager.py:543:9:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/manager.py#L543'>airflow/dag_processing/manager.py:543:9:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/processor.py#L421'>airflow/dag_processing/processor.py:421:9:</a> PLR0912 Too many branches (27 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/dag_processing/processor.py#L421'>airflow/dag_processing/processor.py:421:9:</a> PLR0912 Too many branches (30 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/datasets/__init__.py#L52'>airflow/datasets/__init__.py:52:5:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L465'>airflow/jobs/backfill_job_runner.py:465:9:</a> PLR0912 Too many branches (23 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L465'>airflow/jobs/backfill_job_runner.py:465:9:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L497'>airflow/jobs/backfill_job_runner.py:497:17:</a> PLR0912 Too many branches (25 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L497'>airflow/jobs/backfill_job_runner.py:497:17:</a> PLR0912 Too many branches (26 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L899'>airflow/jobs/backfill_job_runner.py:899:9:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/backfill_job_runner.py#L899'>airflow/jobs/backfill_job_runner.py:899:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L298'>airflow/jobs/scheduler_job_runner.py:298:9:</a> PLR0912 Too many branches (29 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L298'>airflow/jobs/scheduler_job_runner.py:298:9:</a> PLR0912 Too many branches (30 > 12)
+ <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L804'>airflow/jobs/scheduler_job_runner.py:804:9:</a> PLR0912 Too many branches (13 > 12)
- <a href='https://github.com/apache/airflow/blob/791f3cfc5cc7f2798ee0fcd18ff72906c48ebd01/airflow/jobs/scheduler_job_runner.py#L804'>airflow/jobs/scheduler_job_runner.py:804:9:</a> PLR0912 Too many branches (16 > 12)
... 130 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L615'>src/bokeh/core/has_props.py:615:9:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L342'>src/bokeh/core/property/bases.py:342:9:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L342'>src/bokeh/core/property/bases.py:342:9:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L110'>src/bokeh/core/query.py:110:5:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L110'>src/bokeh/core/query.py:110:5:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (16 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0912 Too many branches (17 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L54'>src/bokeh/plotting/_graph.py:54:5:</a> PLR0912 Too many branches (17 > 12)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L54'>src/bokeh/plotting/_graph.py:54:5:</a> PLR0912 Too many branches (19 > 12)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/server.py#L351'>src/bokeh/server/server.py:351:9:</a> PLR0912 Too many branches (13 > 12)
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+40 -50 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/stripe.py#L5376'>corporate/lib/stripe.py:5376:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/stripe.py#L5376'>corporate/lib/stripe.py:5376:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/support.py#L227'>corporate/lib/support.py:227:5:</a> PLR0912 Too many branches (13 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/lib/support.py#L227'>corporate/lib/support.py:227:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/installation_activity.py#L92'>corporate/views/installation_activity.py:92:5:</a> PLR0912 Too many branches (13 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:5:</a> PLR0912 Too many branches (18 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:5:</a> PLR0912 Too many branches (21 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L335'>corporate/views/support.py:335:5:</a> PLR0912 Too many branches (31 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L335'>corporate/views/support.py:335:5:</a> PLR0912 Too many branches (34 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L629'>corporate/views/support.py:629:5:</a> PLR0912 Too many branches (32 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/support.py#L629'>corporate/views/support.py:629:5:</a> PLR0912 Too many branches (37 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/corporate/views/webhook.py#L22'>corporate/views/webhook.py:22:5:</a> PLR0912 Too many branches (16 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision.py#L342'>tools/lib/provision.py:342:5:</a> PLR0912 Too many branches (14 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision.py#L342'>tools/lib/provision.py:342:5:</a> PLR0912 Too many branches (18 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision_inner.py#L216'>tools/lib/provision_inner.py:216:5:</a> PLR0912 Too many branches (24 > 12)
- <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/provision_inner.py#L216'>tools/lib/provision_inner.py:216:5:</a> PLR0912 Too many branches (25 > 12)
+ <a href='https://github.com/zulip/zulip/blob/0d3e223493aed35ff93afbbb85e9457849131ead/tools/lib/template_parser.py#L54'>tools/lib/template_parser.py:54:5:</a> PLR0912 Too many branches (36 > 12)
... 73 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0912 | 262 | 118 | 144 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-05-22 03:38_

---

_Closed by @charliermarsh on 2024-05-22 03:38_

---

_Branch deleted on 2024-05-22 03:38_

---

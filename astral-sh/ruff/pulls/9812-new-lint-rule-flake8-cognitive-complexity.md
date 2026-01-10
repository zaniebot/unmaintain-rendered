```yaml
number: 9812
title: "New lint rule: flake8_cognitive_complexity function_is_too_cognitive_complex"
type: pull_request
state: closed
author: Anders-Steen
labels:
  - rule
  - preview
assignees: []
base: main
head: cognitive_complexity
created_at: 2024-02-04T14:48:57Z
updated_at: 2024-03-28T16:17:30Z
url: https://github.com/astral-sh/ruff/pull/9812
synced_at: 2026-01-10T22:47:01Z
```

# New lint rule: flake8_cognitive_complexity function_is_too_cognitive_complex

---

_Pull request opened by @Anders-Steen on 2024-02-04 14:48_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Cognitive complexity is a more modern way of calculating the complexity of a code. We do not care how hard it is to test, but rather how hard it is to read and understand.

Created a new lint rule. Took a lot of inspiration from mccabe rule (aka cyclomatic complexity).

fixes #2418

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

This was tested with a python file [crates/ruff_linter/resources/test/fixtures/flake8_cognitive_complexity/CCR001.py](crates/ruff_linter/resources/test/fixtures/flake8_cognitive_complexity/CCR001.py) as well as unit-tests in [crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs](crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs).

## Please help
I tried to follow [contributing guide](https://docs.astral.sh/ruff/contributing/) but ran into a problem on *creating a new release, step 2*. `"Could not resolve to a Repository with the name 'AndersSteenNilsen/ruff.git'.`. Maybe I shouldn't have forked the repo?

## Further work
This linting rule is not perfectly complete, it does not test for nested functions and recursion. But I believe those are edge-cases, also nested functions should not count for decorators.
Did not test and implement bitwise operators, but focused on boolean operators.

---

_Comment by @github-actions[bot] on 2024-02-04 15:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1612 -0 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+934 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/auth/backend/kerberos_auth.py#L138'>airflow/api/auth/backend/kerberos_auth.py:138:5:</a> CCR001 `requires_authentication` is too cognitive complex (9 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/auth/backend/kerberos_auth.py#L150'>airflow/api/auth/backend/kerberos_auth.py:150:9:</a> CCR001 `decorated` is too cognitive complex (8 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/delete_dag.py#L41'>airflow/api/common/delete_dag.py:41:5:</a> CCR001 `delete_dag` is too cognitive complex (11 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/mark_tasks.py#L209'>airflow/api/common/mark_tasks.py:209:5:</a> CCR001 `_iter_subdag_run_ids` is too cognitive complex (15 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/mark_tasks.py#L279'>airflow/api/common/mark_tasks.py:279:5:</a> CCR001 `find_task_relatives` is too cognitive complex (14 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/mark_tasks.py#L378'>airflow/api/common/mark_tasks.py:378:5:</a> CCR001 `set_dag_run_state_to_success` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/mark_tasks.py#L432'>airflow/api/common/mark_tasks.py:432:5:</a> CCR001 `set_dag_run_state_to_failed` is too cognitive complex (15 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/mark_tasks.py#L522'>airflow/api/common/mark_tasks.py:522:5:</a> CCR001 `__set_dag_run_state_to_running_or_queued` is too cognitive complex (9 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/mark_tasks.py#L83'>airflow/api/common/mark_tasks.py:83:5:</a> CCR001 `set_state` is too cognitive complex (19 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api/common/trigger_dag.py#L34'>airflow/api/common/trigger_dag.py:34:5:</a> CCR001 `_trigger_dag` is too cognitive complex (12 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/config_endpoint.py#L106'>airflow/api_connexion/endpoints/config_endpoint.py:106:5:</a> CCR001 `get_value` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/config_endpoint.py#L69'>airflow/api_connexion/endpoints/config_endpoint.py:69:5:</a> CCR001 `get_config` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/dag_run_endpoint.py#L140'>airflow/api_connexion/endpoints/dag_run_endpoint.py:140:5:</a> CCR001 `_fetch_dag_runs` is too cognitive complex (8 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/log_endpoint.py#L45'>airflow/api_connexion/endpoints/log_endpoint.py:45:5:</a> CCR001 `get_log` is too cognitive complex (11 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/pool_endpoint.py#L86'>airflow/api_connexion/endpoints/pool_endpoint.py:86:5:</a> CCR001 `patch_pool` is too cognitive complex (12 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/task_instance_endpoint.py#L157'>airflow/api_connexion/endpoints/task_instance_endpoint.py:157:5:</a> CCR001 `get_mapped_task_instances` is too cognitive complex (12 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/task_instance_endpoint.py#L444'>airflow/api_connexion/endpoints/task_instance_endpoint.py:444:5:</a> CCR001 `post_clear_task_instances` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/endpoints/xcom_endpoint.py#L83'>airflow/api_connexion/endpoints/xcom_endpoint.py:83:5:</a> CCR001 `get_xcom_entry` is too cognitive complex (8 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/api_connexion/schemas/task_instance_schema.py#L140'>airflow/api_connexion/schemas/task_instance_schema.py:140:9:</a> CCR001 `validate_form` is too cognitive complex (12 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/auth/managers/base_auth_manager.py#L377'>airflow/auth/managers/base_auth_manager.py:377:9:</a> CCR001 `get_permitted_menu_items` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/celery_command.py#L119'>airflow/cli/commands/celery_command.py:119:5:</a> CCR001 `worker` is too cognitive complex (11 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/cheat_sheet_command.py#L35'>airflow/cli/commands/cheat_sheet_command.py:35:5:</a> CCR001 `display_commands_index` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/cheat_sheet_command.py#L38'>airflow/cli/commands/cheat_sheet_command.py:38:9:</a> CCR001 `display_recursive` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/connection_command.py#L114'>airflow/cli/commands/connection_command.py:114:5:</a> CCR001 `_format_connections` is too cognitive complex (9 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/connection_command.py#L160'>airflow/cli/commands/connection_command.py:160:5:</a> CCR001 `connections_export` is too cognitive complex (15 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/connection_command.py#L206'>airflow/cli/commands/connection_command.py:206:5:</a> CCR001 `connections_add` is too cognitive complex (33 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/connection_command.py#L322'>airflow/cli/commands/connection_command.py:322:5:</a> CCR001 `_import_helper` is too cognitive complex (10 > 7)
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/cli/commands/dag_command.py#L558'>airflow/cli/commands/dag_command.py:558:5:</a> CCR001 `dag_test` is too cognitive complex (11 > 7)
... 906 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+128 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L26'>examples/server/app/clustering/main.py:26:5:</a> CCR001 `clustering` is too cognitive complex (10 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/crossfilter/main.py#L35'>examples/server/app/crossfilter/main.py:35:5:</a> CCR001 `create_figure` is too cognitive complex (11 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/image_blur.py#L33'>examples/server/app/image_blur.py:33:5:</a> CCR001 `blur` is too cognitive complex (10 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/pipeline.py#L39'>release/pipeline.py:39:9:</a> CCR001 `execute` is too cognitive complex (11 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L122'>scripts/milestone.py:122:5:</a> CCR001 `check_issue` is too cognitive complex (8 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L158'>scripts/milestone.py:158:5:</a> CCR001 `check_pr` is too cognitive complex (11 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L284'>scripts/milestone.py:284:5:</a> CCR001 `main` is too cognitive complex (14 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L80'>setup.py:80:5:</a> CCR001 `install_js` is too cognitive complex (12 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L110'>src/bokeh/application/handlers/directory.py:110:9:</a> CCR001 `__init__` is too cognitive complex (18 > 7)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/server_lifecycle.py#L55'>src/bokeh/application/handlers/server_lifecycle.py:55:9:</a> CCR001 `__init__` is too cognitive complex (9 > 7)
... 118 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+550 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/lib/counts.py#L119'>analytics/lib/counts.py:119:5:</a> CCR001 `process_count_stat` is too cognitive complex (12 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/lib/counts.py#L304'>analytics/lib/counts.py:304:5:</a> CCR001 `do_increment_logging_stat` is too cognitive complex (11 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/lib/fixtures.py#L8'>analytics/lib/fixtures.py:8:5:</a> CCR001 `generate_time_series_data` is too cognitive complex (15 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/management/commands/check_analytics_state.py#L41'>analytics/management/commands/check_analytics_state.py:41:9:</a> CCR001 `get_fill_state` is too cognitive complex (15 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/management/commands/update_analytics_counts.py#L59'>analytics/management/commands/update_analytics_counts.py:59:9:</a> CCR001 `run_update_analytics_counts` is too cognitive complex (11 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/migrations/0015_clear_duplicate_counts.py#L7'>analytics/migrations/0015_clear_duplicate_counts.py:7:5:</a> CCR001 `clear_duplicate_counts` is too cognitive complex (11 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/tests/test_counts.py#L220'>analytics/tests/test_counts.py:220:9:</a> CCR001 `assertTableState` is too cognitive complex (13 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/views/stats.py#L250'>analytics/views/stats.py:250:5:</a> CCR001 `get_chart_data` is too cognitive complex (53 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/analytics/views/stats.py#L519'>analytics/views/stats.py:519:5:</a> CCR001 `client_label_map` is too cognitive complex (10 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/activity.py#L311'>corporate/lib/activity.py:311:5:</a> CCR001 `get_plan_data_by_remote_realm` is too cognitive complex (8 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/stripe.py#L1407'>corporate/lib/stripe.py:1407:9:</a> CCR001 `process_initial_upgrade` is too cognitive complex (40 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/stripe.py#L1626'>corporate/lib/stripe.py:1626:9:</a> CCR001 `do_upgrade` is too cognitive complex (10 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/stripe.py#L1803'>corporate/lib/stripe.py:1803:9:</a> CCR001 `make_end_of_cycle_updates_if_needed` is too cognitive complex (25 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/stripe.py#L1995'>corporate/lib/stripe.py:1995:9:</a> CCR001 `get_billing_context_from_plan` is too cognitive complex (18 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/stripe.py#L2181'>corporate/lib/stripe.py:2181:9:</a> CCR001 `get_initial_upgrade_context` is too cognitive complex (19 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/stripe.py#L2374'>corporate/lib/stripe.py:2374:9:</a> CCR001 `do_update_plan` is too cognitive complex (35 > 7)
+ <a href='https://github.com/zulip/zulip/blob/da7cb0af1c42eadb195849a8d8b3b8a323048239/corporate/lib/stripe.py#L2553'>corporate/lib/stripe.py:2553:9:</a> CCR001 `invoice_plan` is too cognitive complex (23 > 7)
... 533 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| CCR001 | 1612 | 1612 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:21 on 2024-02-26 11:41_

This links to a local file. Is this available online too and we could link to it instead? This link seems to bypass sonar's registration form https://www.sonarsource.com/docs/CognitiveComplexity.pdf

---

_@MichaReiser reviewed on 2024-02-26 11:41_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:13 on 2024-02-26 12:26_

What's the licensing of Sonar's rule? The whitespaper has a footer mentioning "Copyright SonarSource S . A., 2023, Switzerland. All content is copyright protected". I don't know to what extend this applies to algorithms but something we should consider.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:85 on 2024-02-26 12:32_

The code here needs to visit all expression or you miss out in nested expressions: `call(a and b)` or does the algorithm intentionally ignore this?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:96 on 2024-02-26 12:33_

You may want to take a look at implementing the `Visitor` or `PreorderVisitor` traits. Visitors allow you to traverse an entire subtree and ensure that e.g. nested expressions get visited. 

The code of the upstream plugin https://github.com/Melevir/cognitive_complexity/blob/master/cognitive_complexity/utils/ast.py

It traverses all nodes recursively, except boolean operators it seems. It's also worth noting that the upstream plugin doesn't precisely implement the algorithm outlined in the whitepaper.


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:145 on 2024-02-26 12:34_

Nit: You can pass the entire function instead of individual parts
```suggestion
    definition: &FunctionDef,
    max_cognitive_complexity: usize,
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1104 on 2024-02-26 12:35_

How did you decide on the default? Is it the same as the upstream rule uses?

---

_@MichaReiser reviewed on 2024-02-26 12:36_

Thanks for working on this. 

We have to figure out what the copyright of the rule is before implementing and reviewing fully. 

---

_Comment by @Anders-Steen on 2024-02-26 14:38_

Thank you for reviewing @MichaReiser, I'll see if I have time this evening to go through the feedback this evening or later this week.

---

_@Anders-Steen reviewed on 2024-02-26 14:49_

---

_Review comment by @Anders-Steen on `crates/ruff_workspace/src/options.rs`:1104 on 2024-02-26 14:49_

Correct! I copied the default max [ref](https://github.com/Melevir/flake8-cognitive-complexity/blob/d88502e1abdcc4540c27dac430c616ae6ba1eb9b/flake8_cognitive_complexity/checker.py#L10).

Though the author thinks it should be 15 but also states that there perhaps shouldn't be a default... [ref](https://stackoverflow.com/a/45084107)

---

_@Anders-Steen reviewed on 2024-02-26 14:52_

---

_Review comment by @Anders-Steen on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:21 on 2024-02-26 14:52_

Oops, a blunder, yes, it is the same `.pdf` that I used.

---

_Review comment by @Anders-Steen on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:13 on 2024-02-26 18:16_

https://github.com/SonarSource/sonar-python?tab=LGPL-3.0-1-ov-file
Looks like it is *LGPL*, so I guess??? Not completely sure if it means we can use it within this *MIT* licenced project .

---

_@Anders-Steen reviewed on 2024-02-26 18:16_

---

_Comment by @codspeed-hq[bot] on 2024-02-26 18:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AndersSteenNilsen:cognitive_complexity)

### Merging #9812 will **improve performances by 44.6%**

<sub>Comparing <code>AndersSteenNilsen:cognitive_complexity</code> (b51d4be) with <code>main</code> (c53aae0)</sub>



### Summary

`⚡ 21` improvements
`✅ 9` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `AndersSteenNilsen:cognitive_complexity` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `formatter[numpy/ctypeslib.py]` | 10.2 ms | 9.7 ms | +4.97% |
| ⚡ | `formatter[numpy/globals.py]` | 1,152.8 µs | 990.1 µs | +16.43% |
| ⚡ | `formatter[unicode/pypinyin.py]` | 3.4 ms | 3.3 ms | +4.61% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 1.8 ms | 1.6 ms | +15.26% |
| ⚡ | `lexer[numpy/globals.py]` | 223.5 µs | 154.5 µs | +44.6% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 571.5 µs | 526.4 µs | +8.57% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 94.1 ms | 81.4 ms | +15.56% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 22 ms | 18.7 ms | +17.78% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 2.8 ms | 2.6 ms | +8.14% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 44.3 ms | 39.1 ms | +13.49% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 11.8 ms | 9.8 ms | +20.28% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 106.9 ms | 96 ms | +11.39% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 24.6 ms | 22.1 ms | +11.23% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 3.1 ms | 3 ms | +4.6% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 51.9 ms | 47.1 ms | +10.11% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 12.8 ms | 11.1 ms | +15.59% |
| ⚡ | `parser[large/dataset.py]` | 69.1 ms | 63.6 ms | +8.59% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 11.8 ms | 10.8 ms | +9.73% |
| ⚡ | `parser[numpy/globals.py]` | 1,117.4 µs | 963.9 µs | +15.92% |
| ⚡ | `parser[pydantic/types.py]` | 26.4 ms | 24.4 ms | +7.9% |
| ... | ... | ... | ... | ... |

<br/>

> :information_source: _Only the first 20 benchmarks are displayed. [Go to the app to view all benchmarks](https://codspeed.io/astral-sh/ruff/branches/AndersSteenNilsen:cognitive_complexity)._


---

_Review comment by @Anders-Steen on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:85 on 2024-02-26 20:57_

I am not quite sure what you mean, but I think nested expressions should be covered from the outer check `get_cognitive_complexity_number`. Could you perhaps give an example which is not in the tests in the bottom of the file?

---

_@Anders-Steen reviewed on 2024-02-26 20:57_

---

_Label `rule` added by @MichaReiser on 2024-03-06 15:39_

---

_Label `preview` added by @MichaReiser on 2024-03-06 15:39_

---

_@MichaReiser reviewed on 2024-03-06 15:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_cognitive_complexity/rules/function_is_too_cognitive_complex.rs`:13 on 2024-03-06 15:39_

Nice. Yeah that looks okay. Also, code climate implements the same rule. So I think we should be good https://docs.codeclimate.com/docs/cognitive-complexity 

---

_Comment by @MichaReiser on 2024-03-28 16:17_

Thank you @AndersSteenNilsen for contributing this new cognitive complexity rule. I like how Sonar quantifies complexity, and I think I prefer it over MC Cabe. 

I'm sorry but we can't merge the rule as it is today. Our concern is that this new rule overlaps with the existing Mc Cabe complexity rule. A possible solution is to merge the two rules with a configuration option that controls which complexity metric to use. One open design question is how to incorporate other rules that measure complexity like `too-many-local-variables`, `too-many-lines`, `too-many-instance-attributes`, etc. Should the same rule cover these? Should we have two rules, one for control flow complexity and one for structural/shape complexity? etc... 

I'm sorry that we failed to make it clear on the issue that implementing this rule requires more design work. I hope we can build on top of your PR once we figure out these design decisions.



---

_Closed by @MichaReiser on 2024-03-28 16:17_

---

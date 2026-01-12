```yaml
number: 13196
title: "[pylint] Implement `consider-using-assignment-expr` (`R6103`)"
type: pull_request
state: closed
author: vincevannoort
labels:
  - rule
  - preview
assignees: []
base: main
head: consider-using-assignment-expr
created_at: 2024-09-01T17:31:09Z
updated_at: 2025-02-20T08:53:33Z
url: https://github.com/astral-sh/ruff/pull/13196
synced_at: 2026-01-12T15:55:43Z
```

# [pylint] Implement `consider-using-assignment-expr` (`R6103`)

---

_@vincevannoort_

## Summary

This pull request implements the `R6103` pylint rule: [pylint documentation](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/consider-using-assignment-expr.html).

It checks assignments which are directly followed by if statements using that expression:

```python
test1 = "example"
if test1:
    print(test1)
```

And suggest to use the `:=` operator to 

```python
if test1 := "example":
    print(test1)
```

## Test Plan

I have added test cases, and checked some of the `ruff ecosystem` results.


---

_Comment by @codspeed-hq[bot] on 2024-09-01 17:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/vincevannoort%3Aconsider-using-assignment-expr)

### Merging #13196 will **not alter performance**

<sub>Comparing <code>vincevannoort:consider-using-assignment-expr</code> (59a04d8) with <code>main</code> (f8093b6)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Renamed from "[WIP] Pylint `R6103`" to "[pylint][WIP] Implement `R6103`" by @vincevannoort on 2024-09-01 17:49_

---

_Renamed from "[pylint][WIP] Implement `R6103`" to "[WIP] [pylint] Implement `R6103`" by @vincevannoort on 2024-09-01 17:49_

---

_Comment by @github-actions[bot] on 2024-09-01 20:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4612 -0 violations, +0 -0 fixes in 19 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/argx.py#L158'>aiven/client/argx.py:158:16:</a> PLR6103 Use walrus operator `(arg_list := getattr(func, ARG_LIST_PROP, None))`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/argx.py#L251'>aiven/client/argx.py:251:16:</a> PLR6103 Use walrus operator `(cat := tuple(cats[:level + 1]))`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L1453'>aiven/client/cli.py:1453:12:</a> PLR6103 Use walrus operator `(route := self.args.route)`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L1470'>aiven/client/cli.py:1470:12:</a> PLR6103 Use walrus operator `(privatelink_connection_id := self.args.privatelink_connection_id)`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L1828'>aiven/client/cli.py:1828:16:</a> PLR6103 Use walrus operator `(value := arg_vars[key])`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L1849'>aiven/client/cli.py:1849:12:</a> PLR6103 Use walrus operator `(access_control := self._parse_access_control())`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L280'>aiven/client/cli.py:280:12:</a> PLR6103 Use walrus operator `(password := os.environ.get(var))`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L329'>aiven/client/cli.py:329:20:</a> PLR6103 Use walrus operator `(arg_list := getattr(prop, argx.ARG_LIST_PROP, None))`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L3581'>aiven/client/cli.py:3581:20:</a> PLR6103 Use walrus operator `(user_input := input("Re-enter service name {!r} for immediate termination: ".format(name)))`.
+ <a href='https://github.com/aiven/aiven-client/blob/a1c1eebed1a26079076e9593709118e9d9931f33/aiven/client/cli.py#L3733'>aiven/client/cli.py:3733:20:</a> PLR6103 Use walrus operator `(user_peer_network_cidrs := peering_connection.get("user_peer_network_cidrs", []))`.
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+38 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ docs/notebooks/langmuir_samples/_generate_noisy.ipynb:cell 6:2:4: PLR6103 Use walrus operator `(save := False)`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/noxfile.py#L482'>noxfile.py:482:18:</a> PLR6103 Use walrus operator `(extraneous_files := source_directory.glob("changelog/*[0-9]*.*.rst?*"))`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/analysis/nullpoint.py#L1453'>src/plasmapy/analysis/nullpoint.py:1453:24:</a> PLR6103 Use walrus operator `(loc := _locate_null_point(vspace, [i, j, k], maxiter, err))`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/analysis/nullpoint.py#L1456'>src/plasmapy/analysis/nullpoint.py:1456:28:</a> PLR6103 Use walrus operator `(p := NullPoint(loc, null_type))`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/analysis/nullpoint.py#L706'>src/plasmapy/analysis/nullpoint.py:706:40:</a> PLR6103 Use walrus operator `(z_close := np.isclose(root[2], r[2], atol=_EQUALITY_ATOL))`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/analysis/swept_langmuir/floating_potential.py#L280'>src/plasmapy/analysis/swept_langmuir/floating_potential.py:280:12:</a> PLR6103 Use walrus operator `(isl_window := np.abs(np.r_[rtn_extras["islands"][-1]][-1] - np.r_[rtn_extras["islands"][0]][0]) + 1)`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/analysis/swept_langmuir/floating_potential.py#L299'>src/plasmapy/analysis/swept_langmuir/floating_potential.py:299:12:</a> PLR6103 Use walrus operator `(iadd := istop - istart + 1 - min_points)`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L1240'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:1240:8:</a> PLR6103 Use walrus operator `(percentage := np.sum(intensity) / results_dict["num_particles"])`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L754'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:754:12:</a> PLR6103 Use walrus operator `(n_wrong_way := np.sum(np.where(self.theta > np.pi / 2, 1, 0)))`.
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b92054ef266973c3c791999c69d4365de2cd4b2/src/plasmapy/diagnostics/thomson.py#L889'>src/plasmapy/diagnostics/thomson.py:889:16:</a> PLR6103 Use walrus operator `(key := f"{p}_{num!s}")`.
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1123 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/client/__init__.py#L32'>airflow/api/client/__init__.py:32:12:</a> PLR6103 Use walrus operator `(session_factory := getattr(backend, "create_client_session", None))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/client/local_client.py#L49'>airflow/api/client/local_client.py:49:12:</a> PLR6103 Use walrus operator `(dag_run := trigger_dag.trigger_dag(dag_id=dag_id, triggered_by=DagRunTriggeredByType.CLI, run_id=run_id, conf=conf, logical_date=logical_date, replace_microseconds=replace_microseconds))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/common/airflow_health.py#L43'>airflow/api/common/airflow_health.py:43:12:</a> PLR6103 Use walrus operator `(latest_scheduler_job := SchedulerJobRunner.most_recent_job())`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/common/delete_dag.py#L60'>airflow/api/common/delete_dag.py:60:8:</a> PLR6103 Use walrus operator `(running_tis := session.scalar(select(models.TaskInstance.state).where(models.TaskInstance.dag_id == dag_id).where(models.TaskInstance.state == TaskInstanceState.RUNNING).limit(1)))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/common/delete_dag.py#L63'>airflow/api/common/delete_dag.py:63:8:</a> PLR6103 Use walrus operator `(dag := session.scalar(select(DagModel).where(DagModel.dag_id == dag_id).limit(1)))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/common/mark_tasks.py#L81'>airflow/api/common/mark_tasks.py:81:8:</a> PLR6103 Use walrus operator `(dag := next(iter(task_dags)))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/common/trigger_dag.py#L150'>airflow/api/common/trigger_dag.py:150:8:</a> PLR6103 Use walrus operator `(dag_model := DagModel.get_current(dag_id, session=session))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api/common/trigger_dag.py#L79'>airflow/api/common/trigger_dag.py:79:16:</a> PLR6103 Use walrus operator `(min_dag_start_date := dag.default_args["start_date"])`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api_connexion/endpoints/asset_endpoint.py#L197'>airflow/api_connexion/endpoints/asset_endpoint.py:197:8:</a> PLR6103 Use walrus operator `(adrq := session.scalar(select(AssetDagRunQueue).join(AssetModel, AssetDagRunQueue.asset_id == AssetModel.id).where(*where_clause)))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api_connexion/endpoints/connection_endpoint.py#L139'>airflow/api_connexion/endpoints/connection_endpoint.py:139:8:</a> PLR6103 Use walrus operator `(connection := session.scalar(select(Connection).filter_by(conn_id=connection_id).limit(1)))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api_connexion/endpoints/connection_endpoint.py#L69'>airflow/api_connexion/endpoints/connection_endpoint.py:69:8:</a> PLR6103 Use walrus operator `(connection := session.scalar(select(Connection).filter_by(conn_id=connection_id)))`.
+ <a href='https://github.com/apache/airflow/blob/971973725bd368297fda8dbe096ed6b199440ad0/airflow/api_connexion/endpoints/connection_endpoint.py#L84'>airflow/api_connexion/endpoints/connection_endpoint.py:84:8:</a> PLR6103 Use walrus operator `(connection := session.scalar(select(Connection).where(Connection.conn_id == connection_id)))`.
... 1111 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+309 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/RELEASING/changelog.py#L112'>RELEASING/changelog.py:112:12:</a> PLR6103 Use walrus operator `(github_login := self._github_login_cache.get(author_name))`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/RELEASING/changelog.py#L132'>RELEASING/changelog.py:132:12:</a> PLR6103 Use walrus operator `(pr_number := git_log.pr_number)`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/RELEASING/changelog.py#L134'>RELEASING/changelog.py:134:16:</a> PLR6103 Use walrus operator `(detail := self._pr_logs_with_details.get(pr_number))`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/RELEASING/changelog.py#L141'>RELEASING/changelog.py:141:12:</a> PLR6103 Use walrus operator `(pr_type := re.match(SUPERSET_PULL_REQUEST_TYPES, title))`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/RELEASING/changelog.py#L163'>RELEASING/changelog.py:163:16:</a> PLR6103 Use walrus operator `(risk_label := re.match(SUPERSET_RISKY_LABELS, label.name))`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/RELEASING/changelog.py#L284'>RELEASING/changelog.py:284:12:</a> PLR6103 Use walrus operator `(current_head := self._git_get_current_head())`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/RELEASING/changelog.py#L307'>RELEASING/changelog.py:307:12:</a> PLR6103 Use walrus operator `(match := re.match(r".*\(\#(\d*)\)", split_log_item[4]))`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/scripts/benchmark_migration.py#L115'>scripts/benchmark_migration.py:115:20:</a> PLR6103 Use walrus operator `(table := foreign_key.column.table.name)`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/scripts/benchmark_migration.py#L194'>scripts/benchmark_migration.py:194:16:</a> PLR6103 Use walrus operator `(missing := min_entities - model_rows[model])`.
+ <a href='https://github.com/apache/superset/blob/2c583d1584623c65d9f3972beb9e5513224e0cc2/scripts/benchmark_migration.py#L47'>scripts/benchmark_migration.py:47:8:</a> PLR6103 Use walrus operator `(spec := importlib.util.spec_from_file_location(filepath.stem, filepath))`.
... 299 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+203 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/cli/cli_config_file.py#L148'>samcli/cli/cli_config_file.py:148:20:</a> PLR6103 Use walrus operator `(allow_multiple := options_map[config_name].multiple)`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/cli/cli_config_file.py#L319'>samcli/cli/cli_config_file.py:319:12:</a> PLR6103 Use walrus operator `(param_value := ctx.params.get(param_name, None))`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/cli/command.py#L111'>samcli/cli/command.py:111:16:</a> PLR6103 Use walrus operator `(row := param.get_help_record(ctx))`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/cli/context.py#L145'>samcli/cli/context.py:145:12:</a> PLR6103 Use walrus operator `(click_core_ctx := click.get_current_context())`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/cli/context.py#L197'>samcli/cli/context.py:197:12:</a> PLR6103 Use walrus operator `(click_core_ctx := click.get_current_context())`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/cli/types.py#L322'>samcli/cli/types.py:322:12:</a> PLR6103 Use walrus operator `(equals_count := tag_value.count("="))`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/cli/types.py#L431'>samcli/cli/types.py:431:12:</a> PLR6103 Use walrus operator `(equals_count := signing_profile.count(":"))`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/commands/_utils/click_mutex.py#L76'>samcli/commands/_utils/click_mutex.py:76:20:</a> PLR6103 Use walrus operator `(has_all_required_params := False not in [required_param in opts for required_param in required_params])`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/commands/_utils/command_exception_handler.py#L75'>samcli/commands/_utils/command_exception_handler.py:75:20:</a> PLR6103 Use walrus operator `(exception_handler := (additional_mapping or {}).get(exception_type))`.
+ <a href='https://github.com/aws/aws-sam-cli/blob/bb6190948c515e986e039db58beb0d188e526f49/samcli/commands/_utils/command_exception_handler.py#L85'>samcli/commands/_utils/command_exception_handler.py:85:24:</a> PLR6103 Use walrus operator `(handler := exception_handler.get_handler(exception_type))`.
... 193 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+135 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L41'>examples/basic/data/ajax_source.py:41:12:</a> PLR6103 Use walrus operator `(requested_headers := request.headers.get('Access-Control-Request-Headers'))`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L43'>examples/basic/data/server_sent_events_source.py:43:12:</a> PLR6103 Use walrus operator `(requested_headers := request.headers.get('Access-Control-Request-Headers'))`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/markertypes.py#L35'>examples/basic/scatters/markertypes.py:35:12:</a> PLR6103 Use walrus operator `(name := f"{base}_{kind}" if kind else base)`.
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 13:12:8: PLR6103 Use walrus operator `(ksum := np.sum(kernel))`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/gapminder/main.py#L60'>examples/server/app/gapminder/main.py:60:8:</a> PLR6103 Use walrus operator `(year := slider.value + 1)`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L142'>scripts/milestone.py:142:8:</a> PLR6103 Use walrus operator `(num_types := sum(1 for label in labels if label.startswith("type:")))`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L84'>setup.py:84:8:</a> PLR6103 Use walrus operator `(missing := [fn for fn in JS_FILES if not (BUILD_JS / fn).exists()])`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L148'>src/bokeh/application/handlers/code.py:148:12:</a> PLR6103 Use walrus operator `(module := self._runner.new_module())`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L354'>src/bokeh/client/connection.py:354:20:</a> PLR6103 Use walrus operator `(message := await self._receiver.consume(fragment))`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L388'>src/bokeh/client/connection.py:388:12:</a> PLR6103 Use walrus operator `(reply := self._send_message_wait_for_reply(msg))`.
... 125 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+53 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/bootstrap.py#L63'>admin/bootstrap.py:63:8:</a> PLR6103 Use walrus operator `(return_code := popen.wait())`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/securedrop_admin/__init__.py#L1231'>admin/securedrop_admin/__init__.py:1231:12:</a> PLR6103 Use walrus operator `(return_code := args.func(args))`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/securedrop_admin/__init__.py#L139'>admin/securedrop_admin/__init__.py:139:16:</a> PLR6103 Use walrus operator `(text := document.text)`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/securedrop_admin/__init__.py#L196'>admin/securedrop_admin/__init__.py:196:16:</a> PLR6103 Use walrus operator `(text := document.text.lower())`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/securedrop_admin/__init__.py#L203'>admin/securedrop_admin/__init__.py:203:16:</a> PLR6103 Use walrus operator `(text := document.text.replace(" ", ""))`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/securedrop_admin/__init__.py#L253'>admin/securedrop_admin/__init__.py:253:16:</a> PLR6103 Use walrus operator `(text := document.text)`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/securedrop_admin/__init__.py#L267'>admin/securedrop_admin/__init__.py:267:16:</a> PLR6103 Use walrus operator `(text := document.text)`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/admin/securedrop_admin/__init__.py#L277'>admin/securedrop_admin/__init__.py:277:16:</a> PLR6103 Use walrus operator `(text := document.text)`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/install_files/ansible-base/roles/restore/files/compare_torrc.py#L24'>install_files/ansible-base/roles/restore/files/compare_torrc.py:24:16:</a> PLR6103 Use walrus operator `(m := service_re.match(line))`.
+ <a href='https://github.com/freedomofpress/securedrop/blob/9916c90820044d392323587c116a4f1522d3499d/molecule/testinfra/common/test_automatic_updates.py#L70'>molecule/testinfra/common/test_automatic_updates.py:70:8:</a> PLR6103 Use walrus operator `(distro := host.system_info.codename)`.
... 43 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/blinkpy.py#L188'>blinkpy/blinkpy.py:188:20:</a> PLR6103 Use walrus operator `(network_id := str(owl["network_id"]))`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/blinkpy.py#L212'>blinkpy/blinkpy.py:212:20:</a> PLR6103 Use walrus operator `(network_id := str(lotus["network_id"]))`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/blinkpy.py#L242'>blinkpy/blinkpy.py:242:20:</a> PLR6103 Use walrus operator `(camera_network := str(network["network_id"]))`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/blinkpy.py#L306'>blinkpy/blinkpy.py:306:12:</a> PLR6103 Use walrus operator `(last_refresh := self.last_refresh)`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/camera.py#L140'>blinkpy/camera.py:140:12:</a> PLR6103 Use walrus operator `(res := await api.request_get_config(self.sync.blink, self.network_id, self.camera_id, product_type=self.product_type))`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/camera.py#L169'>blinkpy/camera.py:169:12:</a> PLR6103 Use walrus operator `(res := await api.request_update_config(self.sync.blink, self.network_id, self.camera_id, product_type=self.product_type, data=data))`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/camera.py#L221'>blinkpy/camera.py:221:12:</a> PLR6103 Use walrus operator `(response := await self.get_media())`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/camera.py#L342'>blinkpy/camera.py:342:28:</a> PLR6103 Use walrus operator `(recent := {"time": self.last_record, "clip": self.clip})`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/camera.py#L374'>blinkpy/camera.py:374:16:</a> PLR6103 Use walrus operator `(response := await self.get_media())`.
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkpy/camera.py#L379'>blinkpy/camera.py:379:16:</a> PLR6103 Use walrus operator `(response := await self.get_media(media_type="video"))`.
... 10 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6103 | 4612 | 4612 | 0 | 0 | 0 |

</p>
</details>




---

_@vincevannoort reviewed on 2024-09-06 07:20_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1086 on 2024-09-06 07:20_

I applied this rule by checking whether the previous statement from a `IfStmt` is an `AssignStmt`, however I am wondering if it is more desirable to do check if the next statement of a `AssignStmt` is an `IfStmt`?

---

_Renamed from "[WIP] [pylint] Implement `R6103`" to "[WIP] [pylint] Implement `consider-using-assignment-expr` (`R6103`)" by @vincevannoort on 2024-09-07 12:09_

---

_Renamed from "[WIP] [pylint] Implement `consider-using-assignment-expr` (`R6103`)" to "[pylint] Implement `consider-using-assignment-expr` (`R6103`)" by @vincevannoort on 2024-09-07 12:09_

---

_Review comment by @vincevannoort on `crates/ruff_python_semantic/src/model.rs`:1439 on 2024-09-07 12:11_

I needed something like this function, not sure if there is an existing better way?

---

_@vincevannoort reviewed on 2024-09-07 12:11_

---

_Marked ready for review by @vincevannoort on 2024-09-07 12:12_

---

_@sbrugman reviewed on 2024-09-08 16:00_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:28 on 2024-09-08 16:00_

Consider adding a reference to https://peps.python.org/pep-0572/

---

_@sbrugman reviewed on 2024-09-08 16:01_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:28 on 2024-09-08 16:01_

The Python 3.8 release notes would also make a great resource:

https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:28 on 2024-09-08 16:05_

```suggestion
/// ```

## References

- [Python Documentation: What’s New In Python 3.8](https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions)
- [PEP 572 – Assignment Expressions](https://peps.python.org/pep-0572/)
```

---

_@sbrugman reviewed on 2024-09-08 16:05_

---

_@vincevannoort reviewed on 2024-09-08 17:12_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:28 on 2024-09-08 17:12_

Good suggestion, added them! 😄 

---

_Label `rule` added by @MichaReiser on 2024-09-08 19:46_

---

_Label `preview` added by @MichaReiser on 2024-09-08 19:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_assignment.py`:29 on 2024-09-08 19:54_

I like the example because it shows a potentially controversial use case. I would probably prefer the assignment to keep the if smaller. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_assignment.py`:65 on 2024-09-08 19:55_

Do we have any example where the assigned-to variable is used after the `if` condition? 

```python
good = 1

if good:
	pass

print(good)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1086 on 2024-09-08 19:57_

What benefits do you see in testing the next statement after an `AssignStmt`. 

My intuition here is that there are probably more assignment than if statements. Therefore, running the rules on if nodes might overall be faster?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:82 on 2024-09-08 19:59_

What's the motivation for cloning the `test` here? Cloning the condition does a deep-clone. This can be quiet expensive for large expressions.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:105 on 2024-09-08 19:59_

Same here. We should avoid cloning the test. 

---

_@MichaReiser requested changes on 2024-09-08 20:01_

Thanks for this contribution. My plane is about to land. I've to finish the review at a later time. 

I only had enough time to quickly glance over the Rust code. We should look into removing the many `node.clone()` calls because it is fairly expensive to clone nodes and probably unnecessary. Let me know if you need some guidance on how to remove the clone calls (It probably requires adding some lifetimes)

Regarding the rule's naming. 

* I did a quick search to see how we referred to `:=` in other rules. There are not many usages but `named expression (walrus operator)` is the most common form.
* The rule name seems too generic to me and its name is very similar to [`unnecessary-assign`](https://docs.astral.sh/ruff/rules/unnecessary-assign/) (which we should rename to `unnecessary-assignment). Reading through the examples the rule mainly is about assigning a value that is then only used in an if condition. I need to think a bit more about what a good rule name could be. Maybe you have an idea?

---

_Comment by @vincevannoort on 2024-09-10 20:15_

> Thanks for this contribution. My plane is about to land. I've to finish the review at a later time.

Thanks for the review, much appreciated! 👍 

> I only had enough time to quickly glance over the Rust code. We should look into removing the many `node.clone()` calls because it is fairly expensive to clone nodes and probably unnecessary. Let me know if you need some guidance on how to remove the clone calls (It probably requires adding some lifetimes)

I have tried and was able to remove almost all clone calls, except a few which I think are needed for returning the diagnostic. Could you take a look at the remaining ones and see if any of the 3 can still be removed?

> Regarding the rule's naming.
> 
> * I did a quick search to see how we referred to `:=` in other rules. There are not many usages but `named expression (walrus operator)` is the most common form.
> * The rule name seems too generic to me and its name is very similar to [`unnecessary-assign`](https://docs.astral.sh/ruff/rules/unnecessary-assign/) (which we should rename to `unnecessary-assignment). Reading through the examples the rule mainly is about assigning a value that is then only used in an if condition. I need to think a bit more about what a good rule name could be. Maybe you have an idea?

I agree, here are some possible options:

1. unnecessary_assignment_before_if_stmt
2. redundant_assignment_before_if _stmt
3. standalone_assignment_before_if_stmt

These seem close to what the lint is trying to prevent, do you have other ideas in mind?

---

_@vincevannoort reviewed on 2024-09-10 20:23_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1086 on 2024-09-10 20:23_

I agree that assignments are far more common than if statements.

I think the answer depends on the costs for checking `previous_statement` and `next_statement`.

My thought here is that retrieving the previous statement using the newly added `previous_statement` might be more expensive because it has to iterate over all previous statements using `previous_statements` to find the previous statement (if there is a better way, please let me know).

While checking an assignment, then checking the `next_statement` might be a cheap operation.

Do you have any idea? If they have equal cost I think the current implementation is fine. 😄 

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:82 on 2024-09-10 20:30_

I have no motivation other than that I had trouble with not cloning these before, but I have been able to make it work.

---

_@vincevannoort reviewed on 2024-09-10 20:30_

---

_@vincevannoort reviewed on 2024-09-10 20:36_

---

_Review comment by @vincevannoort on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_assignment.py`:65 on 2024-09-10 20:36_

I have added some examples for this now!

---

_Review requested from @MichaReiser by @vincevannoort on 2024-09-11 09:20_

---

_Comment by @vincevannoort on 2024-09-19 07:24_

Hey @MichaReiser, once you have the time, would you mind giving this pull request another look? :smile: 

---

_Assigned to @MichaReiser by @MichaReiser on 2024-09-20 08:41_

---

_Comment by @vincevannoort on 2024-10-19 05:28_

Hey @MichaReiser, could you or someone else from the team have a look? :smile: 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:110 on 2024-10-24 07:17_

Is there a reason why compare expressions aren't handled inside `elif_else` clauses?


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:119 on 2024-10-24 07:17_

Changing assignments to walrus operators in `elif else` branches is semantically incorrect if the variable is used afterwards

```python
>>> if True: ...
... elif x :=10: ...
... 
Ellipsis
>>> x
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:177 on 2024-10-24 07:20_

What's the motivation for calling into the generator here? We try to avoid using the generator because it removes comments. Could we instead take the assignment value as it is in the source (using locator)? Note: We have to be careful about parenthesized expressions.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1439 on 2024-10-24 07:28_

That makes sense. I think we can make it more efficient with https://github.com/astral-sh/ruff/pull/13895
which reduces an extra find and collect

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_assignment.py`:42 on 2024-10-24 07:31_

This is an interesting example and possibly controversial. I would prefer the existing solution because the assignment in the `if` is very subtle and I can see how it can be confusing when trying to figure out what the value of `bad7` is in the `elif` branch

---

_@MichaReiser requested changes on 2024-10-24 07:32_

Thanks for the ping and sorry for the late review. I pushed a few smaller refactors to avoid unnecessary collects.

I created a PR that should allow us to implement a more efficient `previous_statement`  here. 

I think the rule has to become cleverer, at least when we want to support handling `elif` cases because today's implementation can result in changes that fail at runtime. 

---

_Comment by @vincevannoort on 2024-10-24 07:41_

Thanks for the review @MichaReiser :+1: , just a heads up: I will be travelling without my laptop for the coming 5 weeks, so will only get back to pull request this after.

---

_Comment by @nickdrozd on 2025-01-29 18:03_

Hello, is this still being worked on? It is nice check, would be great to have. Especially if possible autofix would amazing.

---

_Review comment by @vincevannoort on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_assignment.py`:29 on 2025-02-13 06:44_

I agree, should we check the complexity of the assignment to determine whether to apply the rule? If so, how do we determine where to draw the line?

---

_@vincevannoort reviewed on 2025-02-13 06:44_

---

_@vincevannoort reviewed on 2025-02-13 07:07_

---

_Review comment by @vincevannoort on `crates/ruff_python_semantic/src/model.rs`:1439 on 2025-02-13 07:07_

Nice, I did that just now in https://github.com/astral-sh/ruff/pull/13196/commits/9e176d1c1878e35ff0888ceeb18ebdb0994ab4df

---

_@vincevannoort reviewed on 2025-02-13 07:08_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1086 on 2025-02-13 07:08_

I can change it, to the suggested approach if you want 😄 

---

_@vincevannoort reviewed on 2025-02-13 07:09_

---

_Review comment by @vincevannoort on `crates/ruff_linter/resources/test/fixtures/pylint/unnecessary_assignment.py`:42 on 2025-02-13 07:09_

Yeah, I think you are right. Due to that I decided to only apply the rule to simple `if` statements, where simple means without `elif` or `else` attached to the `if`.

---

_@vincevannoort reviewed on 2025-02-13 07:12_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:177 on 2025-02-13 07:12_

Could you help me out here? I am not that familiar in how to do this properly.

---

_@vincevannoort reviewed on 2025-02-13 07:13_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:119 on 2025-02-13 07:13_

Yeah, I think you are right. Due to that I decided to only apply the rule to simple `if` statements, where simple means without `elif` or `else` attached to the `if`. That should prevent cases like this.

---

_Comment by @vincevannoort on 2025-02-13 07:15_

@MichaReiser sorry for getting back so late to this PR, and thanks for all the previous input.

I have rebased to the latest changes on main, and fixed some issues mentioned in earlier comments. Additionally, I changed the rule to only apply to simple `if` statements, that do not have an `elif` or `else` attached. This should hopefully resolve most of the more controversial cases.

Coul you take another look?

---

_Review requested from @MichaReiser by @vincevannoort on 2025-02-13 07:17_

---

_Review comment by @vincevannoort on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_assignment.rs`:116 on 2025-02-13 07:32_

I am curious if we should actually do this part. If I look at the ecosystem checks, these also produce some possible controversial results. 🤔 

Without it, I think most of the more controversial results would disappear.

---

_@vincevannoort reviewed on 2025-02-13 07:32_

---

_Comment by @MichaReiser on 2025-02-13 08:52_

Hmm, I just noticed that this is a pylint extensions rule which we don't want to add right now because it would require a new rule group.

---

_Comment by @vincevannoort on 2025-02-13 09:25_

> Hmm, I just noticed that this is a pylint extensions rule which we don't want to add right now because it would require a new rule group.

What would you recommend in this case? Just close the pull request or make it a new ruff rule instead of pylint rule?

---

_Comment by @MichaReiser on 2025-02-20 08:46_

Yeah, I think we have to close this for now because there's no clear path to landing this PR before #1774 is complete. I'm very sorry for wasting your time here. I should have noticed this way sooner. But thank you anyway and we can come back to this PR once we're at a point where we accept pylint extension rules.

---

_Closed by @MichaReiser on 2025-02-20 08:46_

---

_Comment by @vincevannoort on 2025-02-20 08:53_

> Yeah, I think we have to close this for now because there's no clear path to landing this PR before #1774 is complete. I'm very sorry for wasting your time here. I should have noticed this way sooner. But thank you anyway and we can come back to this PR once we're at a point where we accept pylint extension rules.

No worries :smile: , I still learned quite a lot from the implementation so it was still worth it for me personally. Thanks for the effort in the feedback rounds. :+1: 

---

```yaml
number: 9662
title: "SIM108: preserve quoting style in autofixes; don't flag `if`/`else` blocks where the `if`-statement spans multiple lines"
type: pull_request
state: closed
author: AlexWaygood
labels: []
assignees: []
base: main
head: main
created_at: 2024-01-28T13:38:57Z
updated_at: 2024-01-30T08:00:15Z
url: https://github.com/astral-sh/ruff/pull/9662
synced_at: 2026-01-10T22:57:09Z
```

# SIM108: preserve quoting style in autofixes; don't flag `if`/`else` blocks where the `if`-statement spans multiple lines

---

_Pull request opened by @AlexWaygood on 2024-01-28 13:38_

Fixes #9660. Instead of using the expression generator, copy and paste the source-code expression exactly as it is.

---

_Comment by @github-actions[bot] on 2024-01-28 13:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -93 violations, +0 -0 fixes in 6 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/api_connexion/endpoints/dag_run_endpoint.py#L231'>airflow/api_connexion/endpoints/dag_run_endpoint.py:231:5:</a> SIM108 Use ternary operator `query = query.where(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/api_connexion/exceptions.py#L47'>airflow/api_connexion/exceptions.py:47:9:</a> SIM108 Use ternary operator `response = problem(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L109'>airflow/metrics/datadog_logger.py:109:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L128'>airflow/metrics/datadog_logger.py:128:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L148'>airflow/metrics/datadog_logger.py:148:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L68'>airflow/metrics/datadog_logger.py:68:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L88'>airflow/metrics/datadog_logger.py:88:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/models/dagcode.py#L88'>airflow/models/dagcode.py:88:9:</a> SIM108 Use ternary operator `existing_orm_dag_codes_map = {
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/notifications/basenotifier.py#L93'>airflow/notifications/basenotifier.py:93:9:</a> SIM108 Use ternary operator `context = args[0] if len(args) == 1 else {
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/amazon/aws/triggers/redshift_data.py#L95'>airflow/providers/amazon/aws/triggers/redshift_data.py:95:13:</a> SIM108 Use ternary operator `response = {"status": "success", "statement_id": self.statement_id} if is_finished else {
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/apache/hive/hooks/hive.py#L665'>airflow/providers/apache/hive/hooks/hive.py:665:17:</a> SIM108 Use ternary operator `parts = client.get_partitions_by_filter(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/apache/livy/hooks/livy.py#L134'>airflow/providers/apache/livy/hooks/livy.py:134:13:</a> SIM108 Use ternary operator `result = self.run_with_advanced_retry(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/cncf/kubernetes/python_kubernetes_script.py#L82'>airflow/providers/cncf/kubernetes/python_kubernetes_script.py:82:5:</a> SIM108 Use ternary operator `template_env = jinja2.nativetypes.NativeEnvironment(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/compute.py#L750'>airflow/providers/google/cloud/hooks/compute.py:750:13:</a> SIM108 Use ternary operator `operation_response = self._check_global_operation_status(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/compute_ssh.py#L233'>airflow/providers/google/cloud/hooks/compute_ssh.py:233:9:</a> SIM108 Use ternary operator `hostname = self._compute_hook.get_instance_address(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/gcs.py#L805'>airflow/providers/google/cloud/hooks/gcs.py:805:13:</a> SIM108 Use ternary operator `blobs = self._list_blobs_with_match_glob(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/gcs.py#L918'>airflow/providers/google/cloud/hooks/gcs.py:918:13:</a> SIM108 Use ternary operator `blobs = self._list_blobs_with_match_glob(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/operators/dataplex.py#L1686'>airflow/providers/google/cloud/operators/dataplex.py:1686:9:</a> SIM108 Use ternary operator `job = hook.wait_for_data_scan_job(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/common/auth_backend/google_openid.py#L45'>airflow/providers/google/common/auth_backend/google_openid.py:45:5:</a> SIM108 Use ternary operator `id_token_credentials = service_account.IDTokenCredentials.from_service_account_file(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/suite/hooks/drive.py#L231'>airflow/providers/google/suite/hooks/drive.py:231:9:</a> SIM108 Use ternary operator `files = service.files()
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -26 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/examples/flower_classifier/score_images_rest.py#L26'>examples/flower_classifier/score_images_rest.py:26:5:</a> SIM108 Use ternary operator `filenames = [
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/examples/flower_classifier/score_images_spark.py#L39'>examples/flower_classifier/score_images_spark.py:39:5:</a> SIM108 Use ternary operator `filenames = [
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/_spark_autologging.py#L265'>mlflow/_spark_autologging.py:265:13:</a> SIM108 Use ternary operator `tags = {
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/data/numpy_dataset.py#L212'>mlflow/data/numpy_dataset.py:212:9:</a> SIM108 Use ternary operator `resolved_source = source if isinstance(source, DatasetSource) else resolve_dataset_source(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/data/pandas_dataset.py#L217'>mlflow/data/pandas_dataset.py:217:9:</a> SIM108 Use ternary operator `resolved_source = source if isinstance(source, DatasetSource) else resolve_dataset_source(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/data/tensorflow_dataset.py#L282'>mlflow/data/tensorflow_dataset.py:282:9:</a> SIM108 Use ternary operator `resolved_source = source if isinstance(source, DatasetSource) else resolve_dataset_source(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/base.py#L1029'>mlflow/models/evaluation/base.py:1029:13:</a> SIM108 Use ternary operator `relative_change = (
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/base.py#L1851'>mlflow/models/evaluation/base.py:1851:13:</a> SIM108 Use ternary operator `data = _convert_data_to_mlflow_dataset(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/default_evaluator.py#L1633'>mlflow/models/evaluation/default_evaluator.py:1633:13:</a> SIM108 Use ternary operator `data = data.assign(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/default_evaluator.py#L889'>mlflow/models/evaluation/default_evaluator.py:889:17:</a> SIM108 Use ternary operator `explainer = shap.Explainer(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/default_evaluator.py#L908'>mlflow/models/evaluation/default_evaluator.py:908:13:</a> SIM108 Use ternary operator `shap_values = shap.Explanation(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/pyfunc/__init__.py#L1174'>mlflow/pyfunc/__init__.py:1174:13:</a> SIM108 Use ternary operator `field_values = field_values.astype(np_type) if is_pandas_df else None
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/pyfunc/__init__.py#L1183'>mlflow/pyfunc/__init__.py:1183:13:</a> SIM108 Use ternary operator `field_values = pandas.Series(
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/pypa/cibuildwheel/blob/34b049f1389fd9cfef5de1d1781ec67759770eea/bin/generate_schema.py#L223'>bin/generate_schema.py:223:1:</a> SIM108 Use ternary operator `non_global_options = {
- <a href='https://github.com/pypa/cibuildwheel/blob/34b049f1389fd9cfef5de1d1781ec67759770eea/test/utils.py#L152'>test/utils.py:152:9:</a> SIM108 Use ternary operator `manylinux_versions = [
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/src/poetry/mixology/version_solver.py#L76'>src/poetry/mixology/version_solver.py:76:9:</a> SIM108 Use ternary operator `packages = [
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/console/commands/test_new.py#L52'>tests/console/commands/test_new.py:52:5:</a> SIM108 Use ternary operator `package_include = {
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/console/commands/test_run.py#L184'>tests/console/commands/test_run.py:184:5:</a> SIM108 Use ternary operator `expected_message = "" if installed_script else """\
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/puzzle/test_solver.py#L3797'>tests/puzzle/test_solver.py:3797:5:</a> SIM108 Use ternary operator `expected = [
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/utils/test_cache.py#L198'>tests/utils/test_cache.py:198:5:</a> SIM108 Use ternary operator `expected = Path(
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/reflex-dev/reflex/blob/8203b888fe789887cd1bf0f0abb330f9f55a5e16/reflex/app.py#L415'>reflex/app.py:415:9:</a> SIM108 Use ternary operator `component = Fragment.create(
- <a href='https://github.com/reflex-dev/reflex/blob/8203b888fe789887cd1bf0f0abb330f9f55a5e16/reflex/components/chakra/datadisplay/table.py#L143'>reflex/components/chakra/datadisplay/table.py:143:13:</a> SIM108 Use ternary operator `children = [
- <a href='https://github.com/reflex-dev/reflex/blob/8203b888fe789887cd1bf0f0abb330f9f55a5e16/reflex/components/chakra/forms/rangeslider.py#L108'>reflex/components/chakra/forms/rangeslider.py:108:13:</a> SIM108 Use ternary operator `children = [
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -19 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/corporate/lib/analytics.py#L105'>corporate/lib/analytics.py:105:9:</a> SIM108 Use ternary operator `renewal_cents = 0 if plan.tier in (
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/corporate/lib/analytics.py#L169'>corporate/lib/analytics.py:169:9:</a> SIM108 Use ternary operator `renewal_cents = 0 if plan.tier in (
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/actions/invites.py#L342'>zerver/actions/invites.py:342:5:</a> SIM108 Use ternary operator `prereg_users = filter_to_valid_prereg_users(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/bot_lib.py#L120'>zerver/lib/bot_lib.py:120:9:</a> SIM108 Use ternary operator `result = self.send_message(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/events.py#L576'>zerver/lib/events.py:576:9:</a> SIM108 Use ternary operator `sub_info = gather_subscriptions_helper(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/migrate.py#L105'>zerver/lib/migrate.py:105:17:</a> SIM108 Use ternary operator `raw_query = SQL("ALTER INDEX {old_name} RENAME TO {new_name}").format(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/migrate.py#L93'>zerver/lib/migrate.py:93:21:</a> SIM108 Use ternary operator `raw_query = SQL("DROP INDEX {old_name}").format(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/send_email.py#L601'>zerver/lib/send_email.py:601:5:</a> SIM108 Use ternary operator `users = users.annotate(lower_email=Lower("delivery_email"))
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/streams.py#L849'>zerver/lib/streams.py:849:5:</a> SIM108 Use ternary operator `stream_weekly_traffic = get_average_weekly_stream_traffic(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/subscription_info.py#L176'>zerver/lib/subscription_info.py:176:5:</a> SIM108 Use ternary operator `stream_weekly_traffic = get_average_weekly_stream_traffic(
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 93 | 0 | 93 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -93 violations, +0 -0 fixes in 6 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/api_connexion/endpoints/dag_run_endpoint.py#L231'>airflow/api_connexion/endpoints/dag_run_endpoint.py:231:5:</a> SIM108 Use ternary operator `query = query.where(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/api_connexion/exceptions.py#L47'>airflow/api_connexion/exceptions.py:47:9:</a> SIM108 Use ternary operator `response = problem(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L109'>airflow/metrics/datadog_logger.py:109:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L128'>airflow/metrics/datadog_logger.py:128:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L148'>airflow/metrics/datadog_logger.py:148:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L68'>airflow/metrics/datadog_logger.py:68:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/metrics/datadog_logger.py#L88'>airflow/metrics/datadog_logger.py:88:9:</a> SIM108 Use ternary operator `tags_list = [
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/models/dagcode.py#L88'>airflow/models/dagcode.py:88:9:</a> SIM108 Use ternary operator `existing_orm_dag_codes_map = {
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/notifications/basenotifier.py#L93'>airflow/notifications/basenotifier.py:93:9:</a> SIM108 Use ternary operator `context = args[0] if len(args) == 1 else {
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/amazon/aws/triggers/redshift_data.py#L95'>airflow/providers/amazon/aws/triggers/redshift_data.py:95:13:</a> SIM108 Use ternary operator `response = {"status": "success", "statement_id": self.statement_id} if is_finished else {
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/apache/hive/hooks/hive.py#L665'>airflow/providers/apache/hive/hooks/hive.py:665:17:</a> SIM108 Use ternary operator `parts = client.get_partitions_by_filter(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/apache/livy/hooks/livy.py#L134'>airflow/providers/apache/livy/hooks/livy.py:134:13:</a> SIM108 Use ternary operator `result = self.run_with_advanced_retry(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/cncf/kubernetes/python_kubernetes_script.py#L82'>airflow/providers/cncf/kubernetes/python_kubernetes_script.py:82:5:</a> SIM108 Use ternary operator `template_env = jinja2.nativetypes.NativeEnvironment(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/compute.py#L750'>airflow/providers/google/cloud/hooks/compute.py:750:13:</a> SIM108 Use ternary operator `operation_response = self._check_global_operation_status(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/compute_ssh.py#L233'>airflow/providers/google/cloud/hooks/compute_ssh.py:233:9:</a> SIM108 Use ternary operator `hostname = self._compute_hook.get_instance_address(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/gcs.py#L805'>airflow/providers/google/cloud/hooks/gcs.py:805:13:</a> SIM108 Use ternary operator `blobs = self._list_blobs_with_match_glob(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/hooks/gcs.py#L918'>airflow/providers/google/cloud/hooks/gcs.py:918:13:</a> SIM108 Use ternary operator `blobs = self._list_blobs_with_match_glob(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/cloud/operators/dataplex.py#L1686'>airflow/providers/google/cloud/operators/dataplex.py:1686:9:</a> SIM108 Use ternary operator `job = hook.wait_for_data_scan_job(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/common/auth_backend/google_openid.py#L45'>airflow/providers/google/common/auth_backend/google_openid.py:45:5:</a> SIM108 Use ternary operator `id_token_credentials = service_account.IDTokenCredentials.from_service_account_file(
- <a href='https://github.com/apache/airflow/blob/0fce3b6047dcae037cfd8a5bd0638894c36509ab/airflow/providers/google/suite/hooks/drive.py#L231'>airflow/providers/google/suite/hooks/drive.py:231:9:</a> SIM108 Use ternary operator `files = service.files()
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -26 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/examples/flower_classifier/score_images_rest.py#L26'>examples/flower_classifier/score_images_rest.py:26:5:</a> SIM108 Use ternary operator `filenames = [
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/examples/flower_classifier/score_images_spark.py#L39'>examples/flower_classifier/score_images_spark.py:39:5:</a> SIM108 Use ternary operator `filenames = [
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/_spark_autologging.py#L265'>mlflow/_spark_autologging.py:265:13:</a> SIM108 Use ternary operator `tags = {
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/data/numpy_dataset.py#L212'>mlflow/data/numpy_dataset.py:212:9:</a> SIM108 Use ternary operator `resolved_source = source if isinstance(source, DatasetSource) else resolve_dataset_source(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/data/pandas_dataset.py#L217'>mlflow/data/pandas_dataset.py:217:9:</a> SIM108 Use ternary operator `resolved_source = source if isinstance(source, DatasetSource) else resolve_dataset_source(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/data/tensorflow_dataset.py#L282'>mlflow/data/tensorflow_dataset.py:282:9:</a> SIM108 Use ternary operator `resolved_source = source if isinstance(source, DatasetSource) else resolve_dataset_source(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/base.py#L1029'>mlflow/models/evaluation/base.py:1029:13:</a> SIM108 Use ternary operator `relative_change = (
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/base.py#L1851'>mlflow/models/evaluation/base.py:1851:13:</a> SIM108 Use ternary operator `data = _convert_data_to_mlflow_dataset(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/default_evaluator.py#L1633'>mlflow/models/evaluation/default_evaluator.py:1633:13:</a> SIM108 Use ternary operator `data = data.assign(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/default_evaluator.py#L889'>mlflow/models/evaluation/default_evaluator.py:889:17:</a> SIM108 Use ternary operator `explainer = shap.Explainer(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/models/evaluation/default_evaluator.py#L908'>mlflow/models/evaluation/default_evaluator.py:908:13:</a> SIM108 Use ternary operator `shap_values = shap.Explanation(
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/pyfunc/__init__.py#L1174'>mlflow/pyfunc/__init__.py:1174:13:</a> SIM108 Use ternary operator `field_values = field_values.astype(np_type) if is_pandas_df else None
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/mlflow/pyfunc/__init__.py#L1183'>mlflow/pyfunc/__init__.py:1183:13:</a> SIM108 Use ternary operator `field_values = pandas.Series(
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/cibuildwheel/blob/34b049f1389fd9cfef5de1d1781ec67759770eea/bin/generate_schema.py#L223'>bin/generate_schema.py:223:1:</a> SIM108 Use ternary operator `non_global_options = {
- <a href='https://github.com/pypa/cibuildwheel/blob/34b049f1389fd9cfef5de1d1781ec67759770eea/test/utils.py#L152'>test/utils.py:152:9:</a> SIM108 Use ternary operator `manylinux_versions = [
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/src/poetry/mixology/version_solver.py#L76'>src/poetry/mixology/version_solver.py:76:9:</a> SIM108 Use ternary operator `packages = [
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/console/commands/test_new.py#L52'>tests/console/commands/test_new.py:52:5:</a> SIM108 Use ternary operator `package_include = {
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/console/commands/test_run.py#L184'>tests/console/commands/test_run.py:184:5:</a> SIM108 Use ternary operator `expected_message = "" if installed_script else """\
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/puzzle/test_solver.py#L3797'>tests/puzzle/test_solver.py:3797:5:</a> SIM108 Use ternary operator `expected = [
- <a href='https://github.com/python-poetry/poetry/blob/ed1dcddebb6337997829df5b6c73b020cdcdee99/tests/utils/test_cache.py#L198'>tests/utils/test_cache.py:198:5:</a> SIM108 Use ternary operator `expected = Path(
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/reflex-dev/reflex/blob/8203b888fe789887cd1bf0f0abb330f9f55a5e16/reflex/app.py#L415'>reflex/app.py:415:9:</a> SIM108 Use ternary operator `component = Fragment.create(
- <a href='https://github.com/reflex-dev/reflex/blob/8203b888fe789887cd1bf0f0abb330f9f55a5e16/reflex/components/chakra/datadisplay/table.py#L143'>reflex/components/chakra/datadisplay/table.py:143:13:</a> SIM108 Use ternary operator `children = [
- <a href='https://github.com/reflex-dev/reflex/blob/8203b888fe789887cd1bf0f0abb330f9f55a5e16/reflex/components/chakra/forms/rangeslider.py#L108'>reflex/components/chakra/forms/rangeslider.py:108:13:</a> SIM108 Use ternary operator `children = [
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -19 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/corporate/lib/analytics.py#L105'>corporate/lib/analytics.py:105:9:</a> SIM108 Use ternary operator `renewal_cents = 0 if plan.tier in (
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/corporate/lib/analytics.py#L169'>corporate/lib/analytics.py:169:9:</a> SIM108 Use ternary operator `renewal_cents = 0 if plan.tier in (
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/actions/invites.py#L342'>zerver/actions/invites.py:342:5:</a> SIM108 Use ternary operator `prereg_users = filter_to_valid_prereg_users(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/bot_lib.py#L120'>zerver/lib/bot_lib.py:120:9:</a> SIM108 Use ternary operator `result = self.send_message(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/events.py#L576'>zerver/lib/events.py:576:9:</a> SIM108 Use ternary operator `sub_info = gather_subscriptions_helper(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/migrate.py#L105'>zerver/lib/migrate.py:105:17:</a> SIM108 Use ternary operator `raw_query = SQL("ALTER INDEX {old_name} RENAME TO {new_name}").format(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/migrate.py#L93'>zerver/lib/migrate.py:93:21:</a> SIM108 Use ternary operator `raw_query = SQL("DROP INDEX {old_name}").format(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/send_email.py#L601'>zerver/lib/send_email.py:601:5:</a> SIM108 Use ternary operator `users = users.annotate(lower_email=Lower("delivery_email"))
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/streams.py#L849'>zerver/lib/streams.py:849:5:</a> SIM108 Use ternary operator `stream_weekly_traffic = get_average_weekly_stream_traffic(
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/lib/subscription_info.py#L176'>zerver/lib/subscription_info.py:176:5:</a> SIM108 Use ternary operator `stream_weekly_traffic = get_average_weekly_stream_traffic(
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 93 | 0 | 93 | 0 | 0 |

</p>
</details>




---

_@AlexWaygood reviewed on 2024-01-28 14:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:123 on 2024-01-28 14:20_

This restriction wasn't necessary with the old approach that used the expression generator, but it makes the "cut-and-paste" approach much easier. The restriction has the side effect of making a lot of existing violations in the ecosystem check go away.

However, I think that might actually be desirable? Here's an `if`/`else` expression in `cibuildwheel` that's no longer getting flagged. I think it's pretty clear from looking at it that they've deliberately chosen an `if`/`else` block here instead of a ternary precisely _because_ the expression in the `if` block stretches over multiple lines. It wouldn't really simplify the code here to change this into a ternary expression:

```py
if args.schemastore:
    non_global_options = {
        k: {"$ref": f"#/properties/tool/properties/cibuildwheel/properties/{k}"}
        for k in schema["properties"]
    }
else:
    non_global_options = {k: {"$ref": f"#/properties/{k}"} for k in schema["properties"]}
```

So arguably this new restriction gets rid of a bunch of false positives :)

---

_Renamed from "Preserve quoting style when fixing SIM108 violations" to "SIM108: preserve quoting style in autofixes; don't flag `if`/`else` statements where the `if`-statement spans multiple lines" by @AlexWaygood on 2024-01-28 14:21_

---

_Renamed from "SIM108: preserve quoting style in autofixes; don't flag `if`/`else` statements where the `if`-statement spans multiple lines" to "SIM108: preserve quoting style in autofixes; don't flag `if`/`else` blocks where the `if`-statement spans multiple lines" by @AlexWaygood on 2024-01-28 14:22_

---

_@charliermarsh reviewed on 2024-01-28 19:02_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:138 on 2024-01-28 19:02_

I tend to do `.as_ref()` here instead, but maybe @BurntSushi does not like that.

---

_@charliermarsh reviewed on 2024-01-28 19:03_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:123 on 2024-01-28 19:03_

I generally agree with the change. Only question for me is whether it should be in preview. I would be comfortable shipping it to stable since it's a _reduction_ in scope, so shouldn't generally introduce new violations. \cc @zanieb for a second opinion.

---

_@charliermarsh approved on 2024-01-28 19:03_

Looks good to me! One question regarding whether this should go out under preview, so lets just resolve that first.

---

_@AlexWaygood reviewed on 2024-01-28 19:09_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:123 on 2024-01-28 19:09_

Yeah... I guess the only "backwards-incompatible" change here is that people might have `# noqa` comments in their code to suppress violations on this kind of code — those comments could now be flagged by ruff as unused? But I feel like people will mostly be happy if they get to delete `# noqa` comments, so I lean towards arguing it should go straight into stable. But no strong opinion either way :)

---

_@charliermarsh reviewed on 2024-01-28 19:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:123 on 2024-01-28 19:24_

Yeah I feel the same.

---

_@BurntSushi reviewed on 2024-01-28 19:28_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:138 on 2024-01-28 19:28_

I'd say the code as written here is idiomatic. Basically, what you have here is a `&Box<Expr>` and you want a `&Expr`. While it's true that `as_ref()` will work here, it only works because the set of possible trait impls that apply is exactly `1`. If the number of trait impls increases such that it becomes ambiguous, the code will stop compiling. I think in _this_ case, it _seems_ like the relevant trait impls are very unlikely to increase. Which makes `as_ref()` in this particular case probably pretty safe. The problem is that adjudicating which `as_ref()` calls are probably fine and will never break, and which are not is IMO extremely tricky. So it's better to just avoid it unless you have an explicit `AsRef<TargetType>` in scope somewhere (or some other explicit annotation that makes the `as_ref()` target unambiguous).

---

_@zanieb reviewed on 2024-01-29 00:09_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:123 on 2024-01-29 00:09_

Oh interesting... I prefer to write these as ternary expressions even when there are line breaks and would be surprised to see the lint go away. I don't think there's any requirement that we ship a reduction in scope in preview but I think we may want to here. I don't actually see any "unused noqa" changes in the ecosystem so I don't know if we have strong evidence that a line break is a common reason for ignoring this rule. 

A search for [`# noqa: SIM108`](https://github.com/search?q=%23+noqa%3A+SIM108&type=code) may be interesting. Very few cases where there are multiple lines in the brief perusal I just did. It seems like we might want to add an exception to `if TYPE_CHECKING` blocks though!



---

_Comment by @charliermarsh on 2024-01-29 03:21_

@AlexWaygood - There may be a separate fix to apply here (in addition or in lieu of this change). In the generator, we have:

```rust
fn unparse_f_string(&mut self, values: &[ast::FStringElement], is_spec: bool) {
    if is_spec {
        self.unparse_f_string_body(values);
    } else {
        self.p("f");
        let mut generator = Generator::new(
            self.indent,
            match self.quote {
                Quote::Single => Quote::Double,
                Quote::Double => Quote::Single,
            },
            self.line_ending,
        );
        generator.unparse_f_string_body(values);
        let body = &generator.buffer;
        self.p_str_repr(body);
    }
}
```

So, when we recurse into an f-string, we alternate the preferred quote style. If the target version is >= Python 3.12, though, we should skip this and preserve the preferred quote style.

---

_Comment by @AlexWaygood on 2024-01-30 07:58_

Closing this for now -- it seems like the wrong fix, and I accidentally pushed my changes straight to the `main` branch on my fork (😱) which is a bit of a pain for working on other patches etc. I'll try to open a PR with the correct fix soon :)

---

_Closed by @AlexWaygood on 2024-01-30 07:58_

---

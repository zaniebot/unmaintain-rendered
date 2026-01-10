```yaml
number: 9553
title: "[`pylint`] (Re-)Implement `import-private-name` (`C2701`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-import-private-name
created_at: 2024-01-16T18:06:34Z
updated_at: 2024-01-16T21:02:42Z
url: https://github.com/astral-sh/ruff/pull/9553
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] (Re-)Implement `import-private-name` (`C2701`)

---

_Pull request opened by @tjkuson on 2024-01-16 18:06_

## Summary

#5920 with a fix for the erroneous slice in `module_name`. Fixes #9547.

## Test Plan

Added `import bbb.ccc._ddd as eee` to the test fixture to ensure it no longer panics.

`cargo test`


---

_Marked ready for review by @tjkuson on 2024-01-16 18:18_

---

_Comment by @github-actions[bot] on 2024-01-16 18:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+501 -0 violations, +0 -0 fixes in 12 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/base_core.py#L26'>disnake/ext/commands/base_core.py:26:27:</a> PLC2701 Private name import `_generated` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/base_core.py#L26'>disnake/ext/commands/base_core.py:26:39:</a> PLC2701 Private name import `_overload_with_permissions` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/core.py#L31'>disnake/ext/commands/core.py:31:5:</a> PLC2701 Private name import `_generated` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/core.py#L32'>disnake/ext/commands/core.py:32:5:</a> PLC2701 Private name import `_overload_with_permissions` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/flags.py#L8'>disnake/ext/commands/flags.py:8:27:</a> PLC2701 Private name import `_generated` from external module `disnake.utils`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/params.py#L41'>disnake/ext/commands/params.py:41:29:</a> PLC2701 Private name import `_channel_type_factory` from external module `disnake.channel`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/docs/extensions/attributetable.py#L13'>docs/extensions/attributetable.py:13:27:</a> PLC2701 Private name import `_` from external module `sphinx.locale`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+102 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/api/auth/backend/kerberos_auth.py#L55'>airflow/api/auth/backend/kerberos_auth.py:55:51:</a> PLC2701 Private name import `_request_ctx_stack` from external module `flask`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/io/path.py#L29'>airflow/io/path.py:29:52:</a> PLC2701 Private name import `_CloudAccessor` from external module `upath.implementations.cloud`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/metrics/otel_logger.py#L30'>airflow/metrics/otel_logger.py:30:56:</a> PLC2701 Private name import `_internal` from external module `opentelemetry.sdk.metrics`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/metrics/otel_logger.py#L30'>airflow/metrics/otel_logger.py:30:79:</a> PLC2701 Private name import `_internal` from external module `opentelemetry.sdk.metrics`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/providers/google/cloud/hooks/gcs.py#L858'>airflow/providers/google/cloud/hooks/gcs.py:858:49:</a> PLC2701 Private name import `_blobs_page_start` from external module `google.cloud.storage.bucket`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/providers/google/cloud/hooks/gcs.py#L858'>airflow/providers/google/cloud/hooks/gcs.py:858:68:</a> PLC2701 Private name import `_item_to_blob` from external module `google.cloud.storage.bucket`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/providers/google/common/hooks/base_google.py#L39'>airflow/providers/google/common/hooks/base_google.py:39:25:</a> PLC2701 Private name import `_cloud_sdk` from external module `google.auth`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/providers/google/common/hooks/base_google.py#L42'>airflow/providers/google/common/hooks/base_google.py:42:35:</a> PLC2701 Private name import `_http_client` from external module `google.auth.transport`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/providers/google/common/utils/id_token_credentials.py#L149'>airflow/providers/google/common/utils/id_token_credentials.py:149:29:</a> PLC2701 Private name import `_cloud_sdk` from external module `google.auth`
+ <a href='https://github.com/apache/airflow/blob/a22ad47effd6df5b460dac9912aa98009caa44f5/airflow/providers/google/common/utils/id_token_credentials.py#L175'>airflow/providers/google/common/utils/id_token_credentials.py:175:48:</a> PLC2701 Private name import `_metadata` from external module `google.auth.compute_engine`
... 92 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+187 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/schema/make_schema.py#L12'>schema/make_schema.py:12:32:</a> PLC2701 Private name import `_SAM_CLI_COMMAND_PACKAGES` from external module `samcli.cli.command`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/integration/package/test_package_command_image.py#L13'>tests/integration/package/test_package_command_image.py:13:45:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/integration/sync/test_sync_adl.py#L5'>tests/integration/sync/test_sync_adl.py:5:49:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/integration/sync/test_sync_adl.py#L5'>tests/integration/sync/test_sync_adl.py:5:67:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py#L7'>tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py:7:68:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py#L7'>tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py:7:84:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_option_nargs.py#L4'>tests/unit/commands/_utils/custom_options/test_option_nargs.py:4:64:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_replace_help_summary_option.py#L4'>tests/unit/commands/_utils/custom_options/test_replace_help_summary_option.py:4:71:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_cdk_support_decorators.py#L4'>tests/unit/commands/_utils/test_cdk_support_decorators.py:4:59:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_click_mutex.py#L3'>tests/unit/commands/_utils/test_click_mutex.py:3:48:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_command_exception_handler.py#L7'>tests/unit/commands/_utils/test_command_exception_handler.py:7:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_command_exception_handler.py#L8'>tests/unit/commands/_utils/test_command_exception_handler.py:8:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_command_exception_handler.py#L9'>tests/unit/commands/_utils/test_command_exception_handler.py:9:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_experimental.py#L10'>tests/unit/commands/_utils/test_experimental.py:10:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_experimental.py#L11'>tests/unit/commands/_utils/test_experimental.py:11:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_experimental.py#L12'>tests/unit/commands/_utils/test_experimental.py:12:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_experimental.py#L13'>tests/unit/commands/_utils/test_experimental.py:13:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_experimental.py#L14'>tests/unit/commands/_utils/test_experimental.py:14:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
... 169 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+108 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L39'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:39:27:</a> PLC2701 Private name import `_` from external module `sphinx.locale`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/command/subcommands/test_json__subcommands.py#L29'>tests/unit/bokeh/command/subcommands/test_json__subcommands.py:29:31:</a> PLC2701 Private name import `_util_subcommands`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_aliases.py#L29'>tests/unit/bokeh/core/property/test_aliases.py:29:28:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_aliases.py#L29'>tests/unit/bokeh/core/property/test_aliases.py:29:43:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_any.py#L22'>tests/unit/bokeh/core/property/test_any.py:22:28:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_any.py#L22'>tests/unit/bokeh/core/property/test_any.py:22:43:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_auto.py#L22'>tests/unit/bokeh/core/property/test_auto.py:22:28:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_auto.py#L22'>tests/unit/bokeh/core/property/test_auto.py:22:43:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_color__property.py#L23'>tests/unit/bokeh/core/property/test_color__property.py:23:28:</a> PLC2701 Private name import `_util_property`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/core/property/test_color__property.py#L23'>tests/unit/bokeh/core/property/test_color__property.py:23:43:</a> PLC2701 Private name import `_util_property`
... 98 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/secure_tempfile.py#L3'>securedrop/secure_tempfile.py:3:22:</a> PLC2701 Private name import `_TemporaryFileWrapper` from external module `tempfile`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/conftest.py#L24'>securedrop/tests/conftest.py:24:25:</a> PLC2701 Private name import `_SourceScryptManager` from external module `source_user`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/test_config.py#L3'>securedrop/tests/test_config.py:3:22:</a> PLC2701 Private name import `_parse_config_from_file` from external module `sdconfig`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/test_source_user.py#L11'>securedrop/tests/test_source_user.py:11:5:</a> PLC2701 Private name import `_DesignationGenerator` from external module `source_user`
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/test_source_user.py#L12'>securedrop/tests/test_source_user.py:12:5:</a> PLC2701 Private name import `_SourceScryptManager` from external module `source_user`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/backends/app/backend_info_app.py#L14'>docs/backends/app/backend_info_app.py:14:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py#L6'>docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py:6:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/posts/pydata-performance-part2/datafusion_ibis.py#L4'>docs/posts/pydata-performance-part2/datafusion_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/posts/pydata-performance-part2/duckdb_ibis.py#L4'>docs/posts/pydata-performance-part2/duckdb_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/posts/pydata-performance-part2/polars_ibis.py#L4'>docs/posts/pydata-performance-part2/polars_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/posts/pydata-performance/step0.py#L4'>docs/posts/pydata-performance/step0.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/posts/pydata-performance/step1.py#L4'>docs/posts/pydata-performance/step1.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/posts/pydata-performance/step2.py#L4'>docs/posts/pydata-performance/step2.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/docs/posts/pydata-performance/step3.py#L4'>docs/posts/pydata-performance/step3.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/4b73c3be5690cccf212bda175a185867f81b8e4d/ibis/backends/conftest.py#L11'>ibis/backends/conftest.py:11:8:</a> PLC2701 Private name import `_pytest`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/d7a402b8cfdd24d3e5626da817de828ca14d8acb/_version_helper.py#L23'>_version_helper.py:23:5:</a> PLC2701 Private name import `_parse_version_tag` from external module `setuptools_scm.version`
+ <a href='https://github.com/milvus-io/pymilvus/blob/d7a402b8cfdd24d3e5626da817de828ca14d8acb/pymilvus/client/grpc_handler.py#L11'>pymilvus/client/grpc_handler.py:11:26:</a> PLC2701 Private name import `_cython` from external module `grpc`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/dtypes.py#L10'>asv_bench/benchmarks/dtypes.py:10:27:</a> PLC2701 Private name import `_testing` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/indexing_engines.py#L13'>asv_bench/benchmarks/indexing_engines.py:13:35:</a> PLC2701 Private name import `_libs` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/io/parsers.py#L5'>asv_bench/benchmarks/io/parsers.py:5:9:</a> PLC2701 Private name import `_libs` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/io/parsers.py#L6'>asv_bench/benchmarks/io/parsers.py:6:9:</a> PLC2701 Private name import `_libs` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/libs.py#L11'>asv_bench/benchmarks/libs.py:11:5:</a> PLC2701 Private name import `_libs` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/libs.py#L12'>asv_bench/benchmarks/libs.py:12:5:</a> PLC2701 Private name import `_libs` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/libs.py#L13'>asv_bench/benchmarks/libs.py:13:5:</a> PLC2701 Private name import `_libs` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/plotting.py#L25'>asv_bench/benchmarks/plotting.py:25:35:</a> PLC2701 Private name import `_core` from external module `pandas.plotting`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/tslibs/fields.py#L4'>asv_bench/benchmarks/tslibs/fields.py:4:5:</a> PLC2701 Private name import `_libs` from external module `pandas`
+ <a href='https://github.com/pandas-dev/pandas/blob/7cd8ae5bdc069dcdaeb892418c932a7124a87dcf/asv_bench/benchmarks/tslibs/fields.py#L5'>asv_bench/benchmarks/tslibs/fields.py:5:5:</a> PLC2701 Private name import `_libs` from external module `pandas`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/bin/update_pythons.py#L20'>bin/update_pythons.py:20:34:</a> PLC2701 Private name import `_compat` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/bin/update_virtualenv.py#L18'>bin/update_virtualenv.py:18:34:</a> PLC2701 Private name import `_compat` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/build_ids_test.py#L5'>unit_test/build_ids_test.py:5:34:</a> PLC2701 Private name import `_compat` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/main_tests/main_options_test.py#L10'>unit_test/main_tests/main_options_test.py:10:34:</a> PLC2701 Private name import `_compat` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/main_tests/main_options_test.py#L12'>unit_test/main_tests/main_options_test.py:12:48:</a> PLC2701 Private name import `_get_pinned_container_images` from external module `cibuildwheel.options`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/main_tests/main_options_test.py#L9'>unit_test/main_tests/main_options_test.py:9:35:</a> PLC2701 Private name import `__main__` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/main_tests/main_platform_test.py#L7'>unit_test/main_tests/main_platform_test.py:7:35:</a> PLC2701 Private name import `__main__` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/main_tests/main_requires_python_test.py#L9'>unit_test/main_tests/main_requires_python_test.py:9:35:</a> PLC2701 Private name import `__main__` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/option_prepare_test.py#L14'>unit_test/option_prepare_test.py:14:35:</a> PLC2701 Private name import `__main__` from external module `cibuildwheel`
+ <a href='https://github.com/pypa/cibuildwheel/blob/a6da0692cef215eaccf5a81a2212fbd963de829d/unit_test/options_test.py#L10'>unit_test/options_test.py:10:35:</a> PLC2701 Private name import `__main__` from external module `cibuildwheel`
... 4 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2701 | 501 | 501 | 0 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-01-16 19:00_

---

_Label `rule` added by @charliermarsh on 2024-01-16 19:03_

---

_Label `preview` added by @charliermarsh on 2024-01-16 19:03_

---

_Comment by @charliermarsh on 2024-01-16 19:03_

Thank you and sorry that I broke this.

---

_Merged by @charliermarsh on 2024-01-16 19:03_

---

_Closed by @charliermarsh on 2024-01-16 19:03_

---

_Branch deleted on 2024-01-16 21:02_

---

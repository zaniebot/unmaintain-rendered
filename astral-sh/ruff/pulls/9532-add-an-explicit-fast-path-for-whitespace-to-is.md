```yaml
number: 9532
title: "Add an explicit fast path for whitespace to `is_identifier_continuation`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - parser
assignees: []
merged: true
base: main
head: lexer-explicit-id-false-brnaches
created_at: 2024-01-15T15:35:34Z
updated_at: 2024-01-16T08:37:22Z
url: https://github.com/astral-sh/ruff/pull/9532
synced_at: 2026-01-12T15:55:29Z
```

# Add an explicit fast path for whitespace to `is_identifier_continuation`

---

_@MichaReiser_

## Summary

Avoid calling into `is_xid_continue` for ascii characters (all ascii characters not handled by the match are implicitly `false`)

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2024-01-15 15:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/lexer-explicit-id-false-brnaches)

### Merging #9532 will **improve performances by 4.66%**

<sub>Comparing <code>lexer-explicit-id-false-brnaches</code> (cb7488b) with <code>main</code> (896cecd)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `lexer-explicit-id-false-brnaches` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.66% |


---

_Comment by @github-actions[bot] on 2024-01-15 15:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+473 -1002 violations, +0 -0 fixes in 12 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+6 -0 violations, +0 -0 fixes)</summary>
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
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/docs/extensions/attributetable.py#L13'>docs/extensions/attributetable.py:13:27:</a> PLC2701 Private name import `_` from external module `sphinx.locale`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+98 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/api/auth/backend/kerberos_auth.py#L55'>airflow/api/auth/backend/kerberos_auth.py:55:51:</a> PLC2701 Private name import `_request_ctx_stack` from external module `flask`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/io/path.py#L29'>airflow/io/path.py:29:52:</a> PLC2701 Private name import `_CloudAccessor` from external module `upath.implementations.cloud`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/metrics/otel_logger.py#L30'>airflow/metrics/otel_logger.py:30:56:</a> PLC2701 Private name import `_internal` from external module `opentelemetry.sdk.metrics`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/metrics/otel_logger.py#L30'>airflow/metrics/otel_logger.py:30:79:</a> PLC2701 Private name import `_internal` from external module `opentelemetry.sdk.metrics`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/cloud/hooks/gcs.py#L858'>airflow/providers/google/cloud/hooks/gcs.py:858:49:</a> PLC2701 Private name import `_blobs_page_start` from external module `google.cloud.storage.bucket`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/cloud/hooks/gcs.py#L858'>airflow/providers/google/cloud/hooks/gcs.py:858:68:</a> PLC2701 Private name import `_item_to_blob` from external module `google.cloud.storage.bucket`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/hooks/base_google.py#L42'>airflow/providers/google/common/hooks/base_google.py:42:35:</a> PLC2701 Private name import `_http_client` from external module `google.auth.transport`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/utils/id_token_credentials.py#L149'>airflow/providers/google/common/utils/id_token_credentials.py:149:29:</a> PLC2701 Private name import `_cloud_sdk` from external module `google.auth`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/utils/id_token_credentials.py#L175'>airflow/providers/google/common/utils/id_token_credentials.py:175:48:</a> PLC2701 Private name import `_metadata` from external module `google.auth.compute_engine`
+ <a href='https://github.com/apache/airflow/blob/81be6ac6c091c9c922c4060b12fc92e2d7ad424d/airflow/providers/google/common/utils/id_token_credentials.py#L178'>airflow/providers/google/common/utils/id_token_credentials.py:178:39:</a> PLC2701 Private name import `_http_client` from external module `google.auth.transport`
... 88 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+180 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/schema/make_schema.py#L12'>schema/make_schema.py:12:32:</a> PLC2701 Private name import `_SAM_CLI_COMMAND_PACKAGES` from external module `samcli.cli.command`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/integration/package/test_package_command_image.py#L13'>tests/integration/package/test_package_command_image.py:13:45:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/integration/sync/test_sync_adl.py#L5'>tests/integration/sync/test_sync_adl.py:5:67:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py#L7'>tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py:7:68:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py#L7'>tests/unit/commands/_utils/custom_options/test_hook_package_id_option.py:7:84:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_option_nargs.py#L4'>tests/unit/commands/_utils/custom_options/test_option_nargs.py:4:64:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/custom_options/test_replace_help_summary_option.py#L4'>tests/unit/commands/_utils/custom_options/test_replace_help_summary_option.py:4:71:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_cdk_support_decorators.py#L4'>tests/unit/commands/_utils/test_cdk_support_decorators.py:4:59:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_click_mutex.py#L3'>tests/unit/commands/_utils/test_click_mutex.py:3:48:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
+ <a href='https://github.com/aws/aws-sam-cli/blob/e8f0d71e2304bdb20c79ee90ee1a9a03fbba2091/tests/unit/commands/_utils/test_command_exception_handler.py#L7'>tests/unit/commands/_utils/test_command_exception_handler.py:7:5:</a> PLC2701 Private name import `_utils` from external module `samcli.commands`
... 170 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+94 -1002 violations, +0 -0 fixes)</summary>
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
... 89 additional changes omitted for rule PLC2701
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L10'>tests/unit/bokeh/plotting/test__decorators.py:10:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L11'>tests/unit/bokeh/plotting/test__decorators.py:11:35:</a> E261 [*] Insert at least two spaces before an inline comment
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:14:</a> E203 [*] Whitespace before ';'
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:15:</a> E702 Multiple statements on one line (semicolon)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:17:</a> B018 Found useless expression. Either assign it to a variable or remove it.
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L13'>tests/unit/bokeh/plotting/test__decorators.py:13:1:</a> I001 Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L15'>tests/unit/bokeh/plotting/test__decorators.py:15:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L17'>tests/unit/bokeh/plotting/test__decorators.py:17:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L1'>tests/unit/bokeh/plotting/test__decorators.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L1'>tests/unit/bokeh/plotting/test__decorators.py:1:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L1'>tests/unit/bokeh/plotting/test__decorators.py:1:1:</a> INP001 File `tests/unit/bokeh/plotting/test__decorators.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L20'>tests/unit/bokeh/plotting/test__decorators.py:20:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L23'>tests/unit/bokeh/plotting/test__decorators.py:23:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L24'>tests/unit/bokeh/plotting/test__decorators.py:24:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L25'>tests/unit/bokeh/plotting/test__decorators.py:25:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L28'>tests/unit/bokeh/plotting/test__decorators.py:28:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L28'>tests/unit/bokeh/plotting/test__decorators.py:28:41:</a> E261 [*] Insert at least two spaces before an inline comment
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L30'>tests/unit/bokeh/plotting/test__decorators.py:30:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L32'>tests/unit/bokeh/plotting/test__decorators.py:32:1:</a> E265 [*] Block comment should start with `# `
... 99 additional changes omitted for rule E265
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L43'>tests/unit/bokeh/plotting/test__decorators.py:43:3:</a> FIX002 Line contains TODO, consider resolving the issue
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L43'>tests/unit/bokeh/plotting/test__decorators.py:43:3:</a> TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L43'>tests/unit/bokeh/plotting/test__decorators.py:43:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L46'>tests/unit/bokeh/plotting/test__decorators.py:46:25:</a> C408 Unnecessary `dict` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L56'>tests/unit/bokeh/plotting/test__decorators.py:56:26:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L56'>tests/unit/bokeh/plotting/test__decorators.py:56:26:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L57'>tests/unit/bokeh/plotting/test__decorators.py:57:39:</a> ANN001 Missing type annotation for function argument `arg`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L57'>tests/unit/bokeh/plotting/test__decorators.py:57:44:</a> ANN001 Missing type annotation for function argument `values`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L57'>tests/unit/bokeh/plotting/test__decorators.py:57:5:</a> D103 Missing docstring in public function
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L59'>tests/unit/bokeh/plotting/test__decorators.py:59:25:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L60'>tests/unit/bokeh/plotting/test__decorators.py:60:17:</a> ANN202 Missing return type annotation for private function `foo`
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/plotting/test__decorators.py#L60'>tests/unit/bokeh/plotting/test__decorators.py:60:23:</a> ANN003 Missing type annotation for `**kw`
... 1059 additional changes omitted for project
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
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/backends/app/backend_info_app.py#L14'>docs/backends/app/backend_info_app.py:14:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py#L6'>docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py:6:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance-part2/datafusion_ibis.py#L4'>docs/posts/pydata-performance-part2/datafusion_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance-part2/duckdb_ibis.py#L4'>docs/posts/pydata-performance-part2/duckdb_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance-part2/polars_ibis.py#L4'>docs/posts/pydata-performance-part2/polars_ibis.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step0.py#L4'>docs/posts/pydata-performance/step0.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step1.py#L4'>docs/posts/pydata-performance/step1.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step2.py#L4'>docs/posts/pydata-performance/step2.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/docs/posts/pydata-performance/step3.py#L4'>docs/posts/pydata-performance/step3.py:4:18:</a> PLC2701 Private name import `_` from external module `ibis`
+ <a href='https://github.com/ibis-project/ibis/blob/48e89364c08a140cfca630eba72f8c17129c82ae/ibis/backends/conftest.py#L11'>ibis/backends/conftest.py:11:8:</a> PLC2701 Private name import `_pytest`
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (45 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2701 | 473 | 473 | 0 | 0 | 0 |
| S101 | 230 | 0 | 230 | 0 | 0 |
| Q000 | 150 | 0 | 150 | 0 | 0 |
| E265 | 104 | 0 | 104 | 0 | 0 |
| D102 | 63 | 0 | 63 | 0 | 0 |
| ANN101 | 60 | 0 | 60 | 0 | 0 |
| PLR6301 | 60 | 0 | 60 | 0 | 0 |
| PLR2004 | 47 | 0 | 47 | 0 | 0 |
| SLF001 | 38 | 0 | 38 | 0 | 0 |
| E402 | 23 | 0 | 23 | 0 | 0 |
| C408 | 22 | 0 | 22 | 0 | 0 |
| E275 | 18 | 0 | 18 | 0 | 0 |
| PT011 | 17 | 0 | 17 | 0 | 0 |
| E231 | 16 | 0 | 16 | 0 | 0 |
| E261 | 15 | 0 | 15 | 0 | 0 |
| ANN001 | 15 | 0 | 15 | 0 | 0 |
| ERA001 | 14 | 0 | 14 | 0 | 0 |
| D101 | 12 | 0 | 12 | 0 | 0 |
| N801 | 12 | 0 | 12 | 0 | 0 |
| E203 | 8 | 0 | 8 | 0 | 0 |
| E702 | 7 | 0 | 7 | 0 | 0 |
| B018 | 7 | 0 | 7 | 0 | 0 |
| I001 | 7 | 0 | 7 | 0 | 0 |
| D100 | 7 | 0 | 7 | 0 | 0 |
| INP001 | 7 | 0 | 7 | 0 | 0 |
| D103 | 6 | 0 | 6 | 0 | 0 |
| A002 | 6 | 0 | 6 | 0 | 0 |
| E202 | 4 | 0 | 4 | 0 | 0 |
| E201 | 4 | 0 | 4 | 0 | 0 |
| N802 | 3 | 0 | 3 | 0 | 0 |
| FIX002 | 2 | 0 | 2 | 0 | 0 |
| TD002 | 2 | 0 | 2 | 0 | 0 |
| TD003 | 2 | 0 | 2 | 0 | 0 |
| PT018 | 2 | 0 | 2 | 0 | 0 |
| DTZ001 | 2 | 0 | 2 | 0 | 0 |
| PT006 | 1 | 0 | 1 | 0 | 0 |
| ANN202 | 1 | 0 | 1 | 0 | 0 |
| ANN003 | 1 | 0 | 1 | 0 | 0 |
| ARG001 | 1 | 0 | 1 | 0 | 0 |
| PT012 | 1 | 0 | 1 | 0 | 0 |
| ANN201 | 1 | 0 | 1 | 0 | 0 |
| PLC0415 | 1 | 0 | 1 | 0 | 0 |
| E225 | 1 | 0 | 1 | 0 | 0 |
| E241 | 1 | 0 | 1 | 0 | 0 |
| PLR0402 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2024-01-15 16:01_

Where is the explicit fast path? :D

---

_Label `performance` added by @MichaReiser on 2024-01-15 16:35_

---

_Label `parser` added by @MichaReiser on 2024-01-15 16:35_

---

_Comment by @MichaReiser on 2024-01-15 16:35_

> Where is the explicit fast path? :D

The fast path is to avoid calling into `is_xid_continue` for all ascii characters that don't fall into the `a-zA-Z0-9\_` pattern

---

_Marked ready for review by @MichaReiser on 2024-01-15 16:35_

---

_Comment by @MichaReiser on 2024-01-15 16:36_

It seems to be a low 1% improvement. The parser benchmark is a flaky result

---

_Comment by @charliermarsh on 2024-01-15 16:38_

Oh I see, it took me a second to notice the effect.

---

_@BurntSushi reviewed on 2024-01-15 16:39_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/lexer.rs`:1503 on 2024-01-15 16:39_

The fast path here is definitely subtle. I might add a short comment explaining the `is_ascii()` check here. I think it would be pretty easy for someone to come along and remove the check because it is, logically speaking, redundant. Maybe something like:

```rust
// Arrange things such that ASCII codepoints never
// result in the slower `is_xid_continue` getting called.
```

---

_@charliermarsh approved on 2024-01-15 17:05_

(Cool with it in general though, easy win.)

---

_Comment by @charliermarsh on 2024-01-15 17:05_

I do wonder if marking the other path as cold and/or inline-never is good .

---

_Comment by @MichaReiser on 2024-01-16 08:15_

> I do wonder if marking the other path as cold and/or inline-never is good .

Quiet possibly. I avoided adding that change here to better understand the root cause of the improvement. 

---

_Merged by @MichaReiser on 2024-01-16 08:23_

---

_Closed by @MichaReiser on 2024-01-16 08:23_

---

_Branch deleted on 2024-01-16 08:23_

---

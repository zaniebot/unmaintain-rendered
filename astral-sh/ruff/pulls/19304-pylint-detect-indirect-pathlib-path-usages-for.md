```yaml
number: 19304
title: "[`pylint`] Detect indirect `pathlib.Path` usages for `unspecified-encoding` (PLW1514)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-19294
created_at: 2025-07-13T06:22:31Z
updated_at: 2025-07-16T17:28:28Z
url: https://github.com/astral-sh/ruff/pull/19304
synced_at: 2026-01-10T17:58:13Z
```

# [`pylint`] Detect indirect `pathlib.Path` usages for `unspecified-encoding` (PLW1514)

---

_Pull request opened by @danparizher on 2025-07-13 06:22_

## Summary

Fixes #19294

---

_Comment by @github-actions[bot] on 2025-07-13 06:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+206 -0 violations, +0 -0 fixes in 12 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/92af72fdfbb9e657de5a5617d3ab98e7ed9a95c4/.github/scripts/citation_updater.py#L34'>.github/scripts/citation_updater.py:34:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/92af72fdfbb9e657de5a5617d3ab98e7ed9a95c4/.github/scripts/citation_updater.py#L41'>.github/scripts/citation_updater.py:41:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/92af72fdfbb9e657de5a5617d3ab98e7ed9a95c4/.github/scripts/citation_updater.py#L45'>.github/scripts/citation_updater.py:45:25:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/92af72fdfbb9e657de5a5617d3ab98e7ed9a95c4/.github/scripts/citation_updater.py#L64'>.github/scripts/citation_updater.py:64:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/92af72fdfbb9e657de5a5617d3ab98e7ed9a95c4/noxfile.py#L119'>noxfile.py:119:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/92af72fdfbb9e657de5a5617d3ab98e7ed9a95c4/src/plasmapy/utils/calculator/main_interface.py#L163'>src/plasmapy/utils/calculator/main_interface.py:163:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/92af72fdfbb9e657de5a5617d3ab98e7ed9a95c4/tests/utils/data/test_downloader.py#L188'>tests/utils/data/test_downloader.py:188:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+89 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/airflow-core/src/airflow/dag_processing/bundles/base.py#L390'>airflow-core/src/airflow/dag_processing/bundles/base.py:390:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/airflow-core/src/airflow/utils/code_utils.py#L62'>airflow-core/src/airflow/utils/code_utils.py:62:18:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/airflow-core/tests/unit/dags/test_parsing_context.py#L44'>airflow-core/tests/unit/dags/test_parsing_context.py:44:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/assign_cherry_picked_prs_with_milestone.py#L382'>dev/assign_cherry_picked_prs_with_milestone.py:382:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2904'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2904:20:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2922'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2922:26:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2934'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2934:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L412'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:412:30:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L427'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:427:20:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L431'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:431:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L716'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:716:12:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L729'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:729:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L1284'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:1284:25:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L1290'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:1290:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L676'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:676:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/utils/add_back_references.py#L52'>dev/breeze/src/airflow_breeze/utils/add_back_references.py:52:10:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/utils/add_back_references.py#L76'>dev/breeze/src/airflow_breeze/utils/add_back_references.py:76:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/utils/cache.py#L66'>dev/breeze/src/airflow_breeze/utils/cache.py:66:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/utils/constraints_version_check.py#L299'>dev/breeze/src/airflow_breeze/utils/constraints_version_check.py:299:13:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/utils/constraints_version_check.py#L313'>dev/breeze/src/airflow_breeze/utils/constraints_version_check.py:313:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/apache/airflow/blob/d169a6ba6727b56721e4dbc6e957c404fbc1245e/dev/breeze/src/airflow_breeze/utils/constraints_version_check.py#L333'>dev/breeze/src/airflow_breeze/utils/constraints_version_check.py:333:28:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
... 68 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+53 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/samcli/lib/config/file_manager.py#L105'>samcli/lib/config/file_manager.py:105:19:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/samcli/lib/config/file_manager.py#L136'>samcli/lib/config/file_manager.py:136:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/samcli/lib/config/file_manager.py#L191'>samcli/lib/config/file_manager.py:191:50:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/samcli/lib/config/file_manager.py#L289'>samcli/lib/config/file_manager.py:289:36:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/samcli/lib/config/file_manager.py#L312'>samcli/lib/config/file_manager.py:312:14:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/samcli/lib/utils/configuration.py#L12'>samcli/lib/utils/configuration.py:12:21:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/tests/functional/commands/cli/test_global_config.py#L106'>tests/functional/commands/cli/test_global_config.py:106:32:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/tests/functional/commands/cli/test_global_config.py#L119'>tests/functional/commands/cli/test_global_config.py:119:32:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/tests/functional/commands/cli/test_global_config.py#L132'>tests/functional/commands/cli/test_global_config.py:132:32:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/tests/functional/commands/cli/test_global_config.py#L186'>tests/functional/commands/cli/test_global_config.py:186:32:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/tests/functional/commands/cli/test_global_config.py#L200'>tests/functional/commands/cli/test_global_config.py:200:32:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/aws/aws-sam-cli/blob/51a21413b8baacbd0f44a81e544f98af7cb6ed8e/tests/functional/commands/cli/test_global_config.py#L216'>tests/functional/commands/cli/test_global_config.py:216:32:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
... 41 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py#L24'>securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py:24:16:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py#L38'>securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py:38:17:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py#L47'>securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py:47:9:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py#L51'>securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py:51:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py#L58'>securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py:58:16:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py#L73'>securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py:73:13:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py#L78'>securedrop/debian/app-code/usr/bin/securedrop-set-redis-auth.py:78:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/server_os.py#L41'>securedrop/server_os.py:41:31:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/tests/test_i18n.py#L70'>securedrop/tests/test_i18n.py:70:18:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/tests/test_i18n.py#L82'>securedrop/tests/test_i18n.py:82:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/0f4fd454c49abfb04a82732c23fe9f2675bbd91b/ci/make_geography_db.py#L64'>ci/make_geography_db.py:64:13:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/ibis-project/ibis/blob/0f4fd454c49abfb04a82732c23fe9f2675bbd91b/ci/release/verify_release.py#L46'>ci/release/verify_release.py:46:12:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/ibis-project/ibis/blob/0f4fd454c49abfb04a82732c23fe9f2675bbd91b/ibis/backends/snowflake/tests/conftest.py#L40'>ibis/backends/snowflake/tests/conftest.py:40:23:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/ibis-project/ibis/blob/0f4fd454c49abfb04a82732c23fe9f2675bbd91b/ibis/examples/gen_registry.py#L45'>ibis/examples/gen_registry.py:45:39:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/language_models/llms.py#L1432'>libs/core/langchain_core/language_models/llms.py:1432:18:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/language_models/llms.py#L1435'>libs/core/langchain_core/language_models/llms.py:1435:18:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/prompts/base.py#L384'>libs/core/langchain_core/prompts/base.py:384:18:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/prompts/base.py#L387'>libs/core/langchain_core/prompts/base.py:387:18:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/prompts/loading.py#L56'>libs/core/langchain_core/prompts/loading.py:56:24:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/prompts/loading.py#L70'>libs/core/langchain_core/prompts/loading.py:70:14:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/vectorstores/in_memory.py#L600'>libs/core/langchain_core/vectorstores/in_memory.py:600:14:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/b1c7de98f57bdd304699a05b79acc1953704fc72/libs/core/langchain_core/vectorstores/in_memory.py#L614'>libs/core/langchain_core/vectorstores/in_memory.py:614:14:</a> PLW1514 `pathlib.Path(...).open` in text mode without explicit `encoding` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/main.py#L1258'>src/latch_cli/main.py:1258:26:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/nextflow/config.py#L49'>src/latch_cli/nextflow/config.py:49:31:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/nextflow/parse_schema.py#L377'>src/latch_cli/nextflow/parse_schema.py:377:38:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/nextflow/parse_schema.py#L393'>src/latch_cli/nextflow/parse_schema.py:393:38:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/nextflow/workflow.py#L472'>src/latch_cli/nextflow/workflow.py:472:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/services/init/init.py#L138'>src/latch_cli/services/init/init.py:138:13:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/services/init/init.py#L144'>src/latch_cli/services/init/init.py:144:5:</a> PLW1514 `pathlib.Path(...).write_text` without explicit `encoding` argument
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/snakemake/config/parser.py#L47'>src/latch_cli/snakemake/config/parser.py:47:41:</a> PLW1514 `pathlib.Path(...).read_text` without explicit `encoding` argument
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW1514 | 206 | 206 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2025-07-14 09:14_

---

_Label `preview` added by @MichaReiser on 2025-07-14 09:14_

---

_Review requested from @ntBre by @MichaReiser on 2025-07-14 09:14_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:137 on 2025-07-14 18:25_

I think this would be a good use of our `typing` module:

https://github.com/astral-sh/ruff/blob/966fd6f57acafb4d476c406344e16e795ecd5df7/crates/ruff_python_semantic/src/analyze/typing.rs#L1089-L1093

It looks like we already have one for `Path`. I _think_ this will also allow us to cover the annotated function argument case from the issue, whereas I don't think this implementation would.

```py
def format_file(file: Path):
    with file.open() as f:
        contents = f.read()
```

It might be good to add this as a test case if it does work.

---

_@ntBre reviewed on 2025-07-14 18:27_

I think we can reuse some existing type inference code for this.

---

_Review requested from @ntBre by @danparizher on 2025-07-14 20:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:159 on 2025-07-16 16:01_

I think the `typing` check should handle all of these. At least deleting them didn't cause any tests to fail locally.


```suggestion
```

---

_@ntBre approved on 2025-07-16 16:11_

Looks good, just one suggestion to delete a redundant section of code.

---

_Comment by @ntBre on 2025-07-16 16:20_

I spot-checked the ecosystem changes, and this looks like a really handy change! We can use that to make sure my suggestion doesn't break anything too (currently +206 violations).

---

_Comment by @danparizher on 2025-07-16 16:53_

Looks good to go! 🙂

---

_Comment by @ntBre on 2025-07-16 16:57_

Excellent, thank you!

---

_Merged by @ntBre on 2025-07-16 16:57_

---

_Closed by @ntBre on 2025-07-16 16:57_

---

_Branch deleted on 2025-07-16 17:28_

---

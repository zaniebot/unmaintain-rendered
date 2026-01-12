```yaml
number: 6884
title: "Update PTH122 documentation to include `Path.stem` and `Path.parent`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pth122
created_at: 2023-08-25T22:03:02Z
updated_at: 2023-08-25T22:37:06Z
url: https://github.com/astral-sh/ruff/pull/6884
synced_at: 2026-01-12T02:45:38Z
```

# Update PTH122 documentation to include `Path.stem` and `Path.parent`

---

_Pull request opened by @charliermarsh on 2023-08-25 22:03_

Closes https://github.com/astral-sh/ruff/issues/6846.

---

_Label `documentation` added by @charliermarsh on 2023-08-25 22:03_

---

_Merged by @charliermarsh on 2023-08-25 22:11_

---

_Closed by @charliermarsh on 2023-08-25 22:11_

---

_Branch deleted on 2023-08-25 22:11_

---

_Comment by @github-actions[bot] on 2023-08-25 22:16_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+43, -43, 0 error(s))

<details><summary>airflow (+12, -12)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/cli/commands/internal_api_command.py#L173'>airflow/cli/commands/internal_api_command.py:173:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/cli/commands/internal_api_command.py#L173'>airflow/cli/commands/internal_api_command.py:173:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/cli/commands/webserver_command.py#L477'>airflow/cli/commands/webserver_command.py:477:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/cli/commands/webserver_command.py#L477'>airflow/cli/commands/webserver_command.py:477:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/dag_processing/manager.py#L1047'>airflow/dag_processing/manager.py:1047:21:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/dag_processing/manager.py#L1047'>airflow/dag_processing/manager.py:1047:21:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/dag_processing/manager.py#L855'>airflow/dag_processing/manager.py:855:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/dag_processing/manager.py#L855'>airflow/dag_processing/manager.py:855:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/models/dagbag.py#L328'>airflow/models/dagbag.py:328:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/models/dagbag.py#L328'>airflow/models/dagbag.py:328:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/models/dagbag.py#L382'>airflow/models/dagbag.py:382:33:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/models/dagbag.py#L382'>airflow/models/dagbag.py:382:33:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/plugins_manager.py#L257'>airflow/plugins_manager.py:257:30:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/plugins_manager.py#L257'>airflow/plugins_manager.py:257:30:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/providers/apache/hive/transfers/s3_to_hive.py#L159'>airflow/providers/apache/hive/transfers/s3_to_hive.py:159:23:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/providers/apache/hive/transfers/s3_to_hive.py#L159'>airflow/providers/apache/hive/transfers/s3_to_hive.py:159:23:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/utils/file.py#L316'>airflow/utils/file.py:316:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/airflow/utils/file.py#L316'>airflow/utils/file.py:316:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/tests/plugins/test_plugin_ignore.py#L120'>tests/plugins/test_plugin_ignore.py:120:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/tests/plugins/test_plugin_ignore.py#L120'>tests/plugins/test_plugin_ignore.py:120:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/tests/plugins/test_plugin_ignore.py#L93'>tests/plugins/test_plugin_ignore.py:93:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/tests/plugins/test_plugin_ignore.py#L93'>tests/plugins/test_plugin_ignore.py:93:27:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/tests/system/providers/amazon/aws/utils/__init__.py#L70'>tests/system/providers/amazon/aws/utils/__init__.py:70:12:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/apache/airflow/blob/c077d190609f931387c1fcd7b8cc34f12e2372b9/tests/system/providers/amazon/aws/utils/__init__.py#L70'>tests/system/providers/amazon/aws/utils/__init__.py:70:12:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
</pre>

</p>
</details>
<details><summary>bokeh (+14, -14)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/application/handlers/code.py#L169'>src/bokeh/application/handlers/code.py:169:22:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/application/handlers/code.py#L169'>src/bokeh/application/handlers/code.py:169:22:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/application/handlers/server_lifecycle.py#L128'>src/bokeh/application/handlers/server_lifecycle.py:128:26:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/application/handlers/server_lifecycle.py#L128'>src/bokeh/application/handlers/server_lifecycle.py:128:26:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/application/handlers/server_request_handler.py#L121'>src/bokeh/application/handlers/server_request_handler.py:121:26:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/application/handlers/server_request_handler.py#L121'>src/bokeh/application/handlers/server_request_handler.py:121:26:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/command/subcommands/file_output.py#L199'>src/bokeh/command/subcommands/file_output.py:199:33:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/command/subcommands/file_output.py#L199'>src/bokeh/command/subcommands/file_output.py:199:33:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/io/export.py#L419'>src/bokeh/io/export.py:419:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/io/export.py#L419'>src/bokeh/io/export.py:419:25:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/io/util.py#L83'>src/bokeh/io/util.py:83:15:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/io/util.py#L83'>src/bokeh/io/util.py:83:15:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L134'>src/bokeh/util/sampledata.py:134:33:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L134'>src/bokeh/util/sampledata.py:134:33:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L212'>src/bokeh/util/sampledata.py:212:22:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L212'>src/bokeh/util/sampledata.py:212:22:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L215'>src/bokeh/util/sampledata.py:215:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L215'>src/bokeh/util/sampledata.py:215:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L79'>src/bokeh/util/sampledata.py:79:22:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L79'>src/bokeh/util/sampledata.py:79:22:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L81'>src/bokeh/util/sampledata.py:81:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/src/bokeh/util/sampledata.py#L81'>src/bokeh/util/sampledata.py:81:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/tests/codebase/test_code_quality.py#L81'>tests/codebase/test_code_quality.py:81:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/tests/codebase/test_code_quality.py#L81'>tests/codebase/test_code_quality.py:81:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/tests/codebase/test_windows_reserved_filenames.py#L42'>tests/codebase/test_windows_reserved_filenames.py:42:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/tests/codebase/test_windows_reserved_filenames.py#L42'>tests/codebase/test_windows_reserved_filenames.py:42:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/tests/support/util/examples.py#L115'>tests/support/util/examples.py:115:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/bokeh/bokeh/blob/b93b1f75387d41fb39e19e9f77849caa010c7218/tests/support/util/examples.py#L115'>tests/support/util/examples.py:115:16:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
</pre>

</p>
</details>
<details><summary>zulip (+17, -17)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/tools/tests/test_zulint_custom_rules.py#L39'>tools/tests/test_zulint_custom_rules.py:39:24:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/tools/tests/test_zulint_custom_rules.py#L39'>tools/tests/test_zulint_custom_rules.py:39:24:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/emoji.py#L155'>zerver/lib/emoji.py:155:20:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/emoji.py#L155'>zerver/lib/emoji.py:155:20:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/integrations.py#L127'>zerver/lib/integrations.py:127:20:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/integrations.py#L127'>zerver/lib/integrations.py:127:20:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/integrations.py#L243'>zerver/lib/integrations.py:243:23:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/integrations.py#L243'>zerver/lib/integrations.py:243:23:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/sounds.py#L12'>zerver/lib/sounds.py:12:21:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/sounds.py#L12'>zerver/lib/sounds.py:12:21:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/storage.py#L28'>zerver/lib/storage.py:28:15:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/storage.py#L28'>zerver/lib/storage.py:28:15:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/local.py#L219'>zerver/lib/upload/local.py:219:54:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/local.py#L219'>zerver/lib/upload/local.py:219:54:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/local.py#L246'>zerver/lib/upload/local.py:246:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/local.py#L246'>zerver/lib/upload/local.py:246:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/s3.py#L438'>zerver/lib/upload/s3.py:438:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/s3.py#L438'>zerver/lib/upload/s3.py:438:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/s3.py#L476'>zerver/lib/upload/s3.py:476:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/lib/upload/s3.py#L476'>zerver/lib/upload/s3.py:476:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/migrations/0149_realm_emoji_drop_unique_constraint.py#L84'>zerver/migrations/0149_realm_emoji_drop_unique_constraint.py:84:20:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/migrations/0149_realm_emoji_drop_unique_constraint.py#L84'>zerver/migrations/0149_realm_emoji_drop_unique_constraint.py:84:20:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_bots.py#L1215'>zerver/tests/test_bots.py:1215:38:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_bots.py#L1215'>zerver/tests/test_bots.py:1215:38:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_bots.py#L271'>zerver/tests/test_bots.py:271:38:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_bots.py#L271'>zerver/tests/test_bots.py:271:38:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_transfer.py#L133'>zerver/tests/test_transfer.py:133:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_transfer.py#L133'>zerver/tests/test_transfer.py:133:50:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_upload_local.py#L207'>zerver/tests/test_upload_local.py:207:46:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_upload_local.py#L207'>zerver/tests/test_upload_local.py:207:46:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_upload_s3.py#L463'>zerver/tests/test_upload_s3.py:463:65:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_upload_s3.py#L463'>zerver/tests/test_upload_s3.py:463:65:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_urls.py#L47'>zerver/tests/test_urls.py:47:30:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`
+ <a href='https://github.com/zulip/zulip/blob/ada2991f1c3330c59347b1bdc5ec60b13a71dc24/zerver/tests/test_urls.py#L47'>zerver/tests/test_urls.py:47:30:</a> PTH122 `os.path.splitext()` should be replaced by `Path.suffix`, `Path.stem`, and `Path.parent`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH122 | 86 | 43 | 43 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.4±0.02ms     9.2 MB/sec    1.01      4.5±0.01ms     9.1 MB/sec
formatter/numpy/ctypeslib.py               1.00    885.9±2.49µs    18.8 MB/sec    1.00    885.2±2.26µs    18.8 MB/sec
formatter/numpy/globals.py                 1.01     83.6±0.44µs    35.3 MB/sec    1.00     83.1±0.50µs    35.5 MB/sec
formatter/pydantic/types.py                1.01  1683.3±12.64µs    15.2 MB/sec    1.00   1667.6±6.90µs    15.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.07ms     4.0 MB/sec    1.01     10.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    382.0±1.17µs     7.7 MB/sec    1.00    381.5±0.52µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.04ms     4.9 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.01ms     7.6 MB/sec    1.02      5.4±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1176.0±3.62µs    14.2 MB/sec    1.01   1192.7±5.43µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    137.1±2.91µs    21.5 MB/sec    1.01    138.9±2.21µs    21.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.03ms    10.6 MB/sec    1.01      2.4±0.01ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.2±0.05ms     7.9 MB/sec    1.01      5.2±0.07ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00    966.9±9.04µs    17.2 MB/sec    1.01   980.6±25.12µs    17.0 MB/sec
formatter/numpy/globals.py                 1.00     89.6±1.02µs    32.9 MB/sec    1.00     89.5±0.80µs    33.0 MB/sec
formatter/pydantic/types.py                1.00  1913.9±16.04µs    13.3 MB/sec    1.01  1924.9±47.82µs    13.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.4±0.16ms     3.3 MB/sec    1.02     12.6±0.15ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    353.6±4.72µs     8.3 MB/sec    1.02    362.3±6.88µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.07ms     3.9 MB/sec    1.01      6.5±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.05ms     6.0 MB/sec    1.01      6.9±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1415.7±11.29µs    11.8 MB/sec    1.02  1441.6±17.80µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    146.2±2.12µs    20.2 MB/sec    1.01    147.6±2.22µs    20.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.3 MB/sec    1.01      3.1±0.03ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

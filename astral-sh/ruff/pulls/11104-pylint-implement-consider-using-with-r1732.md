```yaml
number: 11104
title: "[pylint] implement `consider-using-with` (R1732)"
type: pull_request
state: closed
author: TomerBin
labels: []
assignees: []
base: main
head: feature/R1732-consider-using-with
created_at: 2024-04-23T12:04:33Z
updated_at: 2024-04-23T17:50:00Z
url: https://github.com/astral-sh/ruff/pull/11104
synced_at: 2026-01-10T22:37:01Z
```

# [pylint] implement `consider-using-with` (R1732)

---

_Pull request opened by @TomerBin on 2024-04-23 12:04_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement pylint [consider-using-with](https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/consider-using-with.html) rule. 
This is my first contribution to Ruff and my initial exploration in Rust programming. I welcome any feedback or guidance to improve my contribution.

## Test Plan
<!-- How was it tested? -->
Cargo test

---

_Comment by @github-actions[bot] on 2024-04-23 12:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+87 -0 violations, +0 -0 fixes in 9 projects; 35 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/d9bbcd295832eab0a2da6bf81e0ae00b9777308a/aiven/client/cli.py#L4776'>aiven/client/cli.py:4776:20:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/aiven/aiven-client/blob/d9bbcd295832eab0a2da6bf81e0ae00b9777308a/aiven/client/cli.py#L4782'>aiven/client/cli.py:4782:24:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+32 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/airflow/providers/amazon/aws/hooks/s3.py#L1411'>airflow/providers/amazon/aws/hooks/s3.py:1411:20:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/airflow/providers/ftp/hooks/ftp.py#L172'>airflow/providers/ftp/hooks/ftp.py:172:33:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/airflow/providers/ftp/hooks/ftp.py#L205'>airflow/providers/ftp/hooks/ftp.py:205:28:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/airflow/providers/weaviate/hooks/weaviate.py#L276'>airflow/providers/weaviate/hooks/weaviate.py:276:48:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/airflow/utils/file.py#L195'>airflow/utils/file.py:195:16:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/airflow/utils/log/file_processor_handler.py#L151'>airflow/utils/log/file_processor_handler.py:151:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/airflow/utils/log/file_task_handler.py#L537'>airflow/utils/log/file_task_handler.py:537:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/docs/exts/operators_and_hooks_ref.py#L181'>docs/exts/operators_and_hooks_ref.py:181:25:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/scripts/ci/pre_commit/check_deferrable_default.py#L66'>scripts/ci/pre_commit/check_deferrable_default.py:66:25:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/scripts/ci/pre_commit/validate_operators_init.py#L227'>scripts/ci/pre_commit/validate_operators_init.py:227:26:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/tests/core/test_logging_config.py#L146'>tests/core/test_logging_config.py:146:17:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/tests/core/test_logging_config.py#L148'>tests/core/test_logging_config.py:148:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/tests/providers/apache/beam/operators/test_beam.py#L580'>tests/providers/apache/beam/operators/test_beam.py:580:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/tests/providers/apache/beam/operators/test_beam.py#L746'>tests/providers/apache/beam/operators/test_beam.py:746:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/tests/sensors/test_filesystem.py#L107'>tests/sensors/test_filesystem.py:107:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/apache/airflow/blob/4ae85d754e9f8a65d461e86eb6111d3b9974a065/tests/sensors/test_filesystem.py#L166'>tests/sensors/test_filesystem.py:166:17:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/48bc6e279dac07532879332744dbe3e02596e57c/samcli/commands/local/cli_common/invoke_context.py#L519'>samcli/commands/local/cli_common/invoke_context.py:519:16:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/aws/aws-sam-cli/blob/48bc6e279dac07532879332744dbe3e02596e57c/samcli/lib/build/build_graph.py#L378'>samcli/lib/build/build_graph.py:378:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/aws/aws-sam-cli/blob/48bc6e279dac07532879332744dbe3e02596e57c/samcli/lib/build/build_graph.py#L470'>samcli/lib/build/build_graph.py:470:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/aws/aws-sam-cli/blob/48bc6e279dac07532879332744dbe3e02596e57c/tests/integration/pipeline/test_init_command.py#L107'>tests/integration/pipeline/test_init_command.py:107:30:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L69'>docs/bokeh/source/conf.py:69:14:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L32'>examples/server/app/spectrogram/main.py:32:17:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L157'>release/build.py:157:29:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L31'>release/remote.py:31:16:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L33'>release/remote.py:33:16:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L318'>scripts/milestone.py:318:11:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L38'>scripts/sri.py:38:21:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L801'>src/bokeh/settings.py:801:47:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L820'>src/bokeh/settings.py:820:34:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_gallery.py#L88'>src/bokeh/sphinxext/bokeh_gallery.py:88:34:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/admin/tests/test_securedrop-admin.py#L699'>admin/tests/test_securedrop-admin.py:699:28:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/journalist_gui/journalist_gui/SecureDropUpdater.py#L61'>journalist_gui/journalist_gui/SecureDropUpdater.py:61:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/molecule/testinfra/app/test_app_network.py#L40'>molecule/testinfra/app/test_app_network.py:40:31:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/molecule/testinfra/mon/test_mon_network.py#L40'>molecule/testinfra/mon/test_mon_network.py:40:31:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/management/submissions.py#L182'>securedrop/management/submissions.py:182:9:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_trust.py#L64'>securedrop/pretty_bad_protocol/_trust.py:64:11:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_trust.py#L82'>securedrop/pretty_bad_protocol/_trust.py:82:15:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/secure_tempfile.py#L54'>securedrop/secure_tempfile.py:54:21:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/source_app/utils.py#L96'>securedrop/source_app/utils.py:96:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/conftest.py#L328'>securedrop/tests/conftest.py:328:23:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/23dd1f12aea8bfd503ea86ce1850de817cf0fe43/pandas/io/common.py#L1141'>pandas/io/common.py:1141:18:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/pandas-dev/pandas/blob/23dd1f12aea8bfd503ea86ce1850de817cf0fe43/pandas/io/common.py#L884'>pandas/io/common.py:884:22:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/pandas-dev/pandas/blob/23dd1f12aea8bfd503ea86ce1850de817cf0fe43/pandas/io/common.py#L893'>pandas/io/common.py:893:22:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/pandas-dev/pandas/blob/23dd1f12aea8bfd503ea86ce1850de817cf0fe43/pandas/tests/io/test_gcs.py#L215'>pandas/tests/io/test_gcs.py:215:20:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/pandas-dev/pandas/blob/23dd1f12aea8bfd503ea86ce1850de817cf0fe43/pandas/tests/io/test_pickle.py#L432'>pandas/tests/io/test_pickle.py:432:25:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/d9ba658aefd7be7ebf0304526a37bb6d61411d2f/tools/profiling/trace.py#L28'>tools/profiling/trace.py:28:29:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/scripts/lib/zulip_tools.py#L478'>scripts/lib/zulip_tools.py:478:20:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/tools/lib/test_server.py#L65'>tools/lib/test_server.py:65:43:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/tools/lib/test_server.py#L68'>tools/lib/test_server.py:68:43:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/zerver/lib/test_helpers.py#L231'>zerver/lib/test_helpers.py:231:12:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/zerver/migrations/0501_delete_dangling_usermessages.py#L151'>zerver/migrations/0501_delete_dangling_usermessages.py:151:17:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/zerver/views/upload.py#L123'>zerver/views/upload.py:123:13:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/zerver/views/upload.py#L207'>zerver/views/upload.py:207:29:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/zerver/views/upload.py#L298'>zerver/views/upload.py:298:51:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/3735a273cae1ae94dcaad45241581a1e81e9f7e1/bin/utils/create_module.py#L58'>bin/utils/create_module.py:58:5:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/indico/indico/blob/3735a273cae1ae94dcaad45241581a1e81e9f7e1/indico/core/storage/backend.py#L231'>indico/core/storage/backend.py:231:20:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
+ <a href='https://github.com/indico/indico/blob/3735a273cae1ae94dcaad45241581a1e81e9f7e1/indico/legacy/pdfinterface/latex.py#L208'>indico/legacy/pdfinterface/latex.py:208:20:</a> PLR1732 Consider using `with open(...) as ...:` instead of `open(...)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1732 | 87 | 87 | 0 | 0 | 0 |

</p>
</details>




---

_Closed by @TomerBin on 2024-04-23 17:49_

---

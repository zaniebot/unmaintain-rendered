```yaml
number: 9462
title: "Restrict `builtin-attribute-shadowing` to actual shadowed references"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/class-builtin
created_at: 2024-01-11T02:33:17Z
updated_at: 2024-01-11T17:59:41Z
url: https://github.com/astral-sh/ruff/pull/9462
synced_at: 2026-01-10T22:57:09Z
```

# Restrict `builtin-attribute-shadowing` to actual shadowed references

---

_Pull request opened by @charliermarsh on 2024-01-11 02:33_

## Summary

This PR attempts to improve `builtin-attribute-shadowing` (`A003`), a rule which has been repeatedly criticized, but _does_ have value (just not in the current form).

Historically, this rule would flag cases like:

```python
class Class:
    id: int
```

This led to an increasing number of exceptions and special-cases to the rule over time to try and improve it's specificity (e.g., ignore `TypedDict`, ignore `@override`).

The crux of the issue is that given the above, referencing `id` will never resolve to `Class.id`, so the shadowing is actually fine. There's one exception, however:

```python
class Class:
    id: int

    def do_thing() -> id:
        pass
```

Here, `id` actually resolves to the `id` attribute on the class, not the `id` builtin.

So this PR completely reworks the rule around this _much_ more targeted case, which will almost always be a mistake: when you reference a class member from within the class, and that member shadows a builtin.

Closes https://github.com/astral-sh/ruff/issues/6524.

Closes https://github.com/astral-sh/ruff/issues/7806.


---

_Label `rule` added by @charliermarsh on 2024-01-11 02:33_

---

_Marked ready for review by @charliermarsh on 2024-01-11 03:02_

---

_Comment by @github-actions[bot] on 2024-01-11 03:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -225 violations, +0 -0 fixes in 6 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -64 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/dataset_schema.py#L123'>airflow/api_connexion/schemas/dataset_schema.py:123:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/dataset_schema.py#L69'>airflow/api_connexion/schemas/dataset_schema.py:69:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/event_log_schema.py#L35'>airflow/api_connexion/schemas/event_log_schema.py:35:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/job_schema.py#L33'>airflow/api_connexion/schemas/job_schema.py:33:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/trigger_schema.py#L33'>airflow/api_connexion/schemas/trigger_schema.py:33:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/auth/managers/models/resource_details.py#L42'>airflow/auth/managers/models/resource_details.py:42:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/cli/cli_config.py#L1011'>airflow/cli/cli_config.py:1011:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/cli/cli_config.py#L1023'>airflow/cli/cli_config.py:1023:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/configuration.py#L1296'>airflow/configuration.py:1296:9:</a> A003 Class attribute `set` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/jobs/job.py#L69'>airflow/jobs/job.py:69:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/models/connection.py#L91'>airflow/models/connection.py:91:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/models/dagpickle.py#L44'>airflow/models/dagpickle.py:44:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/models/dagrun.py#L120'>airflow/models/dagrun.py:120:5:</a> A003 Class attribute `id` is shadowing a Python builtin
... 51 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -55 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/examples/advanced/extensions/widget.py#L44'>examples/advanced/extensions/widget.py:44:5:</a> A003 Class attribute `range` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/application/application.py#L318'>src/bokeh/application/application.py:318:9:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/session.py#L365'>src/bokeh/client/session.py:365:9:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommand.py#L129'>src/bokeh/command/subcommand.py:129:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommand.py#L69'>src/bokeh/command/subcommand.py:69:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommand.py#L72'>src/bokeh/command/subcommand.py:72:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/build.py#L53'>src/bokeh/command/subcommands/build.py:53:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/info.py#L107'>src/bokeh/command/subcommands/info.py:107:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/init.py#L53'>src/bokeh/command/subcommands/init.py:53:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/json.py#L82'>src/bokeh/command/subcommands/json.py:82:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/sampledata.py#L71'>src/bokeh/command/subcommands/sampledata.py:71:5:</a> A003 Class attribute `help` is shadowing a Python builtin
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/1da08460cba9f3d67b7b84249ccd8ee9bd282bf7/common/logging_extra.py#L68'>common/logging_extra.py:68:29:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
+ <a href='https://github.com/commaai/openpilot/blob/1da08460cba9f3d67b7b84249ccd8ee9bd282bf7/common/logging_extra.py#L98'>common/logging_extra.py:98:29:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
+ <a href='https://github.com/commaai/openpilot/blob/1da08460cba9f3d67b7b84249ccd8ee9bd282bf7/selfdrive/athena/athenad.py#L82'>selfdrive/athena/athenad.py:82:21:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/f23d06bfce724138d70df8570c4f70a8d61805f5/rotkehlchen/api/v1/schemas.py#L2183'>rotkehlchen/api/v1/schemas.py:2183:42:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
+ <a href='https://github.com/rotki/rotki/blob/f23d06bfce724138d70df8570c4f70a8d61805f5/rotkehlchen/utils/hexbytes.py#L53'>rotkehlchen/utils/hexbytes.py:53:44:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -105 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/check_analytics_state.py#L24'>analytics/management/commands/check_analytics_state.py:24:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/clear_analytics_tables.py#L11'>analytics/management/commands/clear_analytics_tables.py:11:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/clear_single_stat.py#L11'>analytics/management/commands/clear_single_stat.py:11:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/populate_analytics_db.py#L42'>analytics/management/commands/populate_analytics_db.py:42:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/update_analytics_counts.py#L22'>analytics/management/commands/update_analytics_counts.py:22:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/models.py#L15'>analytics/models.py:15:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/models.py#L41'>analytics/models.py:41:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/confirmation/models.py#L206'>confirmation/models.py:206:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/corporate/models.py#L119'>corporate/models.py:119:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/corporate/models.py#L77'>corporate/models.py:77:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/tools/lib/gitlint_rules.py#L82'>tools/lib/gitlint_rules.py:82:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/actions/message_flags.py#L28'>zerver/actions/message_flags.py:28:5:</a> A003 Class attribute `all` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/actions/message_flags.py#L29'>zerver/actions/message_flags.py:29:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/bot_lib.py#L153'>zerver/lib/bot_lib.py:153:9:</a> A003 Class attribute `quit` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/drafts.py#L27'>zerver/lib/drafts.py:27:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/logging_util.py#L67'>zerver/lib/logging_util.py:67:9:</a> A003 Class attribute `filter` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/mention.py#L30'>zerver/lib/mention.py:30:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/mention.py#L37'>zerver/lib/mention.py:37:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L47'>zerver/lib/remote_server.py:47:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L49'>zerver/lib/remote_server.py:49:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L56'>zerver/lib/remote_server.py:56:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L57'>zerver/lib/remote_server.py:57:5:</a> A003 Class attribute `id` is shadowing a Python builtin
... 83 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
- examples/RAG_with_graph_db.ipynb:cell 45:58:9: A003 Class attribute `format` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A003 | 225 | 0 | 225 | 0 | 0 |
| RUF100 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -216 violations, +0 -0 fixes in 6 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -59 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/dataset_schema.py#L123'>airflow/api_connexion/schemas/dataset_schema.py:123:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/dataset_schema.py#L69'>airflow/api_connexion/schemas/dataset_schema.py:69:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/event_log_schema.py#L35'>airflow/api_connexion/schemas/event_log_schema.py:35:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/job_schema.py#L33'>airflow/api_connexion/schemas/job_schema.py:33:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/api_connexion/schemas/trigger_schema.py#L33'>airflow/api_connexion/schemas/trigger_schema.py:33:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/auth/managers/models/resource_details.py#L42'>airflow/auth/managers/models/resource_details.py:42:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/cli/cli_config.py#L1011'>airflow/cli/cli_config.py:1011:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/cli/cli_config.py#L1023'>airflow/cli/cli_config.py:1023:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/configuration.py#L1296'>airflow/configuration.py:1296:9:</a> A003 Class attribute `set` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/jobs/job.py#L69'>airflow/jobs/job.py:69:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/models/connection.py#L91'>airflow/models/connection.py:91:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/models/dagpickle.py#L44'>airflow/models/dagpickle.py:44:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/4a5da8e05e7ce29dff0ac780a9be9bfb55f216da/airflow/models/dagrun.py#L120'>airflow/models/dagrun.py:120:5:</a> A003 Class attribute `id` is shadowing a Python builtin
... 46 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -52 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/examples/advanced/extensions/widget.py#L44'>examples/advanced/extensions/widget.py:44:5:</a> A003 Class attribute `range` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/session.py#L365'>src/bokeh/client/session.py:365:9:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommand.py#L129'>src/bokeh/command/subcommand.py:129:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommand.py#L69'>src/bokeh/command/subcommand.py:69:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommand.py#L72'>src/bokeh/command/subcommand.py:72:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/build.py#L53'>src/bokeh/command/subcommands/build.py:53:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/info.py#L107'>src/bokeh/command/subcommands/info.py:107:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/init.py#L53'>src/bokeh/command/subcommands/init.py:53:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/json.py#L82'>src/bokeh/command/subcommands/json.py:82:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/sampledata.py#L71'>src/bokeh/command/subcommands/sampledata.py:71:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/secret.py#L67'>src/bokeh/command/subcommands/secret.py:67:5:</a> A003 Class attribute `help` is shadowing a Python builtin
... 41 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/1da08460cba9f3d67b7b84249ccd8ee9bd282bf7/common/logging_extra.py#L68'>common/logging_extra.py:68:29:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
+ <a href='https://github.com/commaai/openpilot/blob/1da08460cba9f3d67b7b84249ccd8ee9bd282bf7/common/logging_extra.py#L98'>common/logging_extra.py:98:29:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
+ <a href='https://github.com/commaai/openpilot/blob/1da08460cba9f3d67b7b84249ccd8ee9bd282bf7/selfdrive/athena/athenad.py#L82'>selfdrive/athena/athenad.py:82:21:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/f23d06bfce724138d70df8570c4f70a8d61805f5/rotkehlchen/api/v1/schemas.py#L2183'>rotkehlchen/api/v1/schemas.py:2183:42:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
+ <a href='https://github.com/rotki/rotki/blob/f23d06bfce724138d70df8570c4f70a8d61805f5/rotkehlchen/utils/hexbytes.py#L53'>rotkehlchen/utils/hexbytes.py:53:44:</a> RUF100 [*] Unused `noqa` directive (unused: `A003`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -104 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/check_analytics_state.py#L24'>analytics/management/commands/check_analytics_state.py:24:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/clear_analytics_tables.py#L11'>analytics/management/commands/clear_analytics_tables.py:11:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/clear_single_stat.py#L11'>analytics/management/commands/clear_single_stat.py:11:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/populate_analytics_db.py#L42'>analytics/management/commands/populate_analytics_db.py:42:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/management/commands/update_analytics_counts.py#L22'>analytics/management/commands/update_analytics_counts.py:22:5:</a> A003 Class attribute `help` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/models.py#L15'>analytics/models.py:15:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/analytics/models.py#L41'>analytics/models.py:41:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/confirmation/models.py#L206'>confirmation/models.py:206:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/corporate/models.py#L119'>corporate/models.py:119:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/corporate/models.py#L77'>corporate/models.py:77:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/tools/lib/gitlint_rules.py#L82'>tools/lib/gitlint_rules.py:82:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/actions/message_flags.py#L28'>zerver/actions/message_flags.py:28:5:</a> A003 Class attribute `all` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/actions/message_flags.py#L29'>zerver/actions/message_flags.py:29:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/bot_lib.py#L153'>zerver/lib/bot_lib.py:153:9:</a> A003 Class attribute `quit` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/drafts.py#L27'>zerver/lib/drafts.py:27:5:</a> A003 Class attribute `type` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/logging_util.py#L67'>zerver/lib/logging_util.py:67:9:</a> A003 Class attribute `filter` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/mention.py#L30'>zerver/lib/mention.py:30:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/mention.py#L37'>zerver/lib/mention.py:37:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L47'>zerver/lib/remote_server.py:47:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L49'>zerver/lib/remote_server.py:49:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L56'>zerver/lib/remote_server.py:56:5:</a> A003 Class attribute `property` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L57'>zerver/lib/remote_server.py:57:5:</a> A003 Class attribute `id` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/8bd9a9121689ba1c1fe9a2c26838a6a36aed2b13/zerver/lib/remote_server.py#L64'>zerver/lib/remote_server.py:64:5:</a> A003 Class attribute `id` is shadowing a Python builtin
... 81 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
- examples/RAG_with_graph_db.ipynb:cell 45:58:9: A003 Class attribute `format` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A003 | 216 | 0 | 216 | 0 | 0 |
| RUF100 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @zanieb by @charliermarsh on 2024-01-11 15:07_

---

_@zanieb approved on 2024-01-11 17:57_

---

_Merged by @charliermarsh on 2024-01-11 17:59_

---

_Closed by @charliermarsh on 2024-01-11 17:59_

---

_Branch deleted on 2024-01-11 17:59_

---

```yaml
number: 4880
title: "`[flake8-bugbear]` add autofix for `B006` `mutable_argument_default`"
type: pull_request
state: closed
author: qdegraaf
labels: []
assignees: []
base: main
head: autofix/B006
created_at: 2023-06-05T21:02:36Z
updated_at: 2023-07-27T15:12:00Z
url: https://github.com/astral-sh/ruff/pull/4880
synced_at: 2026-01-12T15:55:16Z
```

# `[flake8-bugbear]` add autofix for `B006` `mutable_argument_default`

---

_@qdegraaf_

## Summary

Adds an autofix for B006 turning mutable argument defaults into None and setting their original value back in the function body if still `None` at runtime like so:
```python
def before(x=[]):
    pass
    
def after(x=None):
    if x is None:
        x = []
    pass
```

## Test Plan

Added an extra test case to existing fixture with more indentation. Checked results for all old examples.

NOTE: Also adapted the jupyter notebook test as this checked for B006 as well.

## Issue link

Closes: https://github.com/charliermarsh/ruff/issues/4693

---

_@qdegraaf reviewed on 2023-06-05 21:06_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:126 on 2023-06-05 21:06_

As discussed in Discord, went in circles a lot trying to get this to work with existing helpers. I almost got something nicer to work with the suggested `ruff_textwrap::indent` but I couldn't get it consistent somehow., so I opted for this rather brutish solution. Open to any edits or advice on this one.

---

_Comment by @github-actions[bot] on 2023-06-05 21:40_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+45, -45, 0 error(s))

<details><summary>airflow (+11, -11)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/models/dag.py#L2125'>airflow/models/dag.py:2125:30:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/models/dag.py#L2125'>airflow/models/dag.py:2125:30:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/hooks/emr.py#L259'>airflow/providers/amazon/aws/hooks/emr.py:259:78:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/hooks/emr.py#L259'>airflow/providers/amazon/aws/hooks/emr.py:259:78:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/operators/lambda_function.py#L76'>airflow/providers/amazon/aws/operators/lambda_function.py:76:24:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/operators/lambda_function.py#L76'>airflow/providers/amazon/aws/operators/lambda_function.py:76:24:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/sensors/lambda_function.py#L59'>airflow/providers/amazon/aws/sensors/lambda_function.py:59:31:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/sensors/lambda_function.py#L59'>airflow/providers/amazon/aws/sensors/lambda_function.py:59:31:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py#L93'>airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py:93:33:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py#L93'>airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py:93:33:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py#L94'>airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py:94:31:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py#L94'>airflow/providers/amazon/aws/transfers/azure_blob_to_s3.py:94:31:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/redshift_to_s3.py#L106'>airflow/providers/amazon/aws/transfers/redshift_to_s3.py:106:42:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/redshift_to_s3.py#L106'>airflow/providers/amazon/aws/transfers/redshift_to_s3.py:106:42:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/s3_to_redshift.py#L99'>airflow/providers/amazon/aws/transfers/s3_to_redshift.py:99:42:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/providers/amazon/aws/transfers/s3_to_redshift.py#L99'>airflow/providers/amazon/aws/transfers/s3_to_redshift.py:99:42:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/utils/event_scheduler.py#L32'>airflow/utils/event_scheduler.py:32:16:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/airflow/utils/event_scheduler.py#L32'>airflow/utils/event_scheduler.py:32:16:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py#L93'>tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py:93:74:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py#L93'>tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py:93:74:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/tests/conftest.py#L719'>tests/conftest.py:719:25:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/apache/airflow/blob/4a44343731144a7a7dc7fff7e3ed01663d4dd2e1/tests/conftest.py#L719'>tests/conftest.py:719:25:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
</pre>

</p>
</details>
<details><summary>bokeh (+34, -34)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/examples/interaction/js_callbacks/js_on_event.py#L15'>examples/interaction/js_callbacks/js_on_event.py:15:53:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/examples/interaction/js_callbacks/js_on_event.py#L15'>examples/interaction/js_callbacks/js_on_event.py:15:53:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/examples/server/app/events_app.py#L16'>examples/server/app/events_app.py:16:35:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/examples/server/app/events_app.py#L16'>examples/server/app/events_app.py:16:35:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/examples/server/app/events_app.py#L38'>examples/server/app/events_app.py:38:28:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/examples/server/app/events_app.py#L38'>examples/server/app/events_app.py:38:28:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/code.py#L82'>src/bokeh/application/handlers/code.py:82:78:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/code.py#L82'>src/bokeh/application/handlers/code.py:82:78:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L110'>src/bokeh/application/handlers/directory.py:110:65:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/directory.py#L110'>src/bokeh/application/handlers/directory.py:110:65:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/notebook.py#L66'>src/bokeh/application/handlers/notebook.py:66:65:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/notebook.py#L66'>src/bokeh/application/handlers/notebook.py:66:65:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/script.py#L80'>src/bokeh/application/handlers/script.py:80:65:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/script.py#L80'>src/bokeh/application/handlers/script.py:80:65:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_lifecycle.py#L55'>src/bokeh/application/handlers/server_lifecycle.py:55:65:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_lifecycle.py#L55'>src/bokeh/application/handlers/server_lifecycle.py:55:65:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_request_handler.py#L57'>src/bokeh/application/handlers/server_request_handler.py:57:65:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/application/handlers/server_request_handler.py#L57'>src/bokeh/application/handlers/server_request_handler.py:57:65:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/json_encoder.py#L178'>src/bokeh/core/json_encoder.py:178:51:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/json_encoder.py#L178'>src/bokeh/core/json_encoder.py:178:51:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L123'>src/bokeh/core/property/container.py:123:82:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L123'>src/bokeh/core/property/container.py:123:82:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L150'>src/bokeh/core/property/container.py:150:82:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L150'>src/bokeh/core/property/container.py:150:82:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L189'>src/bokeh/core/property/container.py:189:32:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L189'>src/bokeh/core/property/container.py:189:32:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L297'>src/bokeh/core/property/container.py:297:32:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L297'>src/bokeh/core/property/container.py:297:32:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L310'>src/bokeh/core/property/container.py:310:66:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/container.py#L310'>src/bokeh/core/property/container.py:310:66:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/visual.py#L133'>src/bokeh/core/property/visual.py:133:32:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/visual.py#L133'>src/bokeh/core/property/visual.py:133:32:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/visual.py#L92'>src/bokeh/core/property/visual.py:92:32:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/visual.py#L92'>src/bokeh/core/property/visual.py:92:32:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/wrappers.py#L404'>src/bokeh/core/property/wrappers.py:404:37:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/property/wrappers.py#L404'>src/bokeh/core/property/wrappers.py:404:37:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/serialization.py#L217'>src/bokeh/core/serialization.py:217:52:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/core/serialization.py#L217'>src/bokeh/core/serialization.py:217:52:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L108'>src/bokeh/embed/bundle.py:108:46:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L108'>src/bokeh/embed/bundle.py:108:46:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L108'>src/bokeh/embed/bundle.py:108:70:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L108'>src/bokeh/embed/bundle.py:108:70:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L109'>src/bokeh/embed/bundle.py:109:36:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L109'>src/bokeh/embed/bundle.py:109:36:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L109'>src/bokeh/embed/bundle.py:109:61:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L109'>src/bokeh/embed/bundle.py:109:61:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L109'>src/bokeh/embed/bundle.py:109:82:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/bundle.py#L109'>src/bokeh/embed/bundle.py:109:82:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/elements.py#L84'>src/bokeh/embed/elements.py:84:46:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/elements.py#L84'>src/bokeh/embed/elements.py:84:46:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L130'>src/bokeh/embed/server.py:130:114:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/server.py#L130'>src/bokeh/embed/server.py:130:114:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/standalone.py#L299'>src/bokeh/embed/standalone.py:299:52:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/embed/standalone.py#L299'>src/bokeh/embed/standalone.py:299:52:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L317'>src/bokeh/layouts.py:317:26:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/layouts.py#L317'>src/bokeh/layouts.py:317:26:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_renderer.py#L144'>src/bokeh/plotting/_renderer.py:144:56:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_renderer.py#L144'>src/bokeh/plotting/_renderer.py:144:56:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_renderer.py#L144'>src/bokeh/plotting/_renderer.py:144:78:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/src/bokeh/plotting/_renderer.py#L144'>src/bokeh/plotting/_renderer.py:144:78:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/support/util/examples.py#L77'>tests/support/util/examples.py:77:90:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/support/util/examples.py#L77'>tests/support/util/examples.py:77:90:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/unit/bokeh/models/_util_models.py#L87'>tests/unit/bokeh/models/_util_models.py:87:127:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/unit/bokeh/models/_util_models.py#L87'>tests/unit/bokeh/models/_util_models.py:87:127:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/unit/bokeh/server/_util_server.py#L66'>tests/unit/bokeh/server/_util_server.py:66:35:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/unit/bokeh/server/_util_server.py#L66'>tests/unit/bokeh/server/_util_server.py:66:35:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
- <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/unit/bokeh/test_events.py#L43'>tests/unit/bokeh/test_events.py:43:35:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/unit/bokeh/test_events.py#L43'>tests/unit/bokeh/test_events.py:43:35:</a> B006 [*] Replace mutable data structure with `None` in argument default and replace it with data structure inside the function if still `None`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B006 | 90 | 45 | 45 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.6±0.05ms     4.2 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01   1884.1±3.67µs     8.8 MB/sec    1.00   1868.9±3.87µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    210.4±0.28µs    14.0 MB/sec    1.00    210.2±1.44µs    14.0 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.5±0.12ms     3.0 MB/sec    1.00     13.4±0.10ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    440.7±0.92µs     6.7 MB/sec    1.00    434.5±0.64µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.06ms     4.2 MB/sec    1.00      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1401.5±5.19µs    11.9 MB/sec    1.00   1401.8±3.80µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    155.3±0.30µs    19.0 MB/sec    1.00    152.8±2.58µs    19.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.6 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.6±0.61ms     2.8 MB/sec    1.02     14.9±0.89ms     2.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.8±0.15ms     5.9 MB/sec    1.05      3.0±0.18ms     5.6 MB/sec
formatter/numpy/globals.py                 1.00   321.5±25.30µs     9.2 MB/sec    1.03   331.4±31.54µs     8.9 MB/sec
formatter/pydantic/types.py                1.01      6.3±0.31ms     4.1 MB/sec    1.00      6.2±0.29ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.00     20.6±0.66ms  2024.3 KB/sec    1.08     22.2±0.84ms  1874.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.21ms     3.0 MB/sec    1.00      5.6±0.28ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   654.2±39.69µs     4.5 MB/sec    1.00   642.2±31.85µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.6±0.55ms     2.7 MB/sec    1.00      9.5±0.50ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.01     10.9±1.04ms     3.7 MB/sec    1.00     10.9±0.48ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.10ms     7.4 MB/sec    1.00      2.2±0.17ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.09   292.7±24.20µs    10.1 MB/sec    1.00   267.4±18.36µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.06      5.0±0.30ms     5.1 MB/sec    1.00      4.8±0.18ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-06-12 18:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:126 on 2023-06-12 18:31_

I think that what you have here may actually be the best path forward: extract the block indentation, then add two statements at the appropriate depths.

Two pieces of feedback: for the "extra level of indentation", you're using four spaces, but we should instead use `checker.stylist.indentation()`, which gives us the "default indentation" for the file.

And for the newlines, we should use `checker.stylist.line_ending()`.

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:126 on 2023-06-12 20:18_

Cool. Thanks for the feedback. If I understand what you mean by "extra level of indentation" I have pushed the requested edit. Let me know if I misunderstood.

---

_@qdegraaf reviewed on 2023-06-12 20:19_

---

_Renamed from "`[flake8-bugbear]` add autofix for B006 `mutable_argument_default`" to "`[flake8-bugbear]` add autofix for `B006` `mutable_argument_default`" by @qdegraaf on 2023-06-22 19:34_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-26 17:43_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:116 on 2023-07-07 12:26_

i was confused at first why there was no indentation before the `if`, would make sense to add a comment about it

---

_Review comment by @konstin on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-07 12:28_

another test case i'd like to see is a multi-line default value

---

_@konstin approved on 2023-07-07 12:28_

this is really useful fix to have given how popular the problem is

---

_@charliermarsh reviewed on 2023-07-07 15:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:120 on 2023-07-07 15:18_

This should be outside of the `if`, I think?

---

_@charliermarsh reviewed on 2023-07-07 15:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:119 on 2023-07-07 15:18_

I think this should be suggested, arguably even manual.

---

_@charliermarsh reviewed on 2023-07-07 15:19_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-07 15:19_

We also need to handle same-line functions (we should just avoid trying to fix these):

```python
def f(value = {}): ...
```

---

_@qdegraaf reviewed on 2023-07-07 15:36_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-07 15:36_

I'll add both and have already set fix to manual. Can also change to not suggest at all for same-line funcs.

---

_@qdegraaf reviewed on 2023-07-07 15:43_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-07 15:43_

So multiline arg seems to do fine, but same-line func creates defunct Python. It's best to not fix these indeed. Is there a quick way to check if a function is one line, or its body is on the same line as its def?

---

_Review comment by @konstin on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-08 13:41_

`locator.contains_line_break` between the colon (`:`) and the first body statement should work

---

_@konstin reviewed on 2023-07-08 13:41_

---

_Review comment by @konstin on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-08 13:44_

(to avoid misunderstandings: i believe multi line args are fixable, they should just get a test like the one you added. Same line functions on the other hand would require manual indentation handling and i don't think it's worth implementing this)

---

_@konstin reviewed on 2023-07-08 13:44_

---

_@konstin reviewed on 2023-07-08 13:47_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:86 on 2023-07-08 13:47_

it's odd that this is collapsed here. what happens if the content is mandatory multi line, e.g. with a comment in the parentheses?

---

_@qdegraaf reviewed on 2023-07-08 21:47_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:86 on 2023-07-08 21:47_

We end up deleting the comment. Similar thing popped up in a [different PR](https://github.com/astral-sh/ruff/pull/5121#discussion_r1237755195) where I was told most Ruff fixes ignore comments and not doing so requires libCST.  @MichaReiser does the same principle apply here? 

---

_@qdegraaf reviewed on 2023-07-08 22:08_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-08 22:08_

Added a check for single-line func and changed from `AlwaysAutoFixableViolation` to `Violation` and sometimes fixable (also already changed the fixes to manual as per request in another comment so unsure what type of Violation the rule should now be to be fair, as so far it appears to be the only rule using `manual_edit(s)`)

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:196 on 2023-07-09 07:48_

Any idea why these parenthesis are being added here as the default value doesn't contain them? Same thing is happening for other comprehensions as well.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:86 on 2023-07-09 07:53_

Or if the dictionary is pretty long:
```python
def multiline_arg(
    arg={
        "key1": "value",
        "key2": "value",
        "key3": "value",
        "key4": "value",
        "key5": "value",
        "key6": "value",
    }
):
    pass
```

It's getting collapsed as well:
```diff
--- /Users/dhruv/playground/ruff/src/play.py
+++ /Users/dhruv/playground/ruff/src/play.py
@@ -1,11 +1,6 @@
 def multiline_arg(
-    arg={
-        "key1": "value",
-        "key2": "value",
-        "key3": "value",
-        "key4": "value",
-        "key5": "value",
-        "key6": "value",
-    }
+    arg=None
 ):
+    if arg is None:
+        arg = {"key1": "value", "key2": "value", "key3": "value", "key4": "value", "key5": "value", "key6": "value"}
     pass
```

---

_@dhruvmanila reviewed on 2023-07-09 07:56_

---

_@qdegraaf reviewed on 2023-07-09 08:46_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:196 on 2023-07-09 08:46_

I assumed it was intended behaviour of `checker.generator().expr(default)` which for `ListComp`'s (and other comprehensions) unparses in two parts:

```rust
Expr::ListComp(ast::ExprListComp {
                elt,
                generators,
                range: _range,
            }) => {
                self.p("[");
                self.unparse_expr(elt, precedence::MAX);
                self.unparse_comp(generators);
                self.p("]");
            }
```
The `unparse_expr` func adds parentheses conditinoally here, before matching the `ast`:
```rust
macro_rules! group_if {
            ($lvl:expr, $body:block) => {{
                let group = level > $lvl;
                self.p_if(group, "(");
                let ret = $body;
                self.p_if(group, ")");
                ret
            }};
        }
```

I figured this added the parentheses and was intended but looking at the tests of the generator now (e.g.:

```rust
        assert_round_trip!("{i for i in b async for i in a if await i for b in i}");
```

I'm not entirely sure this is the case anymore

---

_@qdegraaf reviewed on 2023-07-09 14:16_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-09 14:16_

Hmm my heuristic is not covering all cases in the end.

```rust
checker
                    .locator
                    .contains_line_break(TextRange::new(arguments.range().end(), body[0].start()))
```

Allows weird things like 

```python
def single_line_func_wrong(value = {}

                           )\
    : ...
```

To slip through. Is there a way to efficiently locate the range value of the `:`? I have found the `lexer::lex` but that would require getting a string representation of the function def. Is that the way to go, or is there a better way to locate single characters in parts of a `StmtFunctionDef`? Maybe a `find_x_in_range` like util or something along those lines?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:70 on 2023-07-10 07:50_

Maybe swap the title and message? The title should be short to fit into the IDE popup. The message is allowed to be more extensive.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:77 on 2023-07-10 07:50_

Nit: You could pass the whole FuncDef (or `AnyFuncDef`) instead of the individual arguments

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:109 on 2023-07-10 07:51_

You can use the `indentation_at_offset` helper here:

https://github.com/astral-sh/ruff/blob/3f1b489058d8be09b151563cc04b174c8eb2342b/crates/ruff_python_ast/src/whitespace.rs#L18

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:86 on 2023-07-10 07:53_

I think that's fine for a first version. We can either try to improve this in a follow up PR, wait for Ruff's formatter to automatically fix it, or leave it as is.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:131 on 2023-07-10 07:55_

Manual edits won't be applied when running `--fix` in the future and are intended to be used for fixes that are known to contain syntax errors. Do you expect the fixes to contain syntax errors? If not, consider using `suggested` instead.

---

_@MichaReiser reviewed on 2023-07-10 07:55_

---

_@qdegraaf reviewed on 2023-07-10 08:50_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:131 on 2023-07-10 08:50_

Currently only syntax errors I am aware of that can occur is in the case of weirdly formatted single line funcs. See also: https://github.com/astral-sh/ruff/pull/4880#discussion_r1257490739 if that can be fixed it can go to suggested AFAIAC.

---

_@qdegraaf reviewed on 2023-07-10 08:55_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:77 on 2023-07-10 08:55_

How would I make that work with the `checkers/ast/mod.rs` setup of:

```rust
 Stmt::FunctionDef(ast::StmtFunctionDef {
                name,
                decorator_list,
                returns,
                args,
                body,
                ..
            })
            | Stmt::AsyncFunctionDef(ast::StmtAsyncFunctionDef {
                name,
                decorator_list,
                returns,
                args,
                body,
                ..
            })
```

Can't give them the same binding but giving them different bindings and then checking those again inside

```rust
if self.enabled(Rule::MutableArgumentDefault) {
```

Seemed less clean than just accepting the arguments and body still.

---

_@MichaReiser reviewed on 2023-07-13 06:42_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:77 on 2023-07-13 06:42_

Hmm yeah that's complicated to do in isolation. You would need to have two dedicated branches, one for each function type, then call `AnyFunctionDef::from(func)`, and pass that `AnyFunctionDef` to a `check_any_function_def` that calls all existing rules. Let's leave it for now. 

---

_@MichaReiser reviewed on 2023-07-13 06:51_

---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-13 06:51_

Using the lexer when creating a diagnostic can be useful, but it is preferred not to when possible. But I don't see how you can get away with not using the lexer here (we wrote a more lightweight lexer in the formatter, because getting to tokens is such a common task, maybe it's time to make it available to ruff too). You can get the string representation of the function def by using `locator.slice(def.range())`. You can then start lexing *after* the last argument, find the `colon`. Then use `locator.contains_line_break(TextRange::new(colon.end(), first_statement.start()`

Which raises an interesting question. Is this valid? Is this a suite or a single statement body 

```python
def single_line_func_wrong(value = {}):\
	...
```

Unrelated: @konstin a use case where `StmtSuite` would be handy... It would tell us right away whether it is a single statement or a suite. 

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:196 on 2023-07-13 07:14_

Yeah, this seems like a bug:

```rust
assert_round_trip!("[i*2 for i in range(5)]");
// thread 'source_code::generator::tests::tmp_test' panicked at 'assertion failed: `(left == right)`
//  left: `"[(i * 2) for i in range(5)]"`,
// right: `"[i*2 for i in range(5)]"`', crates/ruff_python_ast/src/source_code/generator.rs:1491:9
// note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
// test source_code::generator::tests::tmp_test ... FAILED
```

/cc @MichaReiser 

---

_@dhruvmanila reviewed on 2023-07-13 07:14_

---

_@MichaReiser reviewed on 2023-07-14 06:28_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:196 on 2023-07-14 06:28_

Omg, too much macro use in our `Generator`. This is a precedence issue that's probably worth opening an issue for.

This code decides whether to parenthesize an expression or not 

https://github.com/astral-sh/ruff/blob/58a79c1392003ec63904fe20a33e7b62857c3db1/crates/ruff_python_ast/src/source_code/generator.rs#L874-L881

The `oprec` macro took me a while to understand but precedence either gets assigned the `AND` (33) or `OR` (31) precedence (the same is true for bin op, it's just that they use slightly different values for the precedence)

Now, the generator adds parentheses if the parent's precedence is higher. We use the `MAX` precedence for the list comprehension, which means all expressions get parenthesized

https://github.com/astral-sh/ruff/blob/58a79c1392003ec63904fe20a33e7b62857c3db1/crates/ruff_python_ast/src/source_code/generator.rs#L1010-L1019

The fix requires identifying the right precedence and passing it through. A second (unrelated) refactor could be to use an enum for the precedence instead of an `u8`.

---

_Comment by @dhruvmanila on 2023-07-15 13:32_

Sorry I didn't realize that merging #5776 would create merge conflicts here.

Edit: I've resolved the conflicts :)

---

_@dhruvmanila reviewed on 2023-07-15 13:41_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B006_B006_B008.py.snap`:196 on 2023-07-15 13:41_

Tracking this in #5777 

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-17 08:35_

Thanks for the hyperclear instructions and context. I'll see if I can spot and autofix the 
```python
def single_line_func_wrong(value = {}):\
    ...
```
scenario without too much hassle and, if so, include it and only leave out the 
```python
def single_line_func_wrong(value = {}): ...
```
scenario.

---

_@qdegraaf reviewed on 2023-07-17 08:35_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-17 08:44_

---

_@qdegraaf reviewed on 2023-07-17 09:40_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/flake8_bugbear/B006_B008.py`:75 on 2023-07-17 09:40_

So I've managed to exclude `def single_line_func_wrong(value = {}): ...` based on your instructions. The other scenario you raise is tricky though. The lexer does not do anything with the `\` and it counts as a line break for `contains_line_break()` so current implementation still tries to make:
```python
def single_line_func_wrong(value = None):\
    if value is None:
        value = {}
```
of it, which is broken. Is there a way to discern `\` line breaks from other line breaks? Or do we want to go a different route here? 

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:69 on 2023-07-20 14:35_

Should this be reversed as in the new message (the one with "Replace...") would be the autofix title?

---

_@dhruvmanila reviewed on 2023-07-20 14:38_

---

_@qdegraaf reviewed on 2023-07-20 18:06_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:69 on 2023-07-20 18:06_

Yeah that was the case until: https://github.com/astral-sh/ruff/pull/4880#discussion_r1257857815 but that was more of a length argument. Agree that the phrasing now is potentially confusing. Can find a way to get the best of both worlds.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:69 on 2023-07-21 02:23_

Interesting! I think editors would be using the violation message to display instead of the autofix title and I believe that's the top level key in the diagnostic data sent by the LSP server as well. \cc @MichaReiser 

---

_@dhruvmanila reviewed on 2023-07-21 02:23_

---

_Comment by @charliermarsh on 2023-07-21 03:22_

(This PR is on me to do final review + merge.)

---

_Closed by @qdegraaf on 2023-07-27 15:12_

---

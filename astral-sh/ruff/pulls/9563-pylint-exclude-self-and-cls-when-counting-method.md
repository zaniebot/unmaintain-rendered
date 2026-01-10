```yaml
number: 9563
title: "[`pylint`] Exclude `self` and `cls` when counting method arguments"
type: pull_request
state: merged
author: tmke8
labels:
  - rule
assignees: []
merged: true
base: main
head: positional-exclude-self-cls
created_at: 2024-01-17T14:01:45Z
updated_at: 2024-01-18T11:35:03Z
url: https://github.com/astral-sh/ruff/pull/9563
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Exclude `self` and `cls` when counting method arguments

---

_Pull request opened by @tmke8 on 2024-01-17 14:01_

closes #9552

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR detects whether PLR0917 is being applied to a method or class method, and if so, it ignores the first argument for the purposes of counting the number of positional arguments.

## Test Plan

<!-- How was it tested? -->
New tests have been added to the corresponding fixture.


---

_@tmke8 reviewed on 2024-01-17 14:02_

---

_Review comment by @tmke8 on `crates/ruff_linter/resources/test/fixtures/pylint/too_many_positional.py`:1 on 2024-01-17 14:02_

The comments on theses tests were wrong, so I fixed them as well.

---

_Comment by @codspeed-hq[bot] on 2024-01-17 14:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tmke8:positional-exclude-self-cls)

### Merging #9563 will **not alter performance**

<sub>Comparing <code>tmke8:positional-exclude-self-cls</code> (53ee98a) with <code>main</code> (29c130f)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-01-17 14:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1355 -2029 violations, +0 -0 fixes in 10 projects; 33 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+20 -32 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/argx.py#L316'>aiven/client/argx.py:316:9:</a> PLR0917 Too many positional arguments (10/5)
+ <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/argx.py#L316'>aiven/client/argx.py:316:9:</a> PLR0917 Too many positional arguments (9/5)
- <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/cli.py#L3633'>aiven/client/cli.py:3633:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/cli.py#L5697'>aiven/client/cli.py:5697:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/client.py#L1080'>aiven/client/client.py:1080:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/client.py#L1090'>aiven/client/client.py:1090:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/client.py#L1166'>aiven/client/client.py:1166:9:</a> PLR0917 Too many positional arguments (10/5)
+ <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/client.py#L1166'>aiven/client/client.py:1166:9:</a> PLR0917 Too many positional arguments (9/5)
+ <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/client.py#L1206'>aiven/client/client.py:1206:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/client.py#L1206'>aiven/client/client.py:1206:9:</a> PLR0917 Too many positional arguments (7/5)
... 42 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+780 -1077 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/api/client/api_client.py#L33'>airflow/api/client/api_client.py:33:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/api/client/json_client.py#L57'>airflow/api/client/json_client.py:57:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/api/client/local_client.py#L31'>airflow/api/client/local_client.py:31:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/callbacks/callback_requests.py#L76'>airflow/callbacks/callback_requests.py:76:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0917 Too many positional arguments (10/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0917 Too many positional arguments (11/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/cli/commands/webserver_command.py#L84'>airflow/cli/commands/webserver_command.py:84:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/cli/commands/webserver_command.py#L84'>airflow/cli/commands/webserver_command.py:84:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1050'>airflow/configuration.py:1050:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1050'>airflow/configuration.py:1050:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1071'>airflow/configuration.py:1071:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1071'>airflow/configuration.py:1071:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1092'>airflow/configuration.py:1092:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1092'>airflow/configuration.py:1092:9:</a> PLR0917 Too many positional arguments (8/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1114'>airflow/configuration.py:1114:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1114'>airflow/configuration.py:1114:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1366'>airflow/configuration.py:1366:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L1366'>airflow/configuration.py:1366:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L550'>airflow/configuration.py:550:9:</a> PLR0917 Too many positional arguments (10/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L550'>airflow/configuration.py:550:9:</a> PLR0917 Too many positional arguments (11/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L609'>airflow/configuration.py:609:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L609'>airflow/configuration.py:609:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L636'>airflow/configuration.py:636:9:</a> PLR0917 Too many positional arguments (11/5)
- <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/configuration.py#L636'>airflow/configuration.py:636:9:</a> PLR0917 Too many positional arguments (12/5)
+ <a href='https://github.com/apache/airflow/blob/3806a63bfe0c07d120a7181d751033c850f54997/airflow/dag_processing/manager.py#L119'>airflow/dag_processing/manager.py:119:9:</a> PLR0917 Too many positional arguments (6/5)
... 1830 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+207 -318 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/cli/global_config.py#L184'>samcli/cli/global_config.py:184:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/cli/global_config.py#L223'>samcli/cli/global_config.py:223:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/delete/delete_context.py#L32'>samcli/commands/delete/delete_context.py:32:9:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/delete/delete_context.py#L32'>samcli/commands/delete/delete_context.py:32:9:</a> PLR0917 Too many positional arguments (9/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/deploy/deploy_context.py#L183'>samcli/commands/deploy/deploy_context.py:183:9:</a> PLR0917 Too many positional arguments (14/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/deploy/deploy_context.py#L183'>samcli/commands/deploy/deploy_context.py:183:9:</a> PLR0917 Too many positional arguments (15/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/deploy/deploy_context.py#L51'>samcli/commands/deploy/deploy_context.py:51:9:</a> PLR0917 Too many positional arguments (24/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/deploy/deploy_context.py#L51'>samcli/commands/deploy/deploy_context.py:51:9:</a> PLR0917 Too many positional arguments (25/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/deploy/guided_config.py#L51'>samcli/commands/deploy/guided_config.py:51:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/7ba03d6d1f5a718cbb851ae878bff90e04d4d31f/samcli/commands/deploy/guided_context.py#L318'>samcli/commands/deploy/guided_context.py:318:9:</a> PLR0917 Too many positional arguments (6/5)
... 515 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+13 -25 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/connection.py#L85'>src/bokeh/client/connection.py:85:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/client/session.py#L273'>src/bokeh/client/session.py:273:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property/wrappers.py#L438'>src/bokeh/core/property/wrappers.py:438:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/callbacks.py#L223'>src/bokeh/document/callbacks.py:223:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/callbacks.py#L223'>src/bokeh/document/callbacks.py:223:9:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/events.py#L258'>src/bokeh/document/events.py:258:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/events.py#L290'>src/bokeh/document/events.py:290:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/events.py#L290'>src/bokeh/document/events.py:290:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/events.py#L387'>src/bokeh/document/events.py:387:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/document/events.py#L387'>src/bokeh/document/events.py:387:9:</a> PLR0917 Too many positional arguments (8/5)
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+8 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/journalist_app/sessions.py#L100'>securedrop/journalist_app/sessions.py:100:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/journalist_app/sessions.py#L100'>securedrop/journalist_app/sessions.py:100:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/models.py#L425'>securedrop/models.py:425:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/models.py#L425'>securedrop/models.py:425:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/models.py#L78'>securedrop/models.py:78:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/pretty_bad_protocol/_meta.py#L135'>securedrop/pretty_bad_protocol/_meta.py:135:9:</a> PLR0917 Too many positional arguments (10/5)
... 13 additional changes omitted for rule PLR0917
+ <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/pretty_bad_protocol/_meta.py#L752'>securedrop/pretty_bad_protocol/_meta.py:752:111:</a> RUF100 [*] Unused blanket `noqa` directive
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+4 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/auth.py#L160'>blinkpy/auth.py:160:15:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/auth.py#L160'>blinkpy/auth.py:160:15:</a> PLR0917 Too many positional arguments (9/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/auth.py#L25'>blinkpy/auth.py:25:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/blinkpy.py#L322'>blinkpy/blinkpy.py:322:15:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/blinkpy.py#L322'>blinkpy/blinkpy.py:322:15:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/blinkpy.py#L392'>blinkpy/blinkpy.py:392:15:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/sync_module.py#L648'>blinkpy/sync_module.py:648:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/blinkpy/sync_module.py#L648'>blinkpy/sync_module.py:648:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/tests/mock_responses.py#L8'>tests/mock_responses.py:8:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5146b36b2244d77fa7b1283f206241f0f98e0c68/tests/test_blinkpy.py#L196'>tests/test_blinkpy.py:196:15:</a> PLR0917 Too many positional arguments (6/5)
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+28 -53 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/alchemy/__init__.py#L138'>ibis/backends/base/sql/alchemy/__init__.py:138:9:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/alchemy/__init__.py#L138'>ibis/backends/base/sql/alchemy/__init__.py:138:9:</a> PLR0917 Too many positional arguments (9/5)
- <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/alchemy/__init__.py#L72'>ibis/backends/base/sql/alchemy/__init__.py:72:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/compiler/query_builder.py#L192'>ibis/backends/base/sql/compiler/query_builder.py:192:9:</a> PLR0917 Too many positional arguments (14/5)
- <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/compiler/query_builder.py#L192'>ibis/backends/base/sql/compiler/query_builder.py:192:9:</a> PLR0917 Too many positional arguments (15/5)
- <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/compiler/select_builder.py#L26'>ibis/backends/base/sql/compiler/select_builder.py:26:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/compiler/translator.py#L192'>ibis/backends/base/sql/compiler/translator.py:192:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/ddl.py#L118'>ibis/backends/base/sql/ddl.py:118:9:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/ddl.py#L118'>ibis/backends/base/sql/ddl.py:118:9:</a> PLR0917 Too many positional arguments (9/5)
+ <a href='https://github.com/ibis-project/ibis/blob/1523a1c43f2adcd20e3602b0ce941893a66a70f2/ibis/backends/base/sql/ddl.py#L168'>ibis/backends/base/sql/ddl.py:168:9:</a> PLR0917 Too many positional arguments (8/5)
... 71 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+253 -441 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/groupby.py#L490'>asv_bench/benchmarks/groupby.py:490:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/groupby.py#L573'>asv_bench/benchmarks/groupby.py:573:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/groupby.py#L576'>asv_bench/benchmarks/groupby.py:576:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/rolling.py#L108'>asv_bench/benchmarks/rolling.py:108:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/rolling.py#L108'>asv_bench/benchmarks/rolling.py:108:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/rolling.py#L123'>asv_bench/benchmarks/rolling.py:123:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/rolling.py#L123'>asv_bench/benchmarks/rolling.py:123:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/rolling.py#L214'>asv_bench/benchmarks/rolling.py:214:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/rolling.py#L219'>asv_bench/benchmarks/rolling.py:219:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/d4bd7e40400fc5468d69fd6b92479b85f4ec614e/asv_bench/benchmarks/rolling.py#L241'>asv_bench/benchmarks/rolling.py:241:9:</a> PLR0917 Too many positional arguments (6/5)
... 684 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 3383 | 1354 | 2029 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @charliermarsh on 2024-01-18 03:11_

---

_Renamed from "PLR0917: Ignore first argument in methods" to "[`pylint`] Exclude `self` and `cls` when counting method arguments" by @charliermarsh on 2024-01-18 03:12_

---

_@charliermarsh approved on 2024-01-18 03:12_

---

_Merged by @charliermarsh on 2024-01-18 03:17_

---

_Closed by @charliermarsh on 2024-01-18 03:17_

---

_Branch deleted on 2024-01-18 11:35_

---

```yaml
number: 11139
title: "feat: distinguish * and ** in globs"
type: pull_request
state: closed
author: JP-Ellis
labels:
  - breaking
  - configuration
assignees: []
base: ruff-0.5
head: feat/glob-literal-separator
created_at: 2024-04-25T02:16:17Z
updated_at: 2024-06-26T08:20:25Z
url: https://github.com/astral-sh/ruff/pull/11139
synced_at: 2026-01-12T15:55:34Z
```

# feat: distinguish * and ** in globs

---

_@JP-Ellis_

## Summary

Set `literal_separator` to `true` for globs.

## Motivation

By default, the `globset` crate matches files in a fairly straightforward way, which results in `foo/*.py` matching `foo/hello.py` as well as `foo/bar/hello.py`.

This commit changes this by setting the `literal_separator` option to `true`, which ensures that `*` and `?` never match a path separator. This would align Ruff's behaviour with the [gitignore specification](https://git-scm.com/docs/gitignore#_pattern_format).

- Resolves: #6262

## Test Plan

So far, `cargo test` work, but please advise if you would like me to add new tests specifically for this change (and please point me in the right direction).

---

_Comment by @charliermarsh on 2024-04-25 02:20_

I think I'm cool with changing this but it should probably be considered a breaking change (since files that were ignored will now be unignored).

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-04-25 02:20_

---

_Label `breaking` added by @charliermarsh on 2024-04-25 02:20_

---

_Label `configuration` added by @charliermarsh on 2024-04-25 02:20_

---

_@JP-Ellis reviewed on 2024-04-25 02:26_

Yes, completely agree with this being a breaking change.

I noticed a couple of tests failed in CI, which I will investigate now and see if I can get them to pass.

Update: The default `include` glob had to be updated to `**/*.py` and `**/*.pyi`. All CI tests are now passing.

---

_Comment by @github-actions[bot] on 2024-04-25 03:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+49511 -66 violations, +0 -0 fixes in 14 projects; 30 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+125 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/converters.py#L52'>examples/interactions/converters.py:52:29:</a> B008 Do not perform function call `commands.Param` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/injections.py#L61'>examples/interactions/injections.py:61:22:</a> B008 Do not perform function call `commands.inject` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/injections.py#L75'>examples/interactions/injections.py:75:22:</a> B008 Do not perform function call `commands.inject` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/param.py#L68'>examples/interactions/param.py:68:26:</a> B008 Do not perform function call `commands.Param` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/scripts/ci/versiontool.py#L64'>scripts/ci/versiontool.py:64:5:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/scripts/ci/versiontool.py#L78'>scripts/ci/versiontool.py:78:5:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/scripts/codemods/base.py#L47'>scripts/codemods/base.py:47:13:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/test_bot/cogs/injections.py#L62'>test_bot/cogs/injections.py:62:41:</a> B008 Do not perform function call `commands.Param` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/tests/ext/commands/test_base_core.py#L102'>tests/ext/commands/test_base_core.py:102:9:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/tests/ext/commands/test_base_core.py#L28'>tests/ext/commands/test_base_core.py:28:13:</a> S101 Use of `assert` detected
... 115 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+46555 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/libs/__init__.py#L1'>airflow/example_dags/libs/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/libs/helper.py#L1'>airflow/example_dags/libs/helper.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/libs/helper.py#L21'>airflow/example_dags/libs/helper.py:21:5:</a> D103 Missing docstring in public function
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/__init__.py#L1'>airflow/example_dags/plugins/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/decreasing_priority_weight_strategy.py#L1'>airflow/example_dags/plugins/decreasing_priority_weight_strategy.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/decreasing_priority_weight_strategy.py#L32'>airflow/example_dags/plugins/decreasing_priority_weight_strategy.py:32:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/decreasing_priority_weight_strategy.py#L36'>airflow/example_dags/plugins/decreasing_priority_weight_strategy.py:36:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when dag run state changes to SUCCESS."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when dag run state changes to FAILED."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when dag run state changes to RUNNING."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L1'>airflow/example_dags/plugins/event_listener.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when task state changes to RUNNING."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when task state changes to SUCCESS."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D212 [*] Multi-line docstring summary should start at the first line
... 1886 additional changes omitted for rule D212
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when task state changes to FAILED."
... 398 additional changes omitted for rule D401
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D404 First word of the docstring should not be "This"
... 81 additional changes omitted for rule D404
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/listener_plugin.py#L1'>airflow/example_dags/plugins/listener_plugin.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/listener_plugin.py#L24'>airflow/example_dags/plugins/listener_plugin.py:24:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L45'>airflow/example_dags/plugins/workday.py:45:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L46'>airflow/example_dags/plugins/workday.py:46:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L60'>airflow/example_dags/plugins/workday.py:60:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L69'>airflow/example_dags/plugins/workday.py:69:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L99'>airflow/example_dags/plugins/workday.py:99:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/subdags/__init__.py#L1'>airflow/example_dags/subdags/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/arangodb/example_dags/__init__.py#L1'>airflow/providers/arangodb/example_dags/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/arangodb/example_dags/example_arangodb.py#L1'>airflow/providers/arangodb/example_dags/example_arangodb.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/__init__.py#L1'>airflow/providers/google/cloud/example_dags/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py#L18'>airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py:18:1:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/example_looker.py#L18'>airflow/providers/google/cloud/example_dags/example_looker.py:18:1:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/example_presto_to_gcs.py#L18'>airflow/providers/google/cloud/example_dags/example_presto_to_gcs.py:18:1:</a> D200 One-line docstring should fit on one line
... 46509 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+208 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/config_from_2014.py#L40'>securedrop/tests/config_from_2014.py:40:22:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/journalist.pid"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/config_from_2014.py#L41'>securedrop/tests/config_from_2014.py:41:18:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/source.pid"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/config_from_2014.py#L70'>securedrop/tests/config_from_2014.py:70:23:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/securedrop"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/conftest.py#L110'>securedrop/tests/conftest.py:110:37:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/sd-tests/conftest-"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/conftest.py#L310'>securedrop/tests/conftest.py:310:33:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/securedrop_test_worker.pid"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/conftest.py#L328'>securedrop/tests/conftest.py:328:28:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/test_rqworker.log"
... 6 additional changes omitted for rule S108
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L106'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:106:9:</a> S101 Use of `assert` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L122'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:122:9:</a> S101 Use of `assert` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L148'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:148:17:</a> S101 Use of `assert` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L172'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:172:9:</a> S101 Use of `assert` detected
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L27'>ibis/backends/tests/base.py:27:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L27'>ibis/backends/tests/base.py:27:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L337'>ibis/backends/tests/base.py:337:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L81'>ibis/backends/tests/base.py:81:9:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L81'>ibis/backends/tests/base.py:81:9:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/tests/util.py#L25'>ibis/tests/util.py:25:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/tests/util.py#L25'>ibis/tests/util.py:25:5:</a> D209 [*] Multi-line docstring closing quotes should be on a separate line
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+388 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/dev/proto_to_graphql/autogeneration_utils.py#L35'>dev/proto_to_graphql/autogeneration_utils.py:35:5:</a> T201 `print` found
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/auth/auth.py#L56'>examples/auth/auth.py:56:9:</a> T201 `print` found
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/catboost/train.py#L39'>examples/catboost/train.py:39:1:</a> T201 `print` found
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/databricks/log_runs.py#L28'>examples/databricks/log_runs.py:28:101:</a> E501 Line too long (102 > 100)
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/databricks/log_runs.py#L41'>examples/databricks/log_runs.py:41:5:</a> T201 `print` found
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/databricks/log_runs.py#L5'>examples/databricks/log_runs.py:5:101:</a> E501 Line too long (106 > 100)
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/databricks/multipart.py#L138'>examples/databricks/multipart.py:138:9:</a> T201 `print` found
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/databricks/multipart.py#L143'>examples/databricks/multipart.py:143:5:</a> T201 `print` found
... 299 additional changes omitted for rule T201
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/deployments/deployments_server/mistral/example.py#L25'>examples/deployments/deployments_server/mistral/example.py:25:101:</a> E501 Line too long (109 > 100)
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/examples/docker/train.py#L3'>examples/docker/train.py:3:101:</a> E501 Line too long (135 > 100)
... 382 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (56 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S101 | 19886 | 19886 | 0 | 0 | 0 |
| D102 | 11929 | 11929 | 0 | 0 | 0 |
| D103 | 2741 | 2741 | 0 | 0 | 0 |
| D101 | 2242 | 2242 | 0 | 0 | 0 |
| D212 | 1893 | 1893 | 0 | 0 | 0 |
| D100 | 1694 | 1694 | 0 | 0 | 0 |
| D400 | 1514 | 1514 | 0 | 0 | 0 |
| D415 | 1509 | 1509 | 0 | 0 | 0 |
| S113 | 1250 | 1250 | 0 | 0 | 0 |
| D200 | 930 | 930 | 0 | 0 | 0 |
| D205 | 853 | 853 | 0 | 0 | 0 |
| D104 | 693 | 693 | 0 | 0 | 0 |
| NPY002 | 476 | 476 | 0 | 0 | 0 |
| D401 | 403 | 403 | 0 | 0 | 0 |
| T201 | 325 | 325 | 0 | 0 | 0 |
| D202 | 247 | 247 | 0 | 0 | 0 |
| D107 | 122 | 122 | 0 | 0 | 0 |
| FLY002 | 122 | 122 | 0 | 0 | 0 |
| D404 | 86 | 86 | 0 | 0 | 0 |
| S311 | 80 | 80 | 0 | 0 | 0 |
| E501 | 65 | 65 | 0 | 0 | 0 |
| D105 | 62 | 62 | 0 | 0 | 0 |
| RUF012 | 54 | 54 | 0 | 0 | 0 |
| D403 | 47 | 47 | 0 | 0 | 0 |
| TRY002 | 37 | 37 | 0 | 0 | 0 |
| D209 | 36 | 36 | 0 | 0 | 0 |
| B028 | 35 | 35 | 0 | 0 | 0 |
| A001 | 33 | 0 | 33 | 0 | 0 |
| N806 | 16 | 16 | 0 | 0 | 0 |
| B018 | 16 | 0 | 16 | 0 | 0 |
| TID253 | 15 | 15 | 0 | 0 | 0 |
| TID252 | 15 | 15 | 0 | 0 | 0 |
| INP001 | 15 | 15 | 0 | 0 | 0 |
| N802 | 15 | 15 | 0 | 0 | 0 |
| TID251 | 13 | 13 | 0 | 0 | 0 |
| S105 | 13 | 13 | 0 | 0 | 0 |
| S108 | 11 | 11 | 0 | 0 | 0 |
| UP031 | 11 | 7 | 4 | 0 | 0 |
| A002 | 11 | 0 | 11 | 0 | 0 |
| T203 | 9 | 9 | 0 | 0 | 0 |
| RET504 | 9 | 9 | 0 | 0 | 0 |
| B011 | 6 | 6 | 0 | 0 | 0 |
| N803 | 6 | 6 | 0 | 0 | 0 |
| B008 | 5 | 5 | 0 | 0 | 0 |
| PERF203 | 5 | 5 | 0 | 0 | 0 |
| D407 | 4 | 4 | 0 | 0 | 0 |
| D413 | 3 | 3 | 0 | 0 | 0 |
| PT018 | 2 | 2 | 0 | 0 | 0 |
| D106 | 2 | 2 | 0 | 0 | 0 |
| D419 | 2 | 2 | 0 | 0 | 0 |
| PIE790 | 2 | 2 | 0 | 0 | 0 |
| N812 | 2 | 2 | 0 | 0 | 0 |
| E703 | 2 | 0 | 2 | 0 | 0 |
| D406 | 1 | 1 | 0 | 0 | 0 |
| D412 | 1 | 1 | 0 | 0 | 0 |
| RUF018 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+49879 -93 violations, +0 -0 fixes in 15 projects; 29 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+125 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/converters.py#L52'>examples/interactions/converters.py:52:29:</a> B008 Do not perform function call `commands.Param` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/injections.py#L61'>examples/interactions/injections.py:61:22:</a> B008 Do not perform function call `commands.inject` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/injections.py#L75'>examples/interactions/injections.py:75:22:</a> B008 Do not perform function call `commands.inject` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/examples/interactions/param.py#L68'>examples/interactions/param.py:68:26:</a> B008 Do not perform function call `commands.Param` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/scripts/ci/versiontool.py#L64'>scripts/ci/versiontool.py:64:5:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/scripts/ci/versiontool.py#L78'>scripts/ci/versiontool.py:78:5:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/scripts/codemods/base.py#L47'>scripts/codemods/base.py:47:13:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/test_bot/cogs/injections.py#L62'>test_bot/cogs/injections.py:62:41:</a> B008 Do not perform function call `commands.Param` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/tests/ext/commands/test_base_core.py#L102'>tests/ext/commands/test_base_core.py:102:9:</a> S101 Use of `assert` detected
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/tests/ext/commands/test_base_core.py#L28'>tests/ext/commands/test_base_core.py:28:13:</a> S101 Use of `assert` detected
... 115 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+46555 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/libs/__init__.py#L1'>airflow/example_dags/libs/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/libs/helper.py#L1'>airflow/example_dags/libs/helper.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/libs/helper.py#L21'>airflow/example_dags/libs/helper.py:21:5:</a> D103 Missing docstring in public function
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/__init__.py#L1'>airflow/example_dags/plugins/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/decreasing_priority_weight_strategy.py#L1'>airflow/example_dags/plugins/decreasing_priority_weight_strategy.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/decreasing_priority_weight_strategy.py#L32'>airflow/example_dags/plugins/decreasing_priority_weight_strategy.py:32:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/decreasing_priority_weight_strategy.py#L36'>airflow/example_dags/plugins/decreasing_priority_weight_strategy.py:36:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when dag run state changes to SUCCESS."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L128'>airflow/example_dags/plugins/event_listener.py:128:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when dag run state changes to FAILED."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L144'>airflow/example_dags/plugins/event_listener.py:144:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when dag run state changes to RUNNING."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L162'>airflow/example_dags/plugins/event_listener.py:162:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L1'>airflow/example_dags/plugins/event_listener.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when task state changes to RUNNING."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L33'>airflow/example_dags/plugins/event_listener.py:33:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when task state changes to SUCCESS."
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L68'>airflow/example_dags/plugins/event_listener.py:68:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D212 [*] Multi-line docstring summary should start at the first line
... 1886 additional changes omitted for rule D212
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D401 First line of docstring should be in imperative mood: "This method is called when task state changes to FAILED."
... 398 additional changes omitted for rule D401
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/event_listener.py#L95'>airflow/example_dags/plugins/event_listener.py:95:5:</a> D404 First word of the docstring should not be "This"
... 81 additional changes omitted for rule D404
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/listener_plugin.py#L1'>airflow/example_dags/plugins/listener_plugin.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/listener_plugin.py#L24'>airflow/example_dags/plugins/listener_plugin.py:24:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L45'>airflow/example_dags/plugins/workday.py:45:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L46'>airflow/example_dags/plugins/workday.py:46:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L60'>airflow/example_dags/plugins/workday.py:60:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L69'>airflow/example_dags/plugins/workday.py:69:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/plugins/workday.py#L99'>airflow/example_dags/plugins/workday.py:99:7:</a> D101 Missing docstring in public class
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/example_dags/subdags/__init__.py#L1'>airflow/example_dags/subdags/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/arangodb/example_dags/__init__.py#L1'>airflow/providers/arangodb/example_dags/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/arangodb/example_dags/example_arangodb.py#L1'>airflow/providers/arangodb/example_dags/example_arangodb.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/__init__.py#L1'>airflow/providers/google/cloud/example_dags/__init__.py:1:1:</a> D104 Missing docstring in public package
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py#L18'>airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py:18:1:</a> D200 One-line docstring should fit on one line
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/example_looker.py#L18'>airflow/providers/google/cloud/example_dags/example_looker.py:18:1:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/apache/airflow/blob/9619536e6f1f5737d56d2ef761c2e4467f17cd4e/airflow/providers/google/cloud/example_dags/example_presto_to_gcs.py#L18'>airflow/providers/google/cloud/example_dags/example_presto_to_gcs.py:18:1:</a> D200 One-line docstring should fit on one line
... 46509 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+208 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/config_from_2014.py#L40'>securedrop/tests/config_from_2014.py:40:22:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/journalist.pid"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/config_from_2014.py#L41'>securedrop/tests/config_from_2014.py:41:18:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/source.pid"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/config_from_2014.py#L70'>securedrop/tests/config_from_2014.py:70:23:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/securedrop"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/conftest.py#L110'>securedrop/tests/conftest.py:110:37:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/sd-tests/conftest-"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/conftest.py#L310'>securedrop/tests/conftest.py:310:33:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/securedrop_test_worker.pid"
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/conftest.py#L328'>securedrop/tests/conftest.py:328:28:</a> S108 Probable insecure usage of temporary file or directory: "/tmp/test_rqworker.log"
... 6 additional changes omitted for rule S108
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L106'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:106:9:</a> S101 Use of `assert` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L122'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:122:9:</a> S101 Use of `assert` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L148'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:148:17:</a> S101 Use of `assert` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/functional/app_navigators/journalist_app_nav.py#L172'>securedrop/tests/functional/app_navigators/journalist_app_nav.py:172:9:</a> S101 Use of `assert` detected
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L27'>ibis/backends/tests/base.py:27:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L27'>ibis/backends/tests/base.py:27:5:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L337'>ibis/backends/tests/base.py:337:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L81'>ibis/backends/tests/base.py:81:9:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/backends/tests/base.py#L81'>ibis/backends/tests/base.py:81:9:</a> D212 [*] Multi-line docstring summary should start at the first line
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/tests/util.py#L25'>ibis/tests/util.py:25:5:</a> D205 1 blank line required between summary line and description
+ <a href='https://github.com/ibis-project/ibis/blob/01b521cb26c8984d8f9ee968e894673eba3f2e6f/ibis/tests/util.py#L25'>ibis/tests/util.py:25:5:</a> D209 [*] Multi-line docstring closing quotes should be on a separate line
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+388 -22 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/12bb6284c49c4404930577082c4d4279854350c9/dev/proto_to_graphql/autogeneration_utils.py#L35'>dev/proto_to_graphql/autogeneration_utils.py:35:5:</a> T201 `print` found
- docs/source/deep-learning/pytorch/quickstart/pytorch_quickstart.ipynb:cell 29:19:25: E226 [*] Missing whitespace around arithmetic operator
- docs/source/llms/llm-evaluate/notebooks/question-answering-evaluation.ipynb:cell 29:1:1: W391 [*] Extra newline at end of file
- docs/source/llms/llm-evaluate/notebooks/rag-evaluation.ipynb:cell 19:1:1: W391 [*] Extra newline at end of file
- docs/source/llms/openai/notebooks/openai-code-helper.ipynb:cell 28:13:21: C419 Unnecessary list comprehension
- docs/source/llms/rag/notebooks/question-generation-retrieval-evaluation.ipynb:cell 25:6:15: E226 [*] Missing whitespace around arithmetic operator
- docs/source/llms/rag/notebooks/retriever-evaluation-tutorial.ipynb:cell 50:1:1: W391 [*] Extra newline at end of file
- docs/source/llms/transformers/tutorials/text-generation/text-generation.ipynb:cell 18:14:34: E226 [*] Missing whitespace around arithmetic operator
- docs/source/traditional-ml/hyperparameter-tuning-with-child-runs/notebooks/parent-child-runs.ipynb:cell 15:1:1: W391 [*] Extra newline at end of file
- docs/source/traditional-ml/serving-multiple-models-with-pyfunc/notebooks/MME_Tutorial.ipynb:cell 25:1:1: W391 [*] Extra newline at end of file
... 400 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (68 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S101 | 19886 | 19886 | 0 | 0 | 0 |
| D102 | 11929 | 11929 | 0 | 0 | 0 |
| D103 | 2741 | 2741 | 0 | 0 | 0 |
| D101 | 2242 | 2242 | 0 | 0 | 0 |
| D212 | 1893 | 1893 | 0 | 0 | 0 |
| D100 | 1694 | 1694 | 0 | 0 | 0 |
| D400 | 1514 | 1514 | 0 | 0 | 0 |
| D415 | 1509 | 1509 | 0 | 0 | 0 |
| S113 | 1250 | 1250 | 0 | 0 | 0 |
| D200 | 930 | 930 | 0 | 0 | 0 |
| D205 | 853 | 853 | 0 | 0 | 0 |
| D104 | 693 | 693 | 0 | 0 | 0 |
| NPY002 | 476 | 476 | 0 | 0 | 0 |
| D401 | 403 | 403 | 0 | 0 | 0 |
| T201 | 325 | 325 | 0 | 0 | 0 |
| E241 | 268 | 268 | 0 | 0 | 0 |
| D202 | 247 | 247 | 0 | 0 | 0 |
| D107 | 122 | 122 | 0 | 0 | 0 |
| FLY002 | 122 | 122 | 0 | 0 | 0 |
| D404 | 86 | 86 | 0 | 0 | 0 |
| S311 | 80 | 80 | 0 | 0 | 0 |
| E272 | 79 | 79 | 0 | 0 | 0 |
| E501 | 65 | 65 | 0 | 0 | 0 |
| D105 | 62 | 62 | 0 | 0 | 0 |
| RUF012 | 54 | 54 | 0 | 0 | 0 |
| D403 | 47 | 47 | 0 | 0 | 0 |
| TRY002 | 37 | 37 | 0 | 0 | 0 |
| D209 | 36 | 36 | 0 | 0 | 0 |
| B028 | 35 | 35 | 0 | 0 | 0 |
| A001 | 33 | 0 | 33 | 0 | 0 |
| N806 | 16 | 16 | 0 | 0 | 0 |
| B018 | 16 | 0 | 16 | 0 | 0 |
| TID253 | 15 | 15 | 0 | 0 | 0 |
| TID252 | 15 | 15 | 0 | 0 | 0 |
| INP001 | 15 | 15 | 0 | 0 | 0 |
| N802 | 15 | 15 | 0 | 0 | 0 |
| TID251 | 13 | 13 | 0 | 0 | 0 |
| S105 | 13 | 13 | 0 | 0 | 0 |
| S108 | 11 | 11 | 0 | 0 | 0 |
| UP031 | 11 | 7 | 4 | 0 | 0 |
| W391 | 11 | 0 | 11 | 0 | 0 |
| A002 | 11 | 0 | 11 | 0 | 0 |
| PLW0642 | 10 | 5 | 5 | 0 | 0 |
| T203 | 9 | 9 | 0 | 0 | 0 |
| RET504 | 9 | 9 | 0 | 0 | 0 |
| PLC1901 | 7 | 7 | 0 | 0 | 0 |
| RUF003 | 6 | 3 | 3 | 0 | 0 |
| B011 | 6 | 6 | 0 | 0 | 0 |
| N803 | 6 | 6 | 0 | 0 | 0 |
| B008 | 5 | 5 | 0 | 0 | 0 |
| PERF203 | 5 | 5 | 0 | 0 | 0 |
| D407 | 4 | 4 | 0 | 0 | 0 |
| E221 | 4 | 4 | 0 | 0 | 0 |
| E226 | 4 | 0 | 4 | 0 | 0 |
| D413 | 3 | 3 | 0 | 0 | 0 |
| F841 | 3 | 2 | 1 | 0 | 0 |
| PT018 | 2 | 2 | 0 | 0 | 0 |
| D106 | 2 | 2 | 0 | 0 | 0 |
| D419 | 2 | 2 | 0 | 0 | 0 |
| PIE790 | 2 | 2 | 0 | 0 | 0 |
| N812 | 2 | 2 | 0 | 0 | 0 |
| E703 | 2 | 0 | 2 | 0 | 0 |
| D406 | 1 | 1 | 0 | 0 | 0 |
| D412 | 1 | 1 | 0 | 0 | 0 |
| RUF018 | 1 | 1 | 0 | 0 | 0 |
| C419 | 1 | 0 | 1 | 0 | 0 |
| E266 | 1 | 0 | 1 | 0 | 0 |
| E265 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-25 09:18_

---

_Comment by @charliermarsh on 2024-06-25 10:49_

It looks like this is going to cause churn for a lot of projects (based on the ecosystem checks) so we should double-confirm that we want this behavior.

---

_Comment by @JP-Ellis on 2024-06-25 11:02_

While I am open to suggestions for this PR, I do think that it would be good to follow a widely accepted convention such as the [`gitignore` specification](https://git-scm.com/docs/gitignore#_pattern_format). Specifically the following from the specification:

> - An asterisk `*` matches anything except a slash. The character `?` matches any one character except `/`. The range notation, e.g. `[a-zA-Z]`, can be used to match one of the characters in a range. See fnmatch(3) and the FNM_PATHNAME flag for a more detailed description.
>
> Two consecutive asterisks (`**`) in patterns matched against full pathname may have special meaning:
>
> - A leading `**` followed by a slash means match in all directories. For example, `**/foo` matches file or directory `foo` anywhere, the same as pattern `foo`. `**/foo/bar` matches file or directory `bar` anywhere that is directly under directory `foo`.
> - A trailing `/**` matches everything inside. For example, `abc/**` matches all files inside directory `abc`, relative to the location of the `.gitignore` file, with infinite depth.
> - A slash followed by two consecutive asterisks then a slash matches zero or more directories. For example, `a/**/b` matches `a/b`, `a/x/b`, `a/x/y/b` and so on.
> - Other consecutive asterisks are considered regular asterisks and will match according to the previous rules.

To help with the transition, I might suggest two options:

- An option to migrate configurations from `0.4` to `0.5`. In most cases, it would suffice to replace all `*` to `**` (though I suspect there will be edge cases).
- Add a flag to opt in/out of the new way, allowing both behaviours temporarily.

Having said that, this would make the PR significantly more difficult for something which I suspect most developers will straightforwardly understand and adapt.

---

_Comment by @MichaReiser on 2024-06-25 11:18_

I'm generally in favor of changing our semantics to match `gitignore` because it is a well-understood format and it makes copying patterns from `gitignore` to the ruff settings easier. It should also fix https://github.com/astral-sh/ruff/issues/8267

My main concern is that Ruff isn't fully compliant to gitignore even after this change. It still has the following inherited flake8/black behavior:

> We have kind of a weird thing going on, which I think I followed from Flake8 or Black. When given a pattern like foo*.py, we match against both the basename and the absolute path (when testing to exclude a given file or directory). [source](https://github.com/astral-sh/ruff/issues/6262#issuecomment-1662907046)


I think we should remove this behavior as well. 

Having said that, I don't think it's feasible for us to ship this at the moment considering the amount of churn it creates. I think we need a way to ease migration for users by using a cargo like editions concept or provididing a migration tool that automatically rewrites/upgrades the configuration

Edit: My biggest concern isn't the churn of users getting spammed with lint-errors for files that are no longer ignored. I'm worried about the *silent* exclusion of files that no longer match the `include` patterns. 



---

_Removed from milestone `v0.5.0` by @MichaReiser on 2024-06-25 11:42_

---

_Comment by @MichaReiser on 2024-06-26 08:09_

I'll close this PR because there's no clear path of what needs to be done to merge it. We can continue the discussion in the linked issue.

---

_Closed by @MichaReiser on 2024-06-26 08:09_

---

_Branch deleted on 2024-06-26 08:20_

---

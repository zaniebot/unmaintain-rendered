```yaml
number: 9714
title: Add handling for rules that are both deprecated and redirected
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
base: release/0.2.0
head: zb/pygrep
created_at: 2024-01-30T19:12:52Z
updated_at: 2024-01-31T17:22:36Z
url: https://github.com/astral-sh/ruff/pull/9714
synced_at: 2026-01-12T15:55:30Z
```

# Add handling for rules that are both deprecated and redirected

---

_@zanieb_

Follow up to #9689 adds tooling for warning on rules that are deprecated because we replaced them with another rule.

Previously we would display no warnings for deprecated redirected rules because the redirect took place before the warnings could be displayed.

As with the other changes, this is immediately used for testing. Deprecates all of the `PGH` rules and adds redirects where relevant.

Closes https://github.com/astral-sh/ruff/issues/8931

---

_Comment by @codspeed-hq[bot] on 2024-01-30 19:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zb/pygrep)

### Merging #9714 will **not alter performance**

<sub>Comparing <code>zb/pygrep</code> (b75864c) with <code>release/0.2.0</code> (435d025)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@charliermarsh reviewed on 2024-01-30 19:48_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:710 on 2024-01-30 19:48_

Should we deprecate these despite not having replacements for them?

---

_@charliermarsh reviewed on 2024-01-30 19:49_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:923 on 2024-01-30 19:49_

Can this be a `match` instead of mutually-exclusive `if`-`let`? Or am I misreading?

---

_@charliermarsh approved on 2024-01-30 19:49_

Really slick!

---

_@charliermarsh reviewed on 2024-01-30 19:49_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:1074 on 2024-01-30 19:49_

I would typically just do `&format!(...)` here instead of `as_str`.

---

_@charliermarsh reviewed on 2024-01-30 19:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:710 on 2024-01-30 19:50_

I dislike the category but the specific rules seem... ok.

---

_Review comment by @zanieb on `crates/ruff_linter/src/codes.rs`:710 on 2024-01-30 19:50_

I thought we had decided that already in #8931? I guess I don't see any problems with these other rules though?

---

_@zanieb reviewed on 2024-01-30 19:50_

---

_@zanieb reviewed on 2024-01-30 19:52_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:923 on 2024-01-30 19:52_

Yes I guess if we want to be explicit about the cases we are ignoring. I'm going to address this in the next commit in the stack though, if that's okay?

---

_Comment by @zanieb on 2024-01-30 20:26_

Eh we have a problem here where redirected rules are being treated as stable when in preview... I need to investigate further.

---

_@charliermarsh reviewed on 2024-01-30 20:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/codes.rs`:710 on 2024-01-30 20:29_

I still want to deprecate the category since it's unfocused, but I think we'd need to find new homes for the others. I'd vote just deprecate the two for now.

---

_@zanieb reviewed on 2024-01-30 20:48_

---

_Review comment by @zanieb on `crates/ruff_linter/src/codes.rs`:710 on 2024-01-30 20:48_

Okay works for me

---

_Comment by @github-actions[bot] on 2024-01-30 22:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+27 -1 violations, +0 -0 fixes in 4 projects; 1 project error; 38 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/utils.py#L1161'>disnake/utils.py:1161:21:</a> PGH001 No builtin `eval()` allowed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/28150e62d0a6795f2248931a3ff3923676f70a7e/Packs/Malwr/Integrations/Malwr/Malwr.py#L96'>Packs/Malwr/Integrations/Malwr/Malwr.py:96:18:</a> PGH001 No builtin `eval()` allowed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/4fb0aad941029b6fbc47342297b47305f5366d1c/ibis/common/typing.py#L206'>ibis/common/typing.py:206:12:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/ibis-project/ibis/blob/4fb0aad941029b6fbc47342297b47305f5366d1c/ibis/common/typing.py#L206'>ibis/common/typing.py:206:69:</a> RUF100 Unused `noqa` directive (non-enabled: `S307`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/computation/test_eval.py#L1110'>pandas/tests/computation/test_eval.py:1110:15:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/computation/test_eval.py#L1126'>pandas/tests/computation/test_eval.py:1126:15:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/computation/test_eval.py#L298'>pandas/tests/computation/test_eval.py:298:28:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/computation/test_eval.py#L760'>pandas/tests/computation/test_eval.py:760:36:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/methods/test_sample.py#L174'>pandas/tests/frame/methods/test_sample.py:174:47:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/methods/test_sample.py#L175'>pandas/tests/frame/methods/test_sample.py:175:66:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/test_query_eval.py#L117'>pandas/tests/frame/test_query_eval.py:117:20:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/frame/test_query_eval.py#L120'>pandas/tests/frame/test_query_eval.py:120:18:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/base_class/test_formats.py#L16'>pandas/tests/indexes/base_class/test_formats.py:16:15:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/categorical/test_category.py#L203'>pandas/tests/indexes/categorical/test_category.py:203:31:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/multi/test_formats.py#L64'>pandas/tests/indexes/multi/test_formats.py:64:9:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/numeric/test_numeric.py#L37'>pandas/tests/indexes/numeric/test_numeric.py:37:31:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/ranges/test_range.py#L398'>pandas/tests/indexes/ranges/test_range.py:398:31:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/ranges/test_range.py#L63'>pandas/tests/indexes/ranges/test_range.py:63:18:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/ranges/test_range.py#L71'>pandas/tests/indexes/ranges/test_range.py:71:18:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/indexes/test_old_base.py#L239'>pandas/tests/indexes/test_old_base.py:239:31:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_constructors.py#L664'>pandas/tests/scalar/timestamp/test_constructors.py:664:26:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_constructors.py#L672'>pandas/tests/scalar/timestamp/test_constructors.py:672:26:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_constructors.py#L681'>pandas/tests/scalar/timestamp/test_constructors.py:681:26:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_constructors.py#L689'>pandas/tests/scalar/timestamp/test_constructors.py:689:26:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_formats.py#L110'>pandas/tests/scalar/timestamp/test_formats.py:110:29:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_formats.py#L116'>pandas/tests/scalar/timestamp/test_formats.py:116:27:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_formats.py#L126'>pandas/tests/scalar/timestamp/test_formats.py:126:40:</a> PGH001 No builtin `eval()` allowed
+ <a href='https://github.com/pandas-dev/pandas/blob/9b95e4596c5d47b5dcc3cbedf143023475930595/pandas/tests/scalar/timestamp/test_formats.py#L163'>pandas/tests/scalar/timestamp/test_formats.py:163:20:</a> PGH001 No builtin `eval()` allowed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB131
	- FURB113
	- FURB132
```

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PGH001 | 27 | 26 | 1 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -34464 violations, +0 -0 fixes in 6 projects; 4 project errors; 33 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -23410 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/__init__.py#L59'>airflow/__init__.py:59:67:</a> PGH003 Use specific rule codes when ignoring type issues
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/__init__.py#L46'>airflow/api/__init__.py:46:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/auth/backend/kerberos_auth.py#L55'>airflow/api/auth/backend/kerberos_auth.py:55:85:</a> PGH003 Use specific rule codes when ignoring type issues
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/auth/backend/kerberos_auth.py#L73'>airflow/api/auth/backend/kerberos_auth.py:73:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/client/api_client.py#L27'>airflow/api/client/api_client.py:27:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/client/api_client.py#L33'>airflow/api/client/api_client.py:33:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/client/api_client.py#L45'>airflow/api/client/api_client.py:45:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/client/api_client.py#L52'>airflow/api/client/api_client.py:52:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/client/api_client.py#L59'>airflow/api/client/api_client.py:59:19:</a> ANN101 Missing type annotation for `self` in method
... 22044 additional changes omitted for rule ANN101
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/client/local_client.py#L80'>airflow/api/client/local_client.py:80:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/common/experimental/delete_dag.py#L23'>airflow/api/common/experimental/delete_dag.py:23:46:</a> PGH004 Use specific rule codes when using `noqa`
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/common/experimental/get_code.py#L41'>airflow/api/common/experimental/get_code.py:41:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/common/experimental/mark_tasks.py#L23'>airflow/api/common/experimental/mark_tasks.py:23:46:</a> PGH004 Use specific rule codes when using `noqa`
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/common/experimental/pool.py#L65'>airflow/api/common/experimental/pool.py:65:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api/common/experimental/trigger_dag.py#L23'>airflow/api/common/experimental/trigger_dag.py:23:47:</a> PGH004 Use specific rule codes when using `noqa`
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api_connexion/endpoints/connection_endpoint.py#L131'>airflow/api_connexion/endpoints/connection_endpoint.py:131:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api_connexion/endpoints/connection_endpoint.py#L164'>airflow/api_connexion/endpoints/connection_endpoint.py:164:9:</a> TRY200 Use `raise from` to specify exception cause
... 357 additional changes omitted for rule TRY200
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/api_connexion/schemas/dag_schema.py#L107'>airflow/api_connexion/schemas/dag_schema.py:107:55:</a> PGH003 Use specific rule codes when ignoring type issues
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/callbacks/callback_requests.py#L57'>airflow/callbacks/callback_requests.py:57:19:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/callbacks/callback_requests.py#L95'>airflow/callbacks/callback_requests.py:95:19:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/cli/commands/standalone_command.py#L51'>airflow/cli/commands/standalone_command.py:51:20:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/compat/functools.py#L31'>airflow/compat/functools.py:31:40:</a> PGH003 Use specific rule codes when ignoring type issues
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/configuration.py#L1592'>airflow/configuration.py:1592:61:</a> PGH003 Use specific rule codes when ignoring type issues
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/dag_processing/manager.py#L1301'>airflow/dag_processing/manager.py:1301:93:</a> PGH003 Use specific rule codes when ignoring type issues
... 421 additional changes omitted for rule PGH003
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/dag_processing/manager.py#L496'>airflow/dag_processing/manager.py:496:9:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/dag_processing/processor.py#L420'>airflow/dag_processing/processor.py:420:21:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/dag_processing/processor.py#L795'>airflow/dag_processing/processor.py:795:21:</a> ANN102 Missing type annotation for `cls` in classmethod
... 478 additional changes omitted for rule ANN102
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/hooks/dbapi.py#L25'>airflow/hooks/dbapi.py:25:25:</a> PGH004 Use specific rule codes when using `noqa`
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/hooks/dbapi.py#L26'>airflow/hooks/dbapi.py:26:17:</a> PGH004 Use specific rule codes when using `noqa`
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/airflow/macros/__init__.py#L20'>airflow/macros/__init__.py:20:14:</a> PGH004 Use specific rule codes when using `noqa`
... 44 additional changes omitted for rule PGH004
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/dev/stats/get_important_pr_candidates.py#L131'>dev/stats/get_important_pr_candidates.py:131:32:</a> PGH001 No builtin `eval()` allowed
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L1135'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:1135:16:</a> PGH005 Non-existent mock method: `called_once`
- <a href='https://github.com/apache/airflow/blob/ba053f79edab54366208aad9d88877a95b789eca/tests/providers/amazon/aws/operators/test_eks.py#L568'>tests/providers/amazon/aws/operators/test_eks.py:568:9:</a> PGH005 Mock method should be called: `assert_called_once`
... 23377 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -4146 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L99'>docs/bokeh/docserver.py:99:39:</a> PGH003 Use specific rule codes when ignoring type issues
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L32'>examples/output/apis/autoload_static.py:32:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L34'>examples/output/apis/autoload_static.py:34:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L41'>examples/output/apis/autoload_static.py:41:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L43'>examples/output/apis/autoload_static.py:43:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/glyphs.py#L8'>examples/plotting/glyphs.py:8:5:</a> PGH004 Use specific rule codes when using `noqa`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L15'>examples/server/api/tornado_embed.py:15:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L27'>examples/server/app/server_auth/auth.py:27:13:</a> ANN101 Missing type annotation for `self` in method
... 4003 additional changes omitted for rule ANN101
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/util.py#L32'>release/util.py:32:38:</a> PGH003 Use specific rule codes when ignoring type issues
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L159'>setup.py:159:22:</a> PGH003 Use specific rule codes when ignoring type issues
... 4136 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/4fb0aad941029b6fbc47342297b47305f5366d1c/ibis/common/typing.py#L206'>ibis/common/typing.py:206:69:</a> RUF100 Unused `noqa` directive (non-enabled: `S307`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/4858eee3e2d02069343ebf581b20d82603b7a94d/_version_helper.py#L13'>_version_helper.py:13:50:</a> PGH004 Use specific rule codes when using `noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/83bdf5c7be48c4af715bcd745a99b6014aae7ba5/pytestgeventwrapper.py#L1'>pytestgeventwrapper.py:1:48:</a> PGH004 Use specific rule codes when using `noqa`
- <a href='https://github.com/rotki/rotki/blob/83bdf5c7be48c4af715bcd745a99b6014aae7ba5/pytestgeventwrapper.py#L2'>pytestgeventwrapper.py:2:34:</a> PGH004 Use specific rule codes when using `noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -6905 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/analytics/lib/counts.py#L105'>analytics/lib/counts.py:105:9:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/analytics/lib/counts.py#L112'>analytics/lib/counts.py:112:26:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/analytics/lib/counts.py#L49'>analytics/lib/counts.py:49:24:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/analytics/lib/counts.py#L55'>analytics/lib/counts.py:55:9:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/analytics/lib/counts.py#L73'>analytics/lib/counts.py:73:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/analytics/lib/counts.py#L76'>analytics/lib/counts.py:76:30:</a> ANN101 Missing type annotation for `self` in method
... 6614 additional changes omitted for rule ANN101
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/analytics/views/stats.py#L156'>analytics/views/stats.py:156:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/confirmation/models.py#L102'>confirmation/models.py:102:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/corporate/lib/registration.py#L115'>corporate/lib/registration.py:115:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/10a45fc479ffa72c3c37132c601b04e10c77bbce/corporate/lib/remote_billing_util.py#L124'>corporate/lib/remote_billing_util.py:124:9:</a> TRY200 Use `raise from` to specify exception cause
... 6895 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Selection of deprecated rules is not allowed when preview is enabled. Remove selection of:
	- PGH002 (use `G010` instead)
	- PGH001 (use `S307` instead)
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'mccabe' -> 'lint.mccabe'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Selection of deprecated rule `TRY200` is not allowed when preview mode is enabled.
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'isort' -> 'lint.isort'


ruff failed
  Cause: Selection of deprecated rule `PGH004` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview mode is enabled.
```

</p>
</details>
<details><summary>Changes by rule (8 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN101 | 32676 | 0 | 32676 | 0 | 0 |
| TRY200 | 636 | 0 | 636 | 0 | 0 |
| ANN102 | 612 | 0 | 612 | 0 | 0 |
| PGH003 | 444 | 0 | 444 | 0 | 0 |
| PGH004 | 55 | 0 | 55 | 0 | 0 |
| PGH005 | 40 | 0 | 40 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |
| PGH001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Closed by @zanieb on 2024-01-31 17:22_

---

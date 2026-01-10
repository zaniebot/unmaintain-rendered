```yaml
number: 9755
title: "Redirect `TRY200` to `B904`"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
assignees: []
merged: true
base: release/0.2.0
head: zb/try200
created_at: 2024-02-01T15:47:02Z
updated_at: 2024-02-01T17:01:09Z
url: https://github.com/astral-sh/ruff/pull/9755
synced_at: 2026-01-10T22:57:09Z
```

# Redirect `TRY200` to `B904`

---

_Pull request opened by @zanieb on 2024-02-01 15:47_

Follow-up to #9754 and #9689. Alternative to #9714.

Marks `TRY200` as removed and redirects to `B904` instead of marking as deprecated and suggesting `B904` instead.

---

_Label `rule` added by @zanieb on 2024-02-01 15:48_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-01 15:48_

---

_Renamed from "Redirect TRY200 to B904" to "Redirect `TRY200` to `B904`" by @zanieb on 2024-02-01 15:55_

---

_@charliermarsh approved on 2024-02-01 15:58_

---

_Comment by @github-actions[bot] on 2024-02-01 16:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -636 violations, +0 -0 fixes in 3 projects; 1 project error; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -362 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api/__init__.py#L46'>airflow/api/__init__.py:46:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api/client/local_client.py#L80'>airflow/api/client/local_client.py:80:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api/common/experimental/get_code.py#L41'>airflow/api/common/experimental/get_code.py:41:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api/common/experimental/pool.py#L65'>airflow/api/common/experimental/pool.py:65:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/connection_endpoint.py#L131'>airflow/api_connexion/endpoints/connection_endpoint.py:131:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/connection_endpoint.py#L164'>airflow/api_connexion/endpoints/connection_endpoint.py:164:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/connection_endpoint.py#L169'>airflow/api_connexion/endpoints/connection_endpoint.py:169:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/connection_endpoint.py#L206'>airflow/api_connexion/endpoints/connection_endpoint.py:206:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_endpoint.py#L138'>airflow/api_connexion/endpoints/dag_endpoint.py:138:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_endpoint.py#L148'>airflow/api_connexion/endpoints/dag_endpoint.py:148:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_endpoint.py#L171'>airflow/api_connexion/endpoints/dag_endpoint.py:171:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_endpoint.py#L220'>airflow/api_connexion/endpoints/dag_endpoint.py:220:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_endpoint.py#L222'>airflow/api_connexion/endpoints/dag_endpoint.py:222:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_endpoint.py#L65'>airflow/api_connexion/endpoints/dag_endpoint.py:65:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_endpoint.py#L87'>airflow/api_connexion/endpoints/dag_endpoint.py:87:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L113'>airflow/api_connexion/endpoints/dag_run_endpoint.py:113:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L262'>airflow/api_connexion/endpoints/dag_run_endpoint.py:262:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L273'>airflow/api_connexion/endpoints/dag_run_endpoint.py:273:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L325'>airflow/api_connexion/endpoints/dag_run_endpoint.py:325:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L368'>airflow/api_connexion/endpoints/dag_run_endpoint.py:368:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L394'>airflow/api_connexion/endpoints/dag_run_endpoint.py:394:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L421'>airflow/api_connexion/endpoints/dag_run_endpoint.py:421:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_run_endpoint.py#L468'>airflow/api_connexion/endpoints/dag_run_endpoint.py:468:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/dag_source_endpoint.py#L62'>airflow/api_connexion/endpoints/dag_source_endpoint.py:62:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/extra_link_endpoint.py#L58'>airflow/api_connexion/endpoints/extra_link_endpoint.py:58:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/log_endpoint.py#L64'>airflow/api_connexion/endpoints/log_endpoint.py:64:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/pool_endpoint.py#L108'>airflow/api_connexion/endpoints/pool_endpoint.py:108:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/airflow/api_connexion/endpoints/pool_endpoint.py#L121'>airflow/api_connexion/endpoints/pool_endpoint.py:121:13:</a> TRY200 Use `raise from` to specify exception cause
... 334 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L163'>src/bokeh/colors/util.py:163:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/constraints.py#L91'>src/bokeh/core/property/constraints.py:91:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/datetime.py#L79'>src/bokeh/core/property/datetime.py:79:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/json.py#L68'>src/bokeh/core/property/json.py:68:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/decorators.py#L85'>src/bokeh/core/validation/decorators.py:85:21:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L354'>src/bokeh/document/callbacks.py:354:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L223'>src/bokeh/document/document.py:223:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L917'>src/bokeh/models/plots.py:917:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L266'>src/bokeh/models/sources.py:266:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L62'>src/bokeh/plotting/_graph.py:62:13:</a> TRY200 Use `raise from` to specify exception cause
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -237 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/analytics/views/stats.py#L156'>analytics/views/stats.py:156:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/confirmation/models.py#L102'>confirmation/models.py:102:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/registration.py#L115'>corporate/lib/registration.py:115:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/remote_billing_util.py#L124'>corporate/lib/remote_billing_util.py:124:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/remote_billing_util.py#L166'>corporate/lib/remote_billing_util.py:166:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L1019'>corporate/lib/stripe.py:1019:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L185'>corporate/lib/stripe.py:185:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L2743'>corporate/lib/stripe.py:2743:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L4440'>corporate/lib/stripe.py:4440:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L4442'>corporate/lib/stripe.py:4442:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L489'>corporate/lib/stripe.py:489:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L498'>corporate/lib/stripe.py:498:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/lib/stripe.py#L502'>corporate/lib/stripe.py:502:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/tests/test_stripe.py#L1124'>corporate/tests/test_stripe.py:1124:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/tests/test_stripe.py#L1337'>corporate/tests/test_stripe.py:1337:17:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/tests/test_stripe.py#L814'>corporate/tests/test_stripe.py:814:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/views/remote_billing_page.py#L139'>corporate/views/remote_billing_page.py:139:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/0e6f013e03a2033d0660b841e1050f6f7fe9be3d/corporate/views/remote_billing_page.py#L141'>corporate/views/remote_billing_page.py:141:9:</a> TRY200 Use `raise from` to specify exception cause
... 219 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
warning: `RUF011` has been remapped to `B035`.
warning: `TCH006` has been remapped to `TCH010`.
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB131
	- FURB113
	- FURB132
```

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TRY200 | 636 | 0 | 636 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'mccabe' -> 'lint.mccabe'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Selection of deprecated rule `TRY200` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview is enabled.
```

</p>
</details>




---

_Merged by @zanieb on 2024-02-01 16:52_

---

_Closed by @zanieb on 2024-02-01 16:52_

---

_Branch deleted on 2024-02-01 16:52_

---

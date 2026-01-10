```yaml
number: 12050
title: "[`flake8-simplify`] Stabilize detection of Yoda conditions for \"constant\" collections (`SIM300`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: charlie/sim300
created_at: 2024-06-26T15:38:35Z
updated_at: 2024-06-27T10:36:39Z
url: https://github.com/astral-sh/ruff/pull/12050
synced_at: 2026-01-10T21:56:00Z
```

# [`flake8-simplify`] Stabilize detection of Yoda conditions for "constant" collections (`SIM300`)

---

_Pull request opened by @charliermarsh on 2024-06-26 15:38_

## Summary

See: https://github.com/astral-sh/ruff/pull/9164.

We made a bunch of improvements to the rule, to handle "constant" collections (e.g., lists that consist of constants). This will lead to new violations, but I do think it's a better behavior and it's clearly stable.


---

_Label `rule` added by @charliermarsh on 2024-06-26 15:38_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-06-26 15:38_

---

_Comment by @charliermarsh on 2024-06-26 15:38_

A bit more controversial than the others because the scope is larger (hopefully ecosystem reflects that).

---

_@AlexWaygood reviewed on 2024-06-26 16:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/yoda_conditions.rs`:98 on 2024-06-26 16:08_

Should this now be `impl From<&Expr> for ConstantLikelihood {}`? (https://github.com/astral-sh/ruff/pull/9164#discussion_r1433002869)

---

_Comment by @AlexWaygood on 2024-06-26 16:10_

I pushed to your branch to fix some trivial clippy issues, but looks like some snapshots are out of date as well

---

_Comment by @github-actions[bot] on 2024-06-26 16:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2403 -1905 violations, +0 -0 fixes in 6 projects; 44 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1827 -1422 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/arangodb/sensors/arangodb.py#L54'>airflow/providers/arangodb/sensors/arangodb.py:54:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/arangodb/sensors/arangodb.py#L54'>airflow/providers/arangodb/sensors/arangodb.py:54:16:</a> SIM300 [*] Yoda conditions are discouraged, use `records != 0` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L118'>airflow/providers/google/cloud/sensors/dataproc.py:118:14:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L118'>airflow/providers/google/cloud/sensors/dataproc.py:118:14:</a> SIM300 [*] Yoda conditions are discouraged, use `state == JobStatus.State.DONE` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L121'>airflow/providers/google/cloud/sensors/dataproc.py:121:14:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L121'>airflow/providers/google/cloud/sensors/dataproc.py:121:14:</a> SIM300 [*] Yoda conditions are discouraged, use `state == JobStatus.State.ATTEMPT_FAILURE` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1169'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1169:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1169'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1169:17:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "amd"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1170'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1170:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1170'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1170:20:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "amd64"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1171'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1171:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1171'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1171:20:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "x64"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1172'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1172:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1172'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1172:20:</a> SIM300 [*] Yoda conditions are discouraged, use `label == "asf-runner"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:17:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "arm"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:43:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:43:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "arm64"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:71:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:71:</a> SIM300 [*] Yoda conditions are discouraged, use `label == "asf-arm"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L112'>dev/breeze/tests/test_packages.py:112:12:</a> SIM300 [*] Yoda condition detected
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L117'>dev/breeze/tests/test_packages.py:117:12:</a> SIM300 [*] Yoda condition detected
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L122'>dev/breeze/tests/test_packages.py:122:12:</a> SIM300 [*] Yoda condition detected
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L60'>docker_tests/test_prod_image.py:60:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L60'>docker_tests/test_prod_image.py:60:16:</a> SIM300 [*] Yoda conditions are discouraged, use `ctx.value.return_code == 2` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L66'>docker_tests/test_prod_image.py:66:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L66'>docker_tests/test_prod_image.py:66:16:</a> SIM300 [*] Yoda conditions are discouraged, use `ctx.value.return_code == 2` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L102'>helm_tests/airflow_aux/test_airflow_common.py:102:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L102'>helm_tests/airflow_aux/test_airflow_common.py:102:16:</a> SIM300 [*] Yoda conditions are discouraged, use `len(docs) == 3` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L167'>helm_tests/airflow_aux/test_airflow_common.py:167:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L167'>helm_tests/airflow_aux/test_airflow_common.py:167:16:</a> SIM300 [*] Yoda conditions are discouraged, use `len(k8s_objects) == 7` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L228'>helm_tests/airflow_aux/test_airflow_common.py:228:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L228'>helm_tests/airflow_aux/test_airflow_common.py:228:16:</a> SIM300 [*] Yoda conditions are discouraged, use `len(k8s_objects) == 12` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L236'>helm_tests/airflow_aux/test_airflow_common.py:236:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L236'>helm_tests/airflow_aux/test_airflow_common.py:236:20:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L244'>helm_tests/airflow_aux/test_airflow_common.py:244:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L244'>helm_tests/airflow_aux/test_airflow_common.py:244:20:</a> SIM300 [*] Yoda conditions are discouraged
... 3212 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+162 -149 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/stats/sinaplot.py#L42'>examples/topics/stats/sinaplot.py:42:19:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/stats/sinaplot.py#L42'>examples/topics/stats/sinaplot.py:42:19:</a> SIM300 [*] Yoda conditions are discouraged, use `month == df.MONTH` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L118'>src/bokeh/core/property/numeric.py:118:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L118'>src/bokeh/core/property/numeric.py:118:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value > 0` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L68'>src/bokeh/core/property/numeric.py:68:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L68'>src/bokeh/core/property/numeric.py:68:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value >= 0` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L80'>src/bokeh/core/property/numeric.py:80:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L80'>src/bokeh/core/property/numeric.py:80:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value > 0` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L99'>src/bokeh/core/property/numeric.py:99:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L99'>src/bokeh/core/property/numeric.py:99:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value >= 0` instead
... 301 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+413 -331 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L114'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:114:12:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L114'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:114:12:</a> SIM300 [*] Yoda conditions are discouraged, use `_action != Action.TEST_CONN` instead
+ <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L281'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:281:31:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L281'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:281:31:</a> SIM300 [*] Yoda conditions are discouraged, use `res_json['rescode'] == 0` instead
+ <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L314'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:314:33:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L314'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:314:33:</a> SIM300 [*] Yoda conditions are discouraged, use `res_json['rescode'] == 0` instead
+ <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L347'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:347:31:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L347'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:347:31:</a> SIM300 [*] Yoda conditions are discouraged, use `res_json['rescode'] == 0` instead
+ <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L381'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:381:33:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/demisto/content/blob/fe6be574c79146cc0090c98ee403d10f40746f23/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L381'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:381:33:</a> SIM300 [*] Yoda conditions are discouraged, use `res_json['rescode'] == 0` instead
... 734 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/model-bakers/model_bakery/blob/a9ae541e9775690d536ad814fe4360d85fff7ca7/tests/test_baker.py#L817'>tests/test_baker.py:817:16:</a> SIM300 [*] Yoda condition detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/1765904436c20241561602053d8cf137ec64ce6e/indico/web/rh_test.py#L85'>indico/web/rh_test.py:85:12:</a> SIM300 [*] Yoda conditions are discouraged, use `{} == DummyRH._CORS` instead
- <a href='https://github.com/indico/indico/blob/1765904436c20241561602053d8cf137ec64ce6e/indico/web/rh_test.py#L92'>indico/web/rh_test.py:92:12:</a> SIM300 [*] Yoda conditions are discouraged, use `{} == DummyRH._CORS` instead
- <a href='https://github.com/indico/indico/blob/1765904436c20241561602053d8cf137ec64ce6e/indico/web/rh_test.py#L99'>indico/web/rh_test.py:99:12:</a> SIM300 [*] Yoda conditions are discouraged, use `{'origins': 'localhost'} == DummyRH._CORS` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM300 | 4308 | 2403 | 1905 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1990 -1990 violations, +0 -0 fixes in 3 projects; 1 project error; 46 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1827 -1827 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/arangodb/sensors/arangodb.py#L54'>airflow/providers/arangodb/sensors/arangodb.py:54:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/arangodb/sensors/arangodb.py#L54'>airflow/providers/arangodb/sensors/arangodb.py:54:16:</a> SIM300 [*] Yoda conditions are discouraged, use `records != 0` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L118'>airflow/providers/google/cloud/sensors/dataproc.py:118:14:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L118'>airflow/providers/google/cloud/sensors/dataproc.py:118:14:</a> SIM300 [*] Yoda conditions are discouraged, use `state == JobStatus.State.DONE` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L121'>airflow/providers/google/cloud/sensors/dataproc.py:121:14:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/providers/google/cloud/sensors/dataproc.py#L121'>airflow/providers/google/cloud/sensors/dataproc.py:121:14:</a> SIM300 [*] Yoda conditions are discouraged, use `state == JobStatus.State.ATTEMPT_FAILURE` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1169'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1169:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1169'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1169:17:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "amd"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1170'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1170:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1170'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1170:20:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "amd64"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1171'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1171:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1171'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1171:20:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "x64"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1172'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1172:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1172'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1172:20:</a> SIM300 [*] Yoda conditions are discouraged, use `label == "asf-runner"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:17:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "arm"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:43:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:43:</a> SIM300 [*] Yoda conditions are discouraged, use `label.lower() == "arm64"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:71:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1189'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1189:71:</a> SIM300 [*] Yoda conditions are discouraged, use `label == "asf-arm"` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L112'>dev/breeze/tests/test_packages.py:112:12:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L112'>dev/breeze/tests/test_packages.py:112:12:</a> SIM300 [*] Yoda conditions are discouraged, use `get_removed_provider_ids() == []` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L117'>dev/breeze/tests/test_packages.py:117:12:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L117'>dev/breeze/tests/test_packages.py:117:12:</a> SIM300 [*] Yoda conditions are discouraged, use `get_suspended_provider_ids() == []` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L122'>dev/breeze/tests/test_packages.py:122:12:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/tests/test_packages.py#L122'>dev/breeze/tests/test_packages.py:122:12:</a> SIM300 [*] Yoda conditions are discouraged, use `get_suspended_provider_folders() == []` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L60'>docker_tests/test_prod_image.py:60:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L60'>docker_tests/test_prod_image.py:60:16:</a> SIM300 [*] Yoda conditions are discouraged, use `ctx.value.return_code == 2` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L66'>docker_tests/test_prod_image.py:66:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docker_tests/test_prod_image.py#L66'>docker_tests/test_prod_image.py:66:16:</a> SIM300 [*] Yoda conditions are discouraged, use `ctx.value.return_code == 2` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L102'>helm_tests/airflow_aux/test_airflow_common.py:102:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L102'>helm_tests/airflow_aux/test_airflow_common.py:102:16:</a> SIM300 [*] Yoda conditions are discouraged, use `len(docs) == 3` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L167'>helm_tests/airflow_aux/test_airflow_common.py:167:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L167'>helm_tests/airflow_aux/test_airflow_common.py:167:16:</a> SIM300 [*] Yoda conditions are discouraged, use `len(k8s_objects) == 7` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L228'>helm_tests/airflow_aux/test_airflow_common.py:228:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L228'>helm_tests/airflow_aux/test_airflow_common.py:228:16:</a> SIM300 [*] Yoda conditions are discouraged, use `len(k8s_objects) == 12` instead
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L236'>helm_tests/airflow_aux/test_airflow_common.py:236:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L236'>helm_tests/airflow_aux/test_airflow_common.py:236:20:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L244'>helm_tests/airflow_aux/test_airflow_common.py:244:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L244'>helm_tests/airflow_aux/test_airflow_common.py:244:20:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L245'>helm_tests/airflow_aux/test_airflow_common.py:245:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L245'>helm_tests/airflow_aux/test_airflow_common.py:245:20:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L246'>helm_tests/airflow_aux/test_airflow_common.py:246:20:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L246'>helm_tests/airflow_aux/test_airflow_common.py:246:20:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/helm_tests/airflow_aux/test_airflow_common.py#L387'>helm_tests/airflow_aux/test_airflow_common.py:387:16:</a> SIM300 [*] Yoda condition detected
... 3609 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+162 -162 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/stats/sinaplot.py#L42'>examples/topics/stats/sinaplot.py:42:19:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/stats/sinaplot.py#L42'>examples/topics/stats/sinaplot.py:42:19:</a> SIM300 [*] Yoda conditions are discouraged, use `month == df.MONTH` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L118'>src/bokeh/core/property/numeric.py:118:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L118'>src/bokeh/core/property/numeric.py:118:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value > 0` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L68'>src/bokeh/core/property/numeric.py:68:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L68'>src/bokeh/core/property/numeric.py:68:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value >= 0` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L80'>src/bokeh/core/property/numeric.py:80:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L80'>src/bokeh/core/property/numeric.py:80:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value > 0` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L99'>src/bokeh/core/property/numeric.py:99:17:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/numeric.py#L99'>src/bokeh/core/property/numeric.py:99:17:</a> SIM300 [*] Yoda conditions are discouraged, use `value >= 0` instead
... 314 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/model-bakers/model_bakery/blob/a9ae541e9775690d536ad814fe4360d85fff7ca7/tests/test_baker.py#L817'>tests/test_baker.py:817:16:</a> SIM300 [*] Yoda condition detected
- <a href='https://github.com/model-bakers/model_bakery/blob/a9ae541e9775690d536ad814fe4360d85fff7ca7/tests/test_baker.py#L817'>tests/test_baker.py:817:16:</a> SIM300 [*] Yoda conditions are discouraged, use `instance.fk == ["foo"]` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM300 | 3980 | 1990 | 1990 | 0 | 0 |

</p>
</details>




---

_Review requested from @AlexWaygood by @charliermarsh on 2024-06-26 17:16_

---

_@AlexWaygood approved on 2024-06-27 10:02_

Lots of ecosystem hits, but that's why it's a minor release! Agreed that this is a big improvement

---

_Comment by @MichaReiser on 2024-06-27 10:12_

It seems that the vast majority of new violations is for usages in tests, specifically inside of `assert`. I think the rule should be disabled for assertions because a common pattern is to write the expected value on the left, which often is a literal. 

I'm also not sure if I would count this as a yoga condition:

```
assert [] == get_removed_provider_ids()
```

The right isn't a variable, it's a function call.

---

_Comment by @AlexWaygood on 2024-06-27 10:14_

I would always put the thing I was testing (the result of the function call) on the left, and the thing I expected it to be on the right. `assert [] == my_function_call()` is just as much a Yoda condition as anything else this rule is detecting, in my opinion.

---

_Comment by @AlexWaygood on 2024-06-27 10:32_

I'm pretty strongly of the opinion that this is a clear improvement for this rule. In all of the Python codebases I've worked on, whether the test suite is using `unittest` or `pytest`, the object being tested goes on the left-hand side of the assertion. If some codebases have the opposite convention, they may just want to switch this rule off for their codebases, but in my opinion this change makes the rule more consistent and smarter without changing the basic premise of the rule. If you disagree with the premise, you can always switch it off ;-)

Pytest doesn't appear to display it any better or worse depending on the order of the assertion:

![image](https://github.com/astral-sh/ruff/assets/66076021/c436ef0f-606e-45a2-86bf-b34940ae772e)


---

_Merged by @AlexWaygood on 2024-06-27 10:32_

---

_Closed by @AlexWaygood on 2024-06-27 10:32_

---

_Branch deleted on 2024-06-27 10:32_

---

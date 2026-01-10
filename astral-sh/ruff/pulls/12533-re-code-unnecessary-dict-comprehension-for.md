```yaml
number: 12533
title: "Re-code `unnecessary-dict-comprehension-for-iterable` (`RUF025`) as `C420`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - breaking
assignees: []
merged: true
base: ruff-0.6
head: charlie/c420
created_at: 2024-07-26T14:36:08Z
updated_at: 2024-08-12T09:27:07Z
url: https://github.com/astral-sh/ruff/pull/12533
synced_at: 2026-01-10T21:38:31Z
```

# Re-code `unnecessary-dict-comprehension-for-iterable` (`RUF025`) as `C420`

---

_Pull request opened by @charliermarsh on 2024-07-26 14:36_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12110.


---

_Added to milestone `v0.6` by @charliermarsh on 2024-07-26 14:36_

---

_Label `rule` added by @charliermarsh on 2024-07-26 14:36_

---

_Comment by @github-actions[bot] on 2024-07-26 16:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+19 -19 violations, +0 -0 fixes in 15 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/ext/commands/flag_converter.py#L314'>disnake/ext/commands/flag_converter.py:314:28:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/ext/commands/flag_converter.py#L314'>disnake/ext/commands/flag_converter.py:314:28:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/evaluation/marker_stats.py#L114'>rasa/core/evaluation/marker_stats.py:114:51:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/featurizers/single_state_featurizer.py#L119'>rasa/core/featurizers/single_state_featurizer.py:119:20:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/featurizers/test_precomputation.py#L110'>tests/core/featurizers/test_precomputation.py:110:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8be53601b14f35af34c054cfbb856a57fb74fa96/src/plasmapy/particles/ionization_state_collection.py#L693'>src/plasmapy/particles/ionization_state_collection.py:693:40:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f0ef69198ec0b7ad0c489cbccf76f6130445fedf/helm_tests/airflow_aux/test_annotations.py#L409'>helm_tests/airflow_aux/test_annotations.py:409:63:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/apache/airflow/blob/f0ef69198ec0b7ad0c489cbccf76f6130445fedf/helm_tests/airflow_aux/test_annotations.py#L409'>helm_tests/airflow_aux/test_annotations.py:409:63:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/apache/airflow/blob/f0ef69198ec0b7ad0c489cbccf76f6130445fedf/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L459'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:459:36:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/apache/airflow/blob/f0ef69198ec0b7ad0c489cbccf76f6130445fedf/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L459'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:459:36:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/dac69e20922ac06b21267502fc9cf18b61de15cc/superset/reports/notifications/email.py#L58'>superset/reports/notifications/email.py:58:28:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/apache/superset/blob/dac69e20922ac06b21267502fc9cf18b61de15cc/superset/reports/notifications/email.py#L58'>superset/reports/notifications/email.py:58:28:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/apache/superset/blob/dac69e20922ac06b21267502fc9cf18b61de15cc/superset/reports/notifications/slack_mixin.py#L107'>superset/reports/notifications/slack_mixin.py:107:43:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/apache/superset/blob/dac69e20922ac06b21267502fc9cf18b61de15cc/superset/reports/notifications/slack_mixin.py#L107'>superset/reports/notifications/slack_mixin.py:107:43:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/apache/superset/blob/dac69e20922ac06b21267502fc9cf18b61de15cc/superset/reports/notifications/slack_mixin.py#L97'>superset/reports/notifications/slack_mixin.py:97:39:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/apache/superset/blob/dac69e20922ac06b21267502fc9cf18b61de15cc/superset/reports/notifications/slack_mixin.py#L97'>superset/reports/notifications/slack_mixin.py:97:39:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L145'>src/bokeh/command/subcommands/file_output.py:145:17:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L145'>src/bokeh/command/subcommands/file_output.py:145:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L831'>src/bokeh/command/subcommands/serve.py:831:17:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L831'>src/bokeh/command/subcommands/serve.py:831:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/docker/docker-py/blob/a3652028b1ead708bd9191efb286f909ba6c2a49/docker/types/containers.py#L725'>docker/types/containers.py:725:22:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/d4a2ea2e9237b8bc0eb1b7faafe6e26c65cd7184/ibis/common/egraph.py#L776'>ibis/common/egraph.py:776:17:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/ibis-project/ibis/blob/d4a2ea2e9237b8bc0eb1b7faafe6e26c65cd7184/ibis/common/egraph.py#L776'>ibis/common/egraph.py:776:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/6466731894ad9e901b105ccffeda3d5f72f46b93/tests/metrics/genai/test_genai_metrics.py#L554'>tests/metrics/genai/test_genai_metrics.py:554:37:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/417e5511f0ad1681b064b90d07d0ac59515e7ac1/src/build/__main__.py#L40'>src/build/__main__.py:40:14:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/pypa/build/blob/417e5511f0ad1681b064b90d07d0ac59515e7ac1/src/build/__main__.py#L40'>src/build/__main__.py:40:14:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/184390f4f2cde0316801eb701f49dda4f7a9a6ac/src/pip/_internal/locations/_sysconfig.py#L164'>src/pip/_internal/locations/_sysconfig.py:164:21:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/pypa/pip/blob/184390f4f2cde0316801eb701f49dda4f7a9a6ac/src/pip/_internal/locations/_sysconfig.py#L166'>src/pip/_internal/locations/_sysconfig.py:166:21:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/368fa5ddc37c35393db919bcd52b2a2a2d668b79/skbuild/setuptools_wrap.py#L516'>skbuild/setuptools_wrap.py:516:22:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/scikit-build/scikit-build/blob/368fa5ddc37c35393db919bcd52b2a2a2d668b79/skbuild/setuptools_wrap.py#L516'>skbuild/setuptools_wrap.py:516:22:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/scikit-build/scikit-build/blob/368fa5ddc37c35393db919bcd52b2a2a2d668b79/skbuild/setuptools_wrap.py#L519'>skbuild/setuptools_wrap.py:519:19:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/scikit-build/scikit-build/blob/368fa5ddc37c35393db919bcd52b2a2a2d668b79/skbuild/setuptools_wrap.py#L519'>skbuild/setuptools_wrap.py:519:19:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/ae0ce3ce83ae5734dfafd91a176e25a6818cd00e/src/scikit_build_core/_logging.py#L118'>src/scikit_build_core/_logging.py:118:14:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/scikit-build/scikit-build-core/blob/ae0ce3ce83ae5734dfafd91a176e25a6818cd00e/src/scikit_build_core/_logging.py#L118'>src/scikit_build_core/_logging.py:118:14:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/6c806b499ddbb844753b5c8c4d70a8b98b9d1c3a/src/_pytest/logging.py#L128'>src/_pytest/logging.py:128:24:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/pytest-dev/pytest/blob/6c806b499ddbb844753b5c8c4d70a8b98b9d1c3a/src/_pytest/reports.py#L345'>src/_pytest/reports.py:345:20:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
- <a href='https://github.com/pytest-dev/pytest/blob/6c806b499ddbb844753b5c8c4d70a8b98b9d1c3a/testing/conftest.py#L191'>testing/conftest.py:191:21:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mesonbuild/meson-python/blob/063fe8bc8ad6984c29b24a0f72a14e1ced091a9c/mesonpy/__init__.py#L570'>mesonpy/__init__.py:570:24:</a> C420 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C420 | 19 | 19 | 0 | 0 | 0 |
| RUF025 | 19 | 0 | 19 | 0 | 0 |

</p>
</details>




---

_@Skylion007 reviewed on 2024-07-27 08:04_

LGTM

---

_Label `breaking` added by @MichaReiser on 2024-07-29 06:00_

---

_@MichaReiser approved on 2024-07-29 06:01_

---

_Merged by @MichaReiser on 2024-08-12 09:27_

---

_Closed by @MichaReiser on 2024-08-12 09:27_

---

_Branch deleted on 2024-08-12 09:27_

---

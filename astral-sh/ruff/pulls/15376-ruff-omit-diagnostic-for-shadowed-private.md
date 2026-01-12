```yaml
number: 15376
title: "[`ruff`] Omit diagnostic for shadowed private function parameters in `used-dummy-variable` (`RUF052`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: private-fn-arg
created_at: 2025-01-09T17:12:08Z
updated_at: 2025-01-10T09:09:26Z
url: https://github.com/astral-sh/ruff/pull/15376
synced_at: 2026-01-12T15:55:51Z
```

# [`ruff`] Omit diagnostic for shadowed private function parameters in `used-dummy-variable` (`RUF052`)

---

_@dylwil3_

As discussed in #14818, we should skip [used-dummy-variable (RUF052)](https://docs.astral.sh/ruff/rules/used-dummy-variable/#used-dummy-variable-ruf052) for private function parameters. This PR extends the logic from loc. cit. and further skips the lint for bindings in the body of a function which shadow a private function parameter.

Closes #14968 


---

_Label `rule` added by @dylwil3 on 2025-01-09 17:12_

---

_Comment by @github-actions[bot] on 2025-01-09 17:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -12 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/crf.py#L119'>rasa/utils/tensorflow/crf.py:119:9:</a> RUF052 Local dummy variable `_state` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/crf.py#L386'>rasa/utils/tensorflow/crf.py:386:9:</a> RUF052 Local dummy variable `_state` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/72ab1d54fdfe1587d7cb1238461332e808c1bcf8/dev/breeze/src/airflow_breeze/utils/run_utils.py#L122'>dev/breeze/src/airflow_breeze/utils/run_utils.py:122:13:</a> RUF052 Local dummy variable `_argument` is accessed
- <a href='https://github.com/apache/airflow/blob/72ab1d54fdfe1587d7cb1238461332e808c1bcf8/providers/src/airflow/providers/elasticsearch/log/es_response.py#L33'>providers/src/airflow/providers/elasticsearch/log/es_response.py:33:13:</a> RUF052 Local dummy variable `_list` is accessed
- <a href='https://github.com/apache/airflow/blob/72ab1d54fdfe1587d7cb1238461332e808c1bcf8/providers/src/airflow/providers/opensearch/log/os_response.py#L33'>providers/src/airflow/providers/opensearch/log/os_response.py:33:13:</a> RUF052 Local dummy variable `_list` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/4693d1a000f34641f8215caea6872b3357f4db76/pandas/core/nanops.py#L765'>pandas/core/nanops.py:765:13:</a> RUF052 Local dummy variable `_mask` is accessed
- <a href='https://github.com/pandas-dev/pandas/blob/4693d1a000f34641f8215caea6872b3357f4db76/pandas/core/nanops.py#L767'>pandas/core/nanops.py:767:13:</a> RUF052 Local dummy variable `_mask` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/reflex-dev/reflex/blob/79611abdabcd4d19f2404e695d65064266701f03/reflex/vars/sequence.py#L624'>reflex/vars/sequence.py:624:9:</a> RUF052 Local dummy variable `_var_type` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build-core/blob/241c63bcec484b63f1c9d95eb696414cfc02f6c8/src/scikit_build_core/settings/sources.py#L72'>src/scikit_build_core/settings/sources.py:72:9:</a> RUF052 Local dummy variable `__dict` is accessed
- <a href='https://github.com/scikit-build/scikit-build-core/blob/241c63bcec484b63f1c9d95eb696414cfc02f6c8/src/scikit_build_core/settings/sources.py#L82'>src/scikit_build_core/settings/sources.py:82:9:</a> RUF052 Local dummy variable `__dict` is accessed
- <a href='https://github.com/scikit-build/scikit-build-core/blob/241c63bcec484b63f1c9d95eb696414cfc02f6c8/src/scikit_build_core/settings/sources.py#L93'>src/scikit_build_core/settings/sources.py:93:10:</a> RUF052 Local dummy variable `__opt` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/fits/util.py#L181'>astropy/io/fits/util.py:181:9:</a> RUF052 Local dummy variable `_seen` is accessed
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF052 | 12 | 0 | 12 | 0 | 0 |

</p>
</details>




---

_Comment by @dylwil3 on 2025-01-09 17:20_

Ecosystem results all look correct to me!

---

_@MichaReiser approved on 2025-01-10 07:40_

---

_Label `preview` added by @MichaReiser on 2025-01-10 07:40_

---

_Merged by @dylwil3 on 2025-01-10 09:09_

---

_Closed by @dylwil3 on 2025-01-10 09:09_

---

```yaml
number: 11131
title: "[`flake8-blind-expect`] Allow raise from in `BLE001`"
type: pull_request
state: merged
author: autinerd
labels:
  - bug
assignees: []
merged: true
base: main
head: ble001-raise-from
created_at: 2024-04-24T15:39:54Z
updated_at: 2024-04-24T15:59:25Z
url: https://github.com/astral-sh/ruff/pull/11131
synced_at: 2026-01-10T22:37:01Z
```

# [`flake8-blind-expect`] Allow raise from in `BLE001`

---

_Pull request opened by @autinerd on 2024-04-24 15:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This allows `raise from` in BLE001.

```python
try:
    ...
except Exception as e:
    raise ValueError from e
```

Fixes #10806

## Test Plan

Test case added.


---

_Label `bug` added by @charliermarsh on 2024-04-24 15:44_

---

_Comment by @github-actions[bot] on 2024-04-24 15:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+10 -12 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/src/plasmapy/formulary/collisions/helio/collisional_analysis.py#L355'>src/plasmapy/formulary/collisions/helio/collisional_analysis.py:355:29:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/dispersion/test_dispersion_functions.py#L122'>tests/dispersion/test_dispersion_functions.py:122:35:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/particles/conftest.py#L19'>tests/particles/conftest.py:19:31:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/particles/test_particle_class.py#L530'>tests/particles/test_particle_class.py:530:31:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/particles/test_particle_class.py#L912'>tests/particles/test_particle_class.py:912:31:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L464'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:464:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L472'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:472:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/operators/python.py#L782'>airflow/operators/python.py:782:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L104'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:104:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L126'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:126:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L143'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:143:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L160'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:160:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L179'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:179:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L192'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:192:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/cncf/kubernetes/pod_generator.py#L454'>airflow/providers/cncf/kubernetes/pod_generator.py:454:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/cncf/kubernetes/pod_generator.py#L462'>airflow/providers/cncf/kubernetes/pod_generator.py:462:20:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/07019781e2992eaaf785a93429da692f3469cd3d/ibis/backends/__init__.py#L54'>ibis/backends/__init__.py:54:35:</a> RUF100 Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/ibis-project/ibis/blob/07019781e2992eaaf785a93429da692f3469cd3d/ibis/backends/__init__.py#L62'>ibis/backends/__init__.py:62:35:</a> RUF100 Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/ibis-project/ibis/blob/07019781e2992eaaf785a93429da692f3469cd3d/ibis/examples/__init__.py#L171'>ibis/examples/__init__.py:171:29:</a> RUF100 Unused `noqa` directive (unused: `BLE001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/scripts/lib/setup_venv.py#L344'>scripts/lib/setup_venv.py:344:16:</a> BLE001 Do not catch blind exception: `BaseException`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/tools/lib/provision.py#L385'>tools/lib/provision.py:385:20:</a> BLE001 Do not catch blind exception: `BaseException`
- <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/zproject/backends.py#L1031'>zproject/backends.py:1031:16:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 14 | 2 | 12 | 0 | 0 |
| RUF100 | 8 | 8 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+11 -13 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+6 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/src/plasmapy/formulary/collisions/helio/collisional_analysis.py#L238'>src/plasmapy/formulary/collisions/helio/collisional_analysis.py:238:9:</a> PLR0917 Too many positional arguments (12/5)
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/src/plasmapy/formulary/collisions/helio/collisional_analysis.py#L238'>src/plasmapy/formulary/collisions/helio/collisional_analysis.py:238:9:</a> PLR0917 Too many positional arguments (12/5)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/src/plasmapy/formulary/collisions/helio/collisional_analysis.py#L355'>src/plasmapy/formulary/collisions/helio/collisional_analysis.py:355:29:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/dispersion/test_dispersion_functions.py#L122'>tests/dispersion/test_dispersion_functions.py:122:35:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/particles/conftest.py#L19'>tests/particles/conftest.py:19:31:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/particles/test_particle_class.py#L530'>tests/particles/test_particle_class.py:530:31:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/31d15be30762c98f40390d5bd6ee20b04df7af71/tests/particles/test_particle_class.py#L912'>tests/particles/test_particle_class.py:912:31:</a> RUF100 [*] Unused `noqa` directive (unused: `BLE001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L464'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:464:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L472'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:472:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/operators/python.py#L782'>airflow/operators/python.py:782:20:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L104'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:104:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L126'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:126:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L143'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:143:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L160'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:160:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L179'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:179:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py#L192'>airflow/providers/alibaba/cloud/hooks/analyticdb_spark.py:192:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/cncf/kubernetes/pod_generator.py#L454'>airflow/providers/cncf/kubernetes/pod_generator.py:454:16:</a> BLE001 Do not catch blind exception: `Exception`
- <a href='https://github.com/apache/airflow/blob/14b631294d07e3323905bde4e9bf257be5d7ba5e/airflow/providers/cncf/kubernetes/pod_generator.py#L462'>airflow/providers/cncf/kubernetes/pod_generator.py:462:20:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/07019781e2992eaaf785a93429da692f3469cd3d/ibis/backends/__init__.py#L54'>ibis/backends/__init__.py:54:35:</a> RUF100 Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/ibis-project/ibis/blob/07019781e2992eaaf785a93429da692f3469cd3d/ibis/backends/__init__.py#L62'>ibis/backends/__init__.py:62:35:</a> RUF100 Unused `noqa` directive (unused: `BLE001`)
+ <a href='https://github.com/ibis-project/ibis/blob/07019781e2992eaaf785a93429da692f3469cd3d/ibis/examples/__init__.py#L171'>ibis/examples/__init__.py:171:29:</a> RUF100 Unused `noqa` directive (unused: `BLE001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/scripts/lib/setup_venv.py#L344'>scripts/lib/setup_venv.py:344:16:</a> BLE001 Do not catch blind exception: `BaseException`
+ <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/tools/lib/provision.py#L385'>tools/lib/provision.py:385:20:</a> BLE001 Do not catch blind exception: `BaseException`
- <a href='https://github.com/zulip/zulip/blob/7aa90440ed6352722d660c1c3f1f3fec7b87dc2c/zproject/backends.py#L1031'>zproject/backends.py:1031:16:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 14 | 2 | 12 | 0 | 0 |
| RUF100 | 8 | 8 | 0 | 0 | 0 |
| PLR0917 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-04-24 15:55_

Thanks!

---

_Renamed from "[flake8-blind-expect] Allow raise from in BLE001" to "[`flake8-blind-expect`] Allow raise from in `BLE001`" by @charliermarsh on 2024-04-24 15:56_

---

_Merged by @charliermarsh on 2024-04-24 15:56_

---

_Closed by @charliermarsh on 2024-04-24 15:56_

---

_Branch deleted on 2024-04-24 15:59_

---

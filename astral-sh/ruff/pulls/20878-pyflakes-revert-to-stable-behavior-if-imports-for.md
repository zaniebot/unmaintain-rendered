```yaml
number: 20878
title: "[`pyflakes`] Revert to stable behavior if imports for module lie in alternate branches for `F401`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: f401-branches
created_at: 2025-10-15T01:25:21Z
updated_at: 2025-10-28T12:57:01Z
url: https://github.com/astral-sh/ruff/pull/20878
synced_at: 2026-01-12T15:57:11Z
```

# [`pyflakes`] Revert to stable behavior if imports for module lie in alternate branches for `F401`

---

_@dylwil3_

Closes #20839

---

_Label `bug` added by @dylwil3 on 2025-10-15 01:25_

---

_Label `rule` added by @dylwil3 on 2025-10-15 01:25_

---

_Label `preview` added by @dylwil3 on 2025-10-15 01:25_

---

_Comment by @github-actions[bot] on 2025-10-15 01:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -22 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/plotting.py#L35'>rasa/utils/plotting.py:35:20:</a> F401 `tkinter` imported but unused; consider using `importlib.util.find_spec` to test for availability
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3eac6805ade2e21d7ef7e4bab2abcacccc02905d/devel-common/src/tests_common/pytest_plugin.py#L2164'>devel-common/src/tests_common/pytest_plugin.py:2164:16:</a> F401 `airflow.sdk._shared.logging` imported but unused; consider using `importlib.util.find_spec` to test for availability
- <a href='https://github.com/apache/airflow/blob/3eac6805ade2e21d7ef7e4bab2abcacccc02905d/devel-common/src/tests_common/pytest_plugin.py#L2167'>devel-common/src/tests_common/pytest_plugin.py:2167:20:</a> F401 `airflow.sdk._shared.logging` imported but unused; consider using `importlib.util.find_spec` to test for availability
- <a href='https://github.com/apache/airflow/blob/3eac6805ade2e21d7ef7e4bab2abcacccc02905d/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L120'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:120:16:</a> F401 [*] `airflow.jobs.local_task_job_runner` imported but unused
- <a href='https://github.com/apache/airflow/blob/3eac6805ade2e21d7ef7e4bab2abcacccc02905d/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L121'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:121:16:</a> F401 [*] `airflow.macros` imported but unused
- <a href='https://github.com/apache/airflow/blob/3eac6805ade2e21d7ef7e4bab2abcacccc02905d/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L124'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:124:16:</a> F401 `airflow.providers.standard.operators.bash` imported but unused; consider using `importlib.util.find_spec` to test for availability
- <a href='https://github.com/apache/airflow/blob/3eac6805ade2e21d7ef7e4bab2abcacccc02905d/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L125'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:125:16:</a> F401 `airflow.providers.standard.operators.python` imported but unused; consider using `importlib.util.find_spec` to test for availability
- <a href='https://github.com/apache/airflow/blob/3eac6805ade2e21d7ef7e4bab2abcacccc02905d/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L127'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:127:16:</a> F401 [*] `airflow.operators.bash` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Multi_Agent_Legacy.py#L57'>crazy_functions/Multi_Agent_Legacy.py:57:16:</a> F401 [*] `autogen` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_model_utils.py#L74'>tests/utils/test_model_utils.py:74:20:</a> F401 [*] `dummy_module` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L320'>tests/utils/test_requirements_utils.py:320:16:</a> F401 [*] `databricks` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L321'>tests/utils/test_requirements_utils.py:321:16:</a> F401 [*] `databricks.automl` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L322'>tests/utils/test_requirements_utils.py:322:16:</a> F401 [*] `databricks.automl_foo` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L323'>tests/utils/test_requirements_utils.py:323:16:</a> F401 [*] `databricks.automl_runtime` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L324'>tests/utils/test_requirements_utils.py:324:16:</a> F401 [*] `databricks.model_monitoring` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L332'>tests/utils/test_requirements_utils.py:332:16:</a> F401 [*] `databricks.automl` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L333'>tests/utils/test_requirements_utils.py:333:16:</a> F401 [*] `databricks.automl_foo` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L334'>tests/utils/test_requirements_utils.py:334:16:</a> F401 [*] `databricks.automl_runtime` imported but unused
- <a href='https://github.com/mlflow/mlflow/blob/7bbd8945a532053f7b5cecda8f8bbcc8a8d9c418/tests/utils/test_requirements_utils.py#L335'>tests/utils/test_requirements_utils.py:335:16:</a> F401 [*] `databricks.model_monitoring` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build/blob/a2afff25f98e0d1fe573f520b659e1ee1f427c38/skbuild/utils/__init__.py#L33'>skbuild/utils/__init__.py:33:12:</a> F401 `setuptools.logging` imported but unused; consider using `importlib.util.find_spec` to test for availability
- <a href='https://github.com/scikit-build/scikit-build/blob/a2afff25f98e0d1fe573f520b659e1ee1f427c38/tests/__init__.py#L9'>tests/__init__.py:9:12:</a> F401 `distutils` imported but unused
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mesonbuild/meson-python/blob/09e18fc87f52c67e341d34147532d23b324142c1/tests/test_editable.py#L281'>tests/test_editable.py:281:24:</a> F401 [*] `pure` imported but unused
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 22 | 0 | 22 | 0 | 0 |

</p>
</details>




---

_Comment by @dylwil3 on 2025-10-17 17:47_

The ecosystem hits look correct - these two are debatable, but keeping the stable behavior seems fine for now:

- [providers/google/src/airflow/providers/google/common/utils/id_token_credentials.py:43:12:](https://github.com/apache/airflow/blob/7939692b266a20b340f133f998612f083c585b6c/providers/google/src/airflow/providers/google/common/utils/id_token_credentials.py#L43)
- [mlflow/pyfunc/loaders/__init__.py:1:8:](https://github.com/mlflow/mlflow/blob/2749d7690b0ed57e0a26af4475021a380e25fdb3/mlflow/pyfunc/loaders/__init__.py#L1)

---

_Marked ready for review by @dylwil3 on 2025-10-17 17:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:947 on 2025-10-20 07:42_

Nit: Maybe add some spaces to make it clearer that these are separate checks (and the comment only applies to the first)
```suggestion
        if i > 0 && shadowed_binding.is_used() {
            return false;
        }
        
        if shadowed_binding
            .source
            .is_none_or(|node_id| !semantic.same_branch(node_id, binding_node))
        {
            return false;
        }
        
        matches!(
```

---

_Comment by @MichaReiser on 2025-10-20 07:45_

The examples with `if TYPE_CHECKING` are a bit unfortunate. Do you think it would be possible to instead test if the import is in an alternate branch than the other import? 

E.g. the following should be flagged

```py

if True:
	import a.b
	if other:
		import a.b
```

but not

```py
if True:
	import a.b
else:
	if other:
		import a.b
```

So, I guess, we'd have to check if the first import ancestor chain is a prefix of the second import's ancestor chain

---

_Renamed from "[`pyflakes`] Revert to stable behavior if imports for module lie in different branches for `F401`" to "[`pyflakes`] Revert to stable behavior if imports for module lie in alternate branches for `F401`" by @dylwil3 on 2025-10-24 19:34_

---

_@MichaReiser approved on 2025-10-27 07:51_

---

_Comment by @MichaReiser on 2025-10-27 07:52_

Did this new approach improve the ecosystem results?

---

_Comment by @dylwil3 on 2025-10-27 12:08_

> Did this new approach improve the ecosystem results?

Yes! The things that remain are variations on the `try`/`except` pattern in the original issue.

---

_Merged by @dylwil3 on 2025-10-27 15:23_

---

_Closed by @dylwil3 on 2025-10-27 15:23_

---

_Branch deleted on 2025-10-28 12:57_

---

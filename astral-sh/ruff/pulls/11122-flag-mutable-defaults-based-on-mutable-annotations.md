```yaml
number: 11122
title: Flag mutable defaults based on mutable annotations
type: pull_request
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
draft: true
base: main
head: charlie/b006
created_at: 2024-04-24T03:10:04Z
updated_at: 2025-12-10T17:44:14Z
url: https://github.com/astral-sh/ruff/pull/11122
synced_at: 2026-01-10T16:42:11Z
```

# Flag mutable defaults based on mutable annotations

---

_Pull request opened by @charliermarsh on 2024-04-24 03:10_

## Summary

We'll now flag `def foo(a: list = LIST): ...` as a mutable default based on the annotation. (If it _is_ immutable, the annotation should be `Sequence`.)

I want to see what the ecosystem checks look like here. It might be too noisy.

Closes #11030.


---

_Label `rule` added by @charliermarsh on 2024-04-24 03:10_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-04-24 03:11_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-04-24 03:11_

---

_Comment by @github-actions[bot] on 2024-04-24 03:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+34 -0 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/5e61775561b6f7cfee9867a8a2a14f73a7b3b78f/tools/lib/live_logreader.py#L10'>tools/lib/live_logreader.py:10:46:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/commaai/openpilot/blob/5e61775561b6f7cfee9867a8a2a14f73a7b3b78f/tools/lib/live_logreader.py#L27'>tools/lib/live_logreader.py:27:42:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e9b0a3c914088ce1f89cde16c61f61807ccc6730/pandas/io/formats/css.py#L351'>pandas/io/formats/css.py:351:76:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/b084c805d4d4a2429cf35eee909779fb45b36d8c/reflex/app.py#L418'>reflex/app.py:418:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/reflex-dev/reflex/blob/b084c805d4d4a2429cf35eee909779fb45b36d8c/reflex/app.py#L571'>reflex/app.py:571:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/reflex-dev/reflex/blob/b084c805d4d4a2429cf35eee909779fb45b36d8c/reflex/utils/prerequisites.py#L361'>reflex/utils/prerequisites.py:361:33:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001.py#L15'>docs_src/dependencies/tutorial001.py:15:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001.py#L20'>docs_src/dependencies/tutorial001.py:20:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001_py310.py#L11'>docs_src/dependencies/tutorial001_py310.py:11:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001_py310.py#L16'>docs_src/dependencies/tutorial001_py310.py:16:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001.py#L16'>docs_src/dependency_testing/tutorial001.py:16:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001.py#L21'>docs_src/dependency_testing/tutorial001.py:21:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001_py310.py#L12'>docs_src/dependency_testing/tutorial001_py310.py:12:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001_py310.py#L17'>docs_src/dependency_testing/tutorial001_py310.py:17:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/query_params_str_validations/tutorial012_py39.py#L7'>docs_src/query_params_str_validations/tutorial012_py39.py:7:37:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/query_params_str_validations/tutorial013.py#L7'>docs_src/query_params_str_validations/tutorial013.py:7:32:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/request_files/tutorial002_py39.py#L8'>docs_src/request_files/tutorial002_py39.py:8:45:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/request_files/tutorial003_py39.py#L16'>docs_src/request_files/tutorial003_py39.py:16:31:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/request_files/tutorial003_py39.py#L9'>docs_src/request_files/tutorial003_py39.py:9:26:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L125'>tests/test_dependency_contextmanager.py:125:39:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L130'>tests/test_dependency_contextmanager.py:130:45:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L137'>tests/test_dependency_contextmanager.py:137:66:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L183'>tests/test_dependency_contextmanager.py:183:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L188'>tests/test_dependency_contextmanager.py:188:44:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L196'>tests/test_dependency_contextmanager.py:196:43:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L74'>tests/test_dependency_contextmanager.py:74:35:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L82'>tests/test_dependency_contextmanager.py:82:35:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_normal_exceptions.py#L30'>tests/test_dependency_normal_exceptions.py:30:50:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_normal_exceptions.py#L37'>tests/test_dependency_normal_exceptions.py:37:59:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_overrides.py#L18'>tests/test_dependency_overrides.py:18:40:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_overrides.py#L28'>tests/test_dependency_overrides.py:28:42:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_overrides.py#L50'>tests/test_dependency_overrides.py:50:53:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_forms_from_non_typing_sequences.py#L13'>tests/test_forms_from_non_typing_sequences.py:13:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_forms_from_non_typing_sequences.py#L8'>tests/test_forms_from_non_typing_sequences.py:8:40:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B006 | 34 | 34 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+34 -0 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/5e61775561b6f7cfee9867a8a2a14f73a7b3b78f/tools/lib/live_logreader.py#L10'>tools/lib/live_logreader.py:10:46:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/commaai/openpilot/blob/5e61775561b6f7cfee9867a8a2a14f73a7b3b78f/tools/lib/live_logreader.py#L27'>tools/lib/live_logreader.py:27:42:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e9b0a3c914088ce1f89cde16c61f61807ccc6730/pandas/io/formats/css.py#L351'>pandas/io/formats/css.py:351:76:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/b084c805d4d4a2429cf35eee909779fb45b36d8c/reflex/app.py#L418'>reflex/app.py:418:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/reflex-dev/reflex/blob/b084c805d4d4a2429cf35eee909779fb45b36d8c/reflex/app.py#L571'>reflex/app.py:571:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/reflex-dev/reflex/blob/b084c805d4d4a2429cf35eee909779fb45b36d8c/reflex/utils/prerequisites.py#L361'>reflex/utils/prerequisites.py:361:33:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001.py#L15'>docs_src/dependencies/tutorial001.py:15:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001.py#L20'>docs_src/dependencies/tutorial001.py:20:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001_py310.py#L11'>docs_src/dependencies/tutorial001_py310.py:11:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependencies/tutorial001_py310.py#L16'>docs_src/dependencies/tutorial001_py310.py:16:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001.py#L16'>docs_src/dependency_testing/tutorial001.py:16:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001.py#L21'>docs_src/dependency_testing/tutorial001.py:21:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001_py310.py#L12'>docs_src/dependency_testing/tutorial001_py310.py:12:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/dependency_testing/tutorial001_py310.py#L17'>docs_src/dependency_testing/tutorial001_py310.py:17:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/query_params_str_validations/tutorial012_py39.py#L7'>docs_src/query_params_str_validations/tutorial012_py39.py:7:37:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/query_params_str_validations/tutorial013.py#L7'>docs_src/query_params_str_validations/tutorial013.py:7:32:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/request_files/tutorial002_py39.py#L8'>docs_src/request_files/tutorial002_py39.py:8:45:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/request_files/tutorial003_py39.py#L16'>docs_src/request_files/tutorial003_py39.py:16:31:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/docs_src/request_files/tutorial003_py39.py#L9'>docs_src/request_files/tutorial003_py39.py:9:26:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L125'>tests/test_dependency_contextmanager.py:125:39:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L130'>tests/test_dependency_contextmanager.py:130:45:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L137'>tests/test_dependency_contextmanager.py:137:66:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L183'>tests/test_dependency_contextmanager.py:183:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L188'>tests/test_dependency_contextmanager.py:188:44:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L196'>tests/test_dependency_contextmanager.py:196:43:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L74'>tests/test_dependency_contextmanager.py:74:35:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_contextmanager.py#L82'>tests/test_dependency_contextmanager.py:82:35:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_normal_exceptions.py#L30'>tests/test_dependency_normal_exceptions.py:30:50:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_normal_exceptions.py#L37'>tests/test_dependency_normal_exceptions.py:37:59:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_overrides.py#L18'>tests/test_dependency_overrides.py:18:40:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_overrides.py#L28'>tests/test_dependency_overrides.py:28:42:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_dependency_overrides.py#L50'>tests/test_dependency_overrides.py:50:53:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_forms_from_non_typing_sequences.py#L13'>tests/test_forms_from_non_typing_sequences.py:13:38:</a> B006 Do not use mutable data structures for argument defaults
+ <a href='https://github.com/tiangolo/fastapi/blob/38929aae1b6d42848652705e5ca618a675dba0e1/tests/test_forms_from_non_typing_sequences.py#L8'>tests/test_forms_from_non_typing_sequences.py:8:40:</a> B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B006 | 34 | 34 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @charliermarsh on 2024-04-24 03:35_

---

_Comment by @charliermarsh on 2024-04-24 03:35_

Just gonna mark as draft since I haven’t had a chance to review the ecosystem results and I’m going to bed.

---

_Comment by @dhruvmanila on 2024-04-24 05:49_

I haven't looked at the code nor the ecosystem checks, but I think my suggestion was to use the `TypeChecker` to determine if the variable is assigned to a mutable type

https://github.com/astral-sh/ruff/blob/f5c7a62aa65decb9e286bd65ba17f1a3bd1f91e6/crates/ruff_python_semantic/src/analyze/typing.rs#L421-L427

But, I think this _could_ be fine as well.

---

_@charliermarsh reviewed on 2024-04-24 13:01_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:125 on 2024-04-24 13:01_

(At the very least I think these should return enums rather than bools, with a single call to check immutability.)

---

_Comment by @zanieb on 2024-04-24 13:07_

The ecosystem checks look good to me, except the FastAPI ones which are... hard to comment on. I'm not sure if they can use this rule?

I'd gate this with preview.

---

_Comment by @charliermarsh on 2024-04-24 13:07_

(I think we generally recommend adding `Query` to `extend-immutable-types` or whatever the setting is called.)

---

_Comment by @AlexWaygood on 2025-04-15 17:57_

removing my request for review for now, since this has been in draft for ages and has merge conflicts now ;)

feel free to re-request my reivew if it's out of draft and you want my input!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-15 17:57_

---

_Comment by @MichaReiser on 2025-12-10 17:44_

Thanks for your contribution. I'll close this PR because it has become stale. Please don't hesitate to resubmit the PR and reference this PR if you plan to keep working on this feature.

---

_Closed by @MichaReiser on 2025-12-10 17:44_

---

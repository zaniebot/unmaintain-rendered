```yaml
number: 8231
title: "Make `unnecessary-paren-on-raise-exception` an unsafe edit"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: charlie/rse
created_at: 2023-10-25T22:04:03Z
updated_at: 2023-10-26T16:30:12Z
url: https://github.com/astral-sh/ruff/pull/8231
synced_at: 2026-01-12T02:32:42Z
```

# Make `unnecessary-paren-on-raise-exception` an unsafe edit

---

_Pull request opened by @charliermarsh on 2023-10-25 22:04_

## Summary

This rule is now unsafe if we can't verify that the `obj` in `raise obj()` is a class or builtin. (If we verify that it's a function, we don't raise at all, as before.)

See the documentation change for motivation behind the unsafe edit.

Closes https://github.com/astral-sh/ruff/issues/8228.


---

_Review requested from @zanieb by @charliermarsh on 2023-10-25 22:04_

---

_Label `autofix` added by @charliermarsh on 2023-10-25 22:04_

---

_Comment by @charliermarsh on 2023-10-25 22:14_

Ah, we can mark it as safe if we know it's a class or a builtin, I think.

---

_Comment by @github-actions[bot] on 2023-10-25 22:23_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+22, -22, 0 error(s))

<details><summary>airflow (+18, -18)</summary>
<p>

<pre>
+ [*] 16237 fixable with the `--fix` option (6278 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 16254 fixable with the `--fix` option (6261 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/api_connexion/security.py#L92'>airflow/api_connexion/security.py:92:27:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/api_connexion/security.py#L92'>airflow/api_connexion/security.py:92:27:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/auth/managers/fab/decorators/auth.py#L59'>airflow/auth/managers/fab/decorators/auth.py:59:35:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/auth/managers/fab/decorators/auth.py#L59'>airflow/auth/managers/fab/decorators/auth.py:59:35:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/exceptions.py#L230'>airflow/exceptions.py:230:22:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/exceptions.py#L230'>airflow/exceptions.py:230:22:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/models/dagcode.py#L198'>airflow/models/dagcode.py:198:34:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/models/dagcode.py#L198'>airflow/models/dagcode.py:198:34:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/models/taskinstance.py#L424'>airflow/models/taskinstance.py:424:41:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/models/taskinstance.py#L424'>airflow/models/taskinstance.py:424:41:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/models/taskinstance.py#L907'>airflow/models/taskinstance.py:907:38:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/models/taskinstance.py#L907'>airflow/models/taskinstance.py:907:38:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/operators/python.py#L418'>airflow/operators/python.py:418:43:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/operators/python.py#L418'>airflow/operators/python.py:418:43:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/providers/dbt/cloud/sensors/dbt.py#L143'>airflow/providers/dbt/cloud/sensors/dbt.py:143:35:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/providers/dbt/cloud/sensors/dbt.py#L143'>airflow/providers/dbt/cloud/sensors/dbt.py:143:35:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/timetables/_cron.py#L69'>airflow/timetables/_cron.py:69:38:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/airflow/timetables/_cron.py#L69'>airflow/timetables/_cron.py:69:38:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/dags/test_on_failure_callback.py#L44'>tests/dags/test_on_failure_callback.py:44:31:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/dags/test_on_failure_callback.py#L44'>tests/dags/test_on_failure_callback.py:44:31:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/decorators/test_python.py#L815'>tests/decorators/test_python.py:815:39:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/decorators/test_python.py#L815'>tests/decorators/test_python.py:815:39:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/decorators/test_python.py#L854'>tests/decorators/test_python.py:854:39:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/decorators/test_python.py#L854'>tests/decorators/test_python.py:854:39:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L1019'>tests/models/test_taskinstance.py:1019:39:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L1019'>tests/models/test_taskinstance.py:1019:39:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L533'>tests/models/test_taskinstance.py:533:35:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L533'>tests/models/test_taskinstance.py:533:35:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L757'>tests/models/test_taskinstance.py:757:39:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L757'>tests/models/test_taskinstance.py:757:39:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L854'>tests/models/test_taskinstance.py:854:39:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L854'>tests/models/test_taskinstance.py:854:39:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L953'>tests/models/test_taskinstance.py:953:39:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/apache/airflow/blob/0940d098590139c8ab5940813f628530c86944b6/tests/models/test_taskinstance.py#L953'>tests/models/test_taskinstance.py:953:39:</a> RSE102 [*] Unnecessary parentheses on raised exception
</pre>

</p>
</details>
<details><summary>bokeh (+2, -2)</summary>
<p>

<pre>
+ [*] 17799 fixable with the `--fix` option (4405 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 17800 fixable with the `--fix` option (4404 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/server/views/test_ws.py#L46'>tests/unit/bokeh/server/views/test_ws.py:46:39:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/server/views/test_ws.py#L46'>tests/unit/bokeh/server/views/test_ws.py:46:39:</a> RSE102 [*] Unnecessary parentheses on raised exception
</pre>

</p>
</details>
<details><summary>content (+2, -2)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/f96b3baa43e9541316ce510169a7599fe29653e4/Packs/SplunkPyPreRelease/Integrations/SplunkPyPreRelease/SplunkPyPreRelease_test.py#L1445'>Packs/SplunkPyPreRelease/Integrations/SplunkPyPreRelease/SplunkPyPreRelease_test.py:1445:34:</a> RSE102 Unnecessary parentheses on raised exception
- <a href='https://github.com/demisto/content/blob/f96b3baa43e9541316ce510169a7599fe29653e4/Packs/SplunkPyPreRelease/Integrations/SplunkPyPreRelease/SplunkPyPreRelease_test.py#L1445'>Packs/SplunkPyPreRelease/Integrations/SplunkPyPreRelease/SplunkPyPreRelease_test.py:1445:34:</a> RSE102 [*] Unnecessary parentheses on raised exception
+ [*] 12923 fixable with the `--fix` option (5492 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 12924 fixable with the `--fix` option (5491 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RSE102 | 38 | 19 | 19 |



---

_Label `bug` added by @charliermarsh on 2023-10-25 23:24_

---

_Comment by @charliermarsh on 2023-10-26 15:33_

@zanieb - Gonna merge this, it's strictly safer (marking some cases as unsafe).

---

_Merged by @charliermarsh on 2023-10-26 15:33_

---

_Closed by @charliermarsh on 2023-10-26 15:33_

---

_Branch deleted on 2023-10-26 15:33_

---

_@zanieb reviewed on 2023-10-26 15:34_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs`:23 on 2023-10-26 15:34_

```suggestion
/// returns an exception object, this rule will incorrectly mark the parentheses
```

---

_@zanieb reviewed on 2023-10-26 15:35_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs`:23 on 2023-10-26 15:35_

I'm also a little confused as I thought function definitions were excluded?

---

_@charliermarsh reviewed on 2023-10-26 16:30_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs`:23 on 2023-10-26 16:30_

Improved in https://github.com/astral-sh/ruff/pull/8256, thank you!

---

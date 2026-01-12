```yaml
number: 3962
title: "Add 'or if cond' to `E712` message"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/e712
created_at: 2023-04-13T18:56:15Z
updated_at: 2023-04-13T19:10:44Z
url: https://github.com/astral-sh/ruff/pull/3962
synced_at: 2026-01-12T15:55:14Z
```

# Add 'or if cond' to `E712` message

---

_@charliermarsh_

Closes #3899.

---

_Label `bug` added by @charliermarsh on 2023-04-13 18:57_

---

_Merged by @charliermarsh on 2023-04-13 19:02_

---

_Closed by @charliermarsh on 2023-04-13 19:02_

---

_Branch deleted on 2023-04-13 19:02_

---

_Comment by @github-actions[bot] on 2023-04-13 19:10_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+5, -5, 0 error(s))

<details><summary>airflow (+5, -5)</summary>
<p>

```diff
- airflow/providers/google_vendor/googleads/client.py:113:40: E712 [*] Comparison to `True` should be `cond is True`
+ airflow/providers/google_vendor/googleads/client.py:113:40: E712 [*] Comparison to `True` should be `cond is True` or `if cond:`
- airflow/providers/google_vendor/googleads/client.py:446:35: E712 [*] Comparison to `True` should be `cond is True`
+ airflow/providers/google_vendor/googleads/client.py:446:35: E712 [*] Comparison to `True` should be `cond is True` or `if cond:`
- airflow/providers/google_vendor/googleads/interceptors/response_wrappers.py:146:36: E712 [*] Comparison to `True` should be `cond is True`
+ airflow/providers/google_vendor/googleads/interceptors/response_wrappers.py:146:36: E712 [*] Comparison to `True` should be `cond is True` or `if cond:`
- airflow/providers/google_vendor/googleads/interceptors/response_wrappers.py:176:36: E712 [*] Comparison to `True` should be `cond is True`
+ airflow/providers/google_vendor/googleads/interceptors/response_wrappers.py:176:36: E712 [*] Comparison to `True` should be `cond is True` or `if cond:`
- airflow/providers/google_vendor/googleads/interceptors/response_wrappers.py:96:40: E712 [*] Comparison to `True` should be `cond is True`
+ airflow/providers/google_vendor/googleads/interceptors/response_wrappers.py:96:40: E712 [*] Comparison to `True` should be `cond is True` or `if cond:`
```

</p>
</details>
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

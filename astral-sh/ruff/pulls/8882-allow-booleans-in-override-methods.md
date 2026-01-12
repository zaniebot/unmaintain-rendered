```yaml
number: 8882
title: "Allow booleans in `@override` methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fbt
created_at: 2023-11-28T21:22:36Z
updated_at: 2023-11-28T21:42:38Z
url: https://github.com/astral-sh/ruff/pull/8882
synced_at: 2026-01-10T23:40:55Z
```

# Allow booleans in `@override` methods

---

_Pull request opened by @charliermarsh on 2023-11-28 21:22_

Closes #8867.

---

_Comment by @github-actions[bot] on 2023-11-28 21:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -37 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/manage.py#L78'>manage.py:78:30:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/manage.py#L78'>manage.py:78:30:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L338'>zerver/forms.py:338:20:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L338'>zerver/forms.py:338:20:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L360'>zerver/forms.py:360:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L360'>zerver/forms.py:360:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/safe_session_cached_db.py#L21'>zerver/lib/safe_session_cached_db.py:21:20:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/safe_session_cached_db.py#L21'>zerver/lib/safe_session_cached_db.py:21:20:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L364'>zerver/lib/test_runner.py:364:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L364'>zerver/lib/test_runner.py:364:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L365'>zerver/lib/test_runner.py:365:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L365'>zerver/lib/test_runner.py:365:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L125'>zerver/lib/upload/local.py:125:45:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L125'>zerver/lib/upload/local.py:125:45:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L160'>zerver/lib/upload/local.py:160:62:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L160'>zerver/lib/upload/local.py:160:62:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L205'>zerver/lib/upload/local.py:205:63:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L214'>zerver/lib/upload/local.py:214:64:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L230'>zerver/lib/upload/local.py:230:66:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L230'>zerver/lib/upload/local.py:230:66:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L320'>zerver/lib/upload/s3.py:320:45:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L320'>zerver/lib/upload/s3.py:320:45:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L351'>zerver/lib/upload/s3.py:351:62:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L351'>zerver/lib/upload/s3.py:351:62:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L412'>zerver/lib/upload/s3.py:412:63:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L422'>zerver/lib/upload/s3.py:422:64:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L452'>zerver/lib/upload/s3.py:452:66:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L452'>zerver/lib/upload/s3.py:452:66:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3343'>zerver/tests/test_auth_backends.py:3343:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3471'>zerver/tests/test_auth_backends.py:3471:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3471'>zerver/tests/test_auth_backends.py:3471:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3921'>zerver/tests/test_auth_backends.py:3921:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3923'>zerver/tests/test_auth_backends.py:3923:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3923'>zerver/tests/test_auth_backends.py:3923:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tornado/django_api.py#L39'>zerver/tornado/django_api.py:39:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tornado/django_api.py#L39'>zerver/tornado/django_api.py:39:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT002 Boolean default positional argument in function definition
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 21 | 0 | 21 | 0 | 0 |
| FBT002 | 16 | 0 | 16 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -38 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/manage.py#L78'>manage.py:78:30:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/manage.py#L78'>manage.py:78:30:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L338'>zerver/forms.py:338:20:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L338'>zerver/forms.py:338:20:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L360'>zerver/forms.py:360:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/forms.py#L360'>zerver/forms.py:360:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/safe_session_cached_db.py#L21'>zerver/lib/safe_session_cached_db.py:21:20:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/safe_session_cached_db.py#L21'>zerver/lib/safe_session_cached_db.py:21:20:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L364'>zerver/lib/test_runner.py:364:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L364'>zerver/lib/test_runner.py:364:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L365'>zerver/lib/test_runner.py:365:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/test_runner.py#L365'>zerver/lib/test_runner.py:365:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L125'>zerver/lib/upload/local.py:125:45:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L125'>zerver/lib/upload/local.py:125:45:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L160'>zerver/lib/upload/local.py:160:62:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L160'>zerver/lib/upload/local.py:160:62:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L205'>zerver/lib/upload/local.py:205:63:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L214'>zerver/lib/upload/local.py:214:64:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L230'>zerver/lib/upload/local.py:230:66:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/local.py#L230'>zerver/lib/upload/local.py:230:66:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L320'>zerver/lib/upload/s3.py:320:45:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L320'>zerver/lib/upload/s3.py:320:45:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L351'>zerver/lib/upload/s3.py:351:62:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L351'>zerver/lib/upload/s3.py:351:62:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L412'>zerver/lib/upload/s3.py:412:63:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L422'>zerver/lib/upload/s3.py:422:64:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L452'>zerver/lib/upload/s3.py:452:66:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/lib/upload/s3.py#L452'>zerver/lib/upload/s3.py:452:66:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3343'>zerver/tests/test_auth_backends.py:3343:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3471'>zerver/tests/test_auth_backends.py:3471:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3471'>zerver/tests/test_auth_backends.py:3471:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3921'>zerver/tests/test_auth_backends.py:3921:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3923'>zerver/tests/test_auth_backends.py:3923:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tests/test_auth_backends.py#L3923'>zerver/tests/test_auth_backends.py:3923:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tornado/django_api.py#L39'>zerver/tornado/django_api.py:39:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tornado/django_api.py#L39'>zerver/tornado/django_api.py:39:9:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/2f91179c1e2f66a0bb9f90b4b894182ee8067738/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:9:</a> FBT002 Boolean default positional argument in function definition
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 22 | 0 | 22 | 0 | 0 |
| FBT002 | 16 | 0 | 16 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-28 21:42_

---

_Closed by @charliermarsh on 2023-11-28 21:42_

---

_Branch deleted on 2023-11-28 21:42_

---

_Comment by @charliermarsh on 2023-11-28 21:42_

Ecosystem checks are correct.

---

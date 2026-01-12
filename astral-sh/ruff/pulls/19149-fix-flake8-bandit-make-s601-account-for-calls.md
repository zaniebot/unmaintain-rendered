```yaml
number: 19149
title: "fix(flake8_bandit): make S601 account for calls through clients"
type: pull_request
state: closed
author: CodeMan62
labels:
  - rule
assignees: []
base: main
head: codeman/19006
created_at: 2025-07-04T17:50:54Z
updated_at: 2025-08-18T07:35:21Z
url: https://github.com/astral-sh/ruff/pull/19149
synced_at: 2026-01-12T15:56:33Z
```

# fix(flake8_bandit): make S601 account for calls through clients

---

_@CodeMan62_

## Summary
closes #19006 
## Test Plan
I have wrote tests for it but let me know from the team that i have to push them or not


---

_Comment by @github-actions[bot] on 2025-07-04 18:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5d5a6d6d0a6e38e90876b04a94c3a1d624d45e27/providers/google/src/airflow/providers/google/cloud/hooks/compute_ssh.py#L49'>providers/google/src/airflow/providers/google/cloud/hooks/compute_ssh.py:49:27:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
+ <a href='https://github.com/apache/airflow/blob/5d5a6d6d0a6e38e90876b04a94c3a1d624d45e27/providers/ssh/src/airflow/providers/ssh/hooks/ssh.py#L273'>providers/ssh/src/airflow/providers/ssh/hooks/ssh.py:273:18:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/62da644302a7cfae4cb73098c1969a54c16578a9/src/latch_cli/centromere/utils.py#L181'>src/latch_cli/centromere/utils.py:181:19:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
+ <a href='https://github.com/latchbio/latch/blob/62da644302a7cfae4cb73098c1969a54c16578a9/src/latch_cli/centromere/utils.py#L207'>src/latch_cli/centromere/utils.py:207:11:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S601 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5d5a6d6d0a6e38e90876b04a94c3a1d624d45e27/providers/google/src/airflow/providers/google/cloud/hooks/compute_ssh.py#L49'>providers/google/src/airflow/providers/google/cloud/hooks/compute_ssh.py:49:27:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
+ <a href='https://github.com/apache/airflow/blob/5d5a6d6d0a6e38e90876b04a94c3a1d624d45e27/providers/ssh/src/airflow/providers/ssh/hooks/ssh.py#L273'>providers/ssh/src/airflow/providers/ssh/hooks/ssh.py:273:18:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/62da644302a7cfae4cb73098c1969a54c16578a9/src/latch_cli/centromere/utils.py#L181'>src/latch_cli/centromere/utils.py:181:19:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
+ <a href='https://github.com/latchbio/latch/blob/62da644302a7cfae4cb73098c1969a54c16578a9/src/latch_cli/centromere/utils.py#L207'>src/latch_cli/centromere/utils.py:207:11:</a> S601 Possible shell injection via Paramiko call; check inputs are properly sanitized
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S601 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @ntBre on 2025-07-04 19:12_

Yes, we'll definitely want to add tests!

I don't think this is quite the right approach. I'd prefer trying to resolve the type of the client. You can see an example of what I had in mind with the `is_fastapi_route` function (and other `is_*` functions in the same file):

https://github.com/astral-sh/ruff/blob/c3e307739b61b5881a9970b6071e935e786fbdc9/crates/ruff_python_semantic/src/analyze/typing.rs#L1095-L1097

That might actually miss some of the cases you're currently catching in the ecosystem check, but I think it would be the right trade-off. And it will automatically get better with improved type inference in the future.

---

_Label `rule` added by @ntBre on 2025-07-04 19:12_

---

_Comment by @CodeMan62 on 2025-07-05 12:19_

I have added test case and i am trying to follow the review 

---

_Comment by @CodeMan62 on 2025-07-05 16:07_

Tests added 
implemented as per the implementation of `FastApiRouteChecker` in `is_fastapi_route` function 
Note:- sorry for putting 2 extra commits for warning and fmt i forget to enable fmt in nvim config

---

_Comment by @CodeMan62 on 2025-07-09 17:09_

This needs a review 

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S613.py`:1 on 2025-07-09 17:33_

I'd recommend naming this file something like `S601_1.py` instead of the unrelated S613. We actually don't have a rule S613.

---

_@ntBre requested changes on 2025-07-09 17:34_

This is still not what I had in mind. I was suggesting implementing an inference method like `is_fastapi_route` and applying that to the receiver of the `exec_command` call. This doesn't really resemble that at all, and it still has the same issue reported initially in that it checks for `paramiko.exec_command`, which doesn't exist.

---

_Comment by @MichaReiser on 2025-08-18 07:35_

Thanks for your contribution. I'll close this due to inactivity.

---

_Closed by @MichaReiser on 2025-08-18 07:35_

---

```yaml
number: 6132
title: "[`perflint`] Add `PERF403`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PERF403
created_at: 2023-07-27T16:13:55Z
updated_at: 2023-09-15T15:32:08Z
url: https://github.com/astral-sh/ruff/pull/6132
synced_at: 2026-01-12T15:55:20Z
```

# [`perflint`] Add `PERF403`

---

_@qdegraaf_

## Summary

Reopening of https://github.com/astral-sh/ruff/pull/5313 

One open question as described in https://github.com/astral-sh/ruff/pull/5313#issuecomment-1645113101 

FYI previous reviewers: @MichaReiser @dhruvmanila @charliermarsh 

# Old Description
## Summary

Adds `PERF403` mirroring `W8403` from https://github.com/tonybaloney/perflint 

## Test Plan

Fixtures were added based on perflint tests

## Issue Links

Refers: https://github.com/astral-sh/ruff/issues/4789 



---

_Comment by @github-actions[bot] on 2023-07-27 16:47_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+22, -0, 0 error(s))

<details><summary>airflow (+5, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L163'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:163:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/exasol/hooks/exasol.py#L68'>airflow/providers/exasol/hooks/exasol.py:68:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/mysql/hooks/mysql.py#L170'>airflow/providers/mysql/hooks/mysql.py:170:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/postgres/hooks/postgres.py#L153'>airflow/providers/postgres/hooks/postgres.py:153:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/tests/providers/apache/spark/hooks/test_spark_submit.py#L72'>tests/providers/apache/spark/hooks/test_spark_submit.py:72:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>rotki (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/4aa415223dd9530d3e1aff76861a17add07aee13/rotkehlchen/api/rest.py#L506'>rotkehlchen/api/rest.py:506:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/rotki/rotki/blob/4aa415223dd9530d3e1aff76861a17add07aee13/rotkehlchen/globaldb/handler.py#L899'>rotkehlchen/globaldb/handler.py:899:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/rotki/rotki/blob/4aa415223dd9530d3e1aff76861a17add07aee13/rotkehlchen/tests/utils/database.py#L104'>rotkehlchen/tests/utils/database.py:104:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>sphinx (+13, -0)</summary>
<p>

<pre>
+ 
+    |
+    |             ^^^^^^^^^^^^ PERF403
+ 28 |     for info in reversed(list(request.node.iter_markers("apidoc"))):
+ 29 |         for i, a in enumerate(info.args):
+ 30 |             pargs[i] = a
+ 31 |         kwargs.update(info.kwargs)
+ 76 |     for info in reversed(list(request.node.iter_markers("sphinx"))):
+ 77 |         for i, a in enumerate(info.args):
+ 78 |             pargs[i] = a
+ 79 |         kwargs.update(info.kwargs)
+ <a href='https://github.com/sphinx-doc/sphinx/blob/f0c25a0adafe36eb987bae6ea13ed4a57d1c3bf0/sphinx/testing/fixtures.py#L78'>sphinx/testing/fixtures.py:78:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/sphinx-doc/sphinx/blob/f0c25a0adafe36eb987bae6ea13ed4a57d1c3bf0/tests/test_ext_apidoc.py#L30'>tests/test_ext_apidoc.py:30:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/db8229ae32475ec9d114d04a35a50c5096d27f2e/analytics/views/stats.py#L456'>analytics/views/stats.py:456:9:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PERF403 | 11 | 11 | 0 |



---

_Converted to draft by @qdegraaf on 2023-07-27 17:25_

---

_Comment by @charliermarsh on 2023-08-29 16:49_

We can probably implement this more confidently now by leveraging some of the basic type inference stuff added in the `refurb` module.

---

_Comment by @qdegraaf on 2023-08-29 17:42_

Awesome! I'll see if I can find some time next week to look at that and implement it in this PR

---

_Comment by @charliermarsh on 2023-08-29 18:09_

Sounds good! We'll need to pull the stuff out of `refurb/helpers.rs` into (probably) `ruff_python_semantic::analyzers::typing`.

---

_Comment by @qdegraaf on 2023-08-29 18:18_

Is that just a simple copy paste to `ruff_python_semantic` and reimport in the refurb module? If so I'll open a separate PR for that and then rebase this one. That will unblock other people from picking this up if they beat me to it and/or any other PRs where I'm sure basic type inference will be very useful.

---

_Comment by @qdegraaf on 2023-08-29 19:05_

I've added the dict check and also excluded if/elif/else comprehensions from the check. Upstream plugin does this as well (I misread the tests upon first implementation). Still need to find some time to implement the heuristic for checking if the dict is empty before the loop.

@charliermarsh For the if/elif/else check I misread upstream for PERF401 as well. Will open a small PR to fix that, can see if adding the type inference there (and PERF402 as well) has added value too.

EDIT: Someone beat me to it,. the fixtures were just still flagged wrong. 

---

_Label `rule` added by @charliermarsh on 2023-08-30 04:19_

---

_Marked ready for review by @charliermarsh on 2023-08-30 04:20_

---

_Merged by @charliermarsh on 2023-09-15 01:23_

---

_Closed by @charliermarsh on 2023-09-15 01:23_

---

_Branch deleted on 2023-09-15 15:32_

---

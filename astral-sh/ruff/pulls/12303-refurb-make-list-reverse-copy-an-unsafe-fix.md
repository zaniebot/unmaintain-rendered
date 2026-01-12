```yaml
number: 12303
title: "[`refurb`] Make `list-reverse-copy` an unsafe fix"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/furb187
created_at: 2024-07-12T13:13:33Z
updated_at: 2024-07-14T07:27:29Z
url: https://github.com/astral-sh/ruff/pull/12303
synced_at: 2026-01-12T15:55:40Z
```

# [`refurb`] Make `list-reverse-copy` an unsafe fix

---

_@charliermarsh_

## Summary

I don't know that there's more to do here. We could consider not raising the violation at all for arguments, but that would have some false negatives and could also be surprising to users.

Closes https://github.com/astral-sh/ruff/issues/12267.


---

_Label `fixes` added by @charliermarsh on 2024-07-12 13:13_

---

_Renamed from "Make `list-reverse-copy` an unsafe fix" to "[`refurb`] Make `list-reverse-copy` an unsafe fix" by @charliermarsh on 2024-07-12 13:13_

---

_Comment by @github-actions[bot] on 2024-07-12 13:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -2 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ea1812112dac254941e7ee0fa2e9b407e703d18b/airflow/jobs/backfill_job_runner.py#L943'>airflow/jobs/backfill_job_runner.py:943:13:</a> FURB187 Use of assignment of `reversed` on list `dagrun_infos`
- <a href='https://github.com/apache/airflow/blob/ea1812112dac254941e7ee0fa2e9b407e703d18b/airflow/jobs/backfill_job_runner.py#L943'>airflow/jobs/backfill_job_runner.py:943:13:</a> FURB187 [*] Use of assignment of `reversed` on list `dagrun_infos`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB187 | 2 | 0 | 0 | 0 | 2 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -2 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ea1812112dac254941e7ee0fa2e9b407e703d18b/airflow/jobs/backfill_job_runner.py#L943'>airflow/jobs/backfill_job_runner.py:943:13:</a> FURB187 Use of assignment of `reversed` on list `dagrun_infos`
- <a href='https://github.com/apache/airflow/blob/ea1812112dac254941e7ee0fa2e9b407e703d18b/airflow/jobs/backfill_job_runner.py#L943'>airflow/jobs/backfill_job_runner.py:943:13:</a> FURB187 [*] Use of assignment of `reversed` on list `dagrun_infos`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB187 | 2 | 0 | 0 | 0 | 2 |

</p>
</details>




---

_@MichaReiser approved on 2024-07-13 07:32_

Fair enough. Do we have other rules that apply fixes that mutate an object in place? These would also need to be marked as unsafe. It's not that we have to do this now. I'm mainly trying to understand the implication of marking such fixes as unsafe. 

We might be able to get better at this long-term with Red Knot, if it has a way to reason whether a binding is local (not an argument), and has no aliases (CC: @carljm )

---

_Comment by @charliermarsh on 2024-07-13 19:23_

Yeah, it's _possible_ that we could make this safe in some cases. Maybe if... there's only one binding, and there are no reads yet, and that binding is an assignment...?

---

_Merged by @charliermarsh on 2024-07-13 19:45_

---

_Closed by @charliermarsh on 2024-07-13 19:45_

---

_Branch deleted on 2024-07-13 19:45_

---

_Comment by @carljm on 2024-07-14 07:27_

Yes, it should be possible to find the cases where this is safe. They might turn out to not be very common in practice. I personally just don't like this lint rule at all, because in-place mutation is just fundamentally different, and that difference will be relevant in many practical cases. But making the autofix unsafe seems like a reasonable option. 

---

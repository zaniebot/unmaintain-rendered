```yaml
number: 22788
title: "fix: Fix false negative when sequence is an attribute"
type: pull_request
state: open
author: leandrodamascena
labels: []
assignees: []
base: main
head: fix/plc1802
created_at: 2026-01-21T14:29:04Z
updated_at: 2026-01-21T14:54:02Z
url: https://github.com/astral-sh/ruff/pull/22788
synced_at: 2026-01-21T15:05:46Z
```

# fix: Fix false negative when sequence is an attribute

---

_@leandrodamascena_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #22780

PLC1802 was not triggering when the sequence passed to len() was an attribute like `self.fruits`, for example. I had the similar issue in my code.

I extended `is_indirect_sequence` to handle Expr::Attribute in addition to Expr::Name.

This is my first contribution here, so, happy to make any change.

## Test Plan

Added test cases to `len_as_condition.py `and updated the snapshot. And I ran the exact example from the issue:

```sh
$ echo 'class Fruits:
    fruits: list[str]

    def __init__(self):
        self.fruits = ["apple", "orange"]

        if len(self.fruits):
            ...

fruits = ["apple", "orange"]
if len(fruits):
    ...' | cargo run -p ruff -- check --select PLC1802 -
```




---

_Comment by @astral-sh-bot[bot] on 2026-01-21 14:40_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a47b69e6c06ba906fb481d799089b59f5f09926d/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3220'>airflow-core/tests/unit/jobs/test_scheduler_job.py:3220:16:</a> PLC1802 [*] `len(dag_listener.success)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/a47b69e6c06ba906fb481d799089b59f5f09926d/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3220'>airflow-core/tests/unit/jobs/test_scheduler_job.py:3220:45:</a> PLC1802 [*] `len(dag_listener.failure)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a47b69e6c06ba906fb481d799089b59f5f09926d/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3220'>airflow-core/tests/unit/jobs/test_scheduler_job.py:3220:16:</a> PLC1802 [*] `len(dag_listener.success)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/a47b69e6c06ba906fb481d799089b59f5f09926d/airflow-core/tests/unit/jobs/test_scheduler_job.py#L3220'>airflow-core/tests/unit/jobs/test_scheduler_job.py:3220:45:</a> PLC1802 [*] `len(dag_listener.failure)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @ntBre on 2026-01-21 14:54_

Thanks for working on this! 

I commented something similar on the issue, but I'm a bit skeptical about making this change. I believe the proposed changes will only catch a very specific instance of this problem and could also result in new false positives. For example, how do your changes handle cases like:

```py
# Redefinition in a method
class C:
	def foo(self):
		self.field = []
		self.update_field()
		if len(self.field): ...

	def update_field(self):
		self.field = None

# Conditional definition
class C:
	def foo(self, field: SomethingElse | None = None):
		if self.field is None:
			self.field = []
		if len(self.field): ...
```

I don't think it's worth the added complexity in the rule code to handle such a small subset of attribute assignments. I also think we'll inherit ty's improved capabilities around resolving attributes in the future, so I'd be inclined to wait for that.

---

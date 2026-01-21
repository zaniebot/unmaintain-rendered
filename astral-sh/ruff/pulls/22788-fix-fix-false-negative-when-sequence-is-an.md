```yaml
number: 22788
title: "fix: Fix false negative when sequence is an attribute"
type: pull_request
state: closed
author: leandrodamascena
labels: []
assignees: []
base: main
head: fix/plc1802
created_at: 2026-01-21T14:29:04Z
updated_at: 2026-01-21T15:48:39Z
url: https://github.com/astral-sh/ruff/pull/22788
synced_at: 2026-01-21T16:04:59Z
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

_Comment by @leandrodamascena on 2026-01-21 14:58_

I went and tested both cases:

Case 1 (redefinition): My implementation triggers a false positive here - it sees self.field = [] and flags len(self.field), but doesn't account for update_field() potentially changing it to None.

Case 2 (conditional): Doesn't trigger (good), but only by accident since the assignment is inside an if block.

I didn't think about this in the beginner and probably the added complexity isn't worth it for such a narrow fix that can still produce false positives. 

I think makes sense to wait for ty's improved attribute resolution. 

Do you want me to close this PR?

BTW, do you have any other easy/medium complexity issues to work? I'd like to contribute with the project oftern :)

---

_Comment by @ntBre on 2026-01-21 15:48_

Thanks for taking a look at the test cases and again for working on this! Yeah let's go ahead and close this for now, but I'm open to other opinions if others support this change :) @amyreese triaged the issue, so she may want to weigh in.

> BTW, do you have any other easy/medium complexity issues to work? I'd like to contribute with the project oftern :)

Anything with the [help wanted](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20is%3Aopen%20label%3A%22help%20wanted%22) or [good first issue](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20is%3Aopen%20label%3A%22good%20first%20issue%22) labels without someone assigned or an open PR is a good place to start, although we may be a bit light on those at the moment. The [bug](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20is%3Aopen%20label%3Abug) label would be the next best place to look, avoiding any with `type-inference` or `needs-decision` labels.

I'm a bit biased, but there are two tracking issues that I'd be glad to review PRs for too: https://github.com/astral-sh/ruff/issues/15642 and https://github.com/astral-sh/ruff/issues/17203. Feel free to ping me on any issues that look interesting, if you need advice on where to start.


---

_Closed by @ntBre on 2026-01-21 15:48_

---

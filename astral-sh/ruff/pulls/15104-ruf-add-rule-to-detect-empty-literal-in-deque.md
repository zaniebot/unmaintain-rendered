```yaml
number: 15104
title: "[`RUF`] Add rule to detect empty literal in deque call (`RUF025`)"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: unnecessary_deque_literal
created_at: 2024-12-22T23:42:44Z
updated_at: 2025-01-04T12:52:33Z
url: https://github.com/astral-sh/ruff/pull/15104
synced_at: 2026-01-12T15:55:50Z
```

# [`RUF`] Add rule to detect empty literal in deque call (`RUF025`)

---

_@w0nder1ng_

## Summary

The idea comes from #13515. When you create a `collections.deque`, it will be empty by default. You can pass an iterable as the first argument, but if the iterable is empty, then it is unnecessary.

```python
from collections import deque

queue = deque([], 5)
```

Becomes:

```python
from collections import deque

queue = deque(maxlen=5)
```

## Test plan

I added test cases for most of the obvious ways this should apply.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_deque_call.rs`:65 on 2024-12-23 09:08_

I think we should bail if there are more than 2 arguments to avoid accidentally removing arguments. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:386 on 2024-12-23 09:10_

We should either open an upstream issue and see if the rule gets accepted and implemented or implement the Rule as a RUFF rule. We otherwise risk that we end up with incompatible rule codes between upstream and ruff (if flake8-comprehension adds a new rule that's not `UnnecessaryLiteralWithinDequeueCall`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_deque_call.rs`:30 on 2024-12-23 09:11_

I'm leaning towards

```suggestion
pub(crate) struct UnnecessaryEmptyIterableWithinDequeCall {
```

---

_@MichaReiser reviewed on 2024-12-23 09:12_

Nice thanks

I think we have to rename the rule to avoid naming collisions with flake8-comprehensions in the future

---

_Label `rule` added by @dhruvmanila on 2024-12-24 05:30_

---

_Label `preview` added by @dhruvmanila on 2024-12-24 05:30_

---

_Comment by @github-actions[bot] on 2025-01-03 09:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/training/test_interactive.py#L663'>tests/core/training/test_interactive.py:663:58:</a> RUF025 [*] Unnecessary empty iterable within a deque call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6f9cf5a4f3f4ed31d5353433203f7afc291ef672/tests/jobs/test_scheduler_job.py#L5887'>tests/jobs/test_scheduler_job.py:5887:38:</a> RUF025 [*] Unnecessary empty iterable within a deque call
+ <a href='https://github.com/apache/airflow/blob/6f9cf5a4f3f4ed31d5353433203f7afc291ef672/tests/jobs/test_scheduler_job.py#L5888'>tests/jobs/test_scheduler_job.py:5888:43:</a> RUF025 [*] Unnecessary empty iterable within a deque call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF025 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "[`flake8_comprehensions`] Add rule to detect empty literal in deque call" to "[`RUF`] Add rule to detect empty literal in deque call (`RUF025`)" by @MichaReiser on 2025-01-03 10:00_

---

_Merged by @MichaReiser on 2025-01-03 10:57_

---

_Closed by @MichaReiser on 2025-01-03 10:57_

---

_Comment by @oprypin on 2025-01-04 12:52_

I opened an issue with this PR: 
* #15255

---

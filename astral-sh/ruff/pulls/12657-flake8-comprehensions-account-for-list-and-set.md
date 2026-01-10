```yaml
number: 12657
title: "[flake8-comprehensions] Account for list and set comprehensions in `unnecessary-literal-within-tuple-call` (`C409`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: tuple-comprehensions
created_at: 2024-08-04T00:31:29Z
updated_at: 2024-08-15T23:04:30Z
url: https://github.com/astral-sh/ruff/pull/12657
synced_at: 2026-01-10T21:38:31Z
```

# [flake8-comprehensions] Account for list and set comprehensions in `unnecessary-literal-within-tuple-call` (`C409`)

---

_Pull request opened by @dylwil3 on 2024-08-04 00:31_

## Summary

Make it a violation of `C409` to call `tuple` with a list or set comprehension, and
implement the (unsafe) fix of calling the `tuple` with the underlying generator instead.

Closes #12648.

## Test Plan

Test fixture updated, cargo test, docs checked for updated description.


---

_Comment by @codspeed-hq[bot] on 2024-08-04 00:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3:tuple-comprehensions)

### Merging #12657 will **not alter performance**

<sub>Comparing <code>dylwil3:tuple-comprehensions</code> (6862cd3) with <code>main</code> (d6c6db5)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-04 00:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1546d7fafd549eab8b01b8f08104d2a1a9b956d3/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L259'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:259:41:</a> C409 Unnecessary list comprehension passed to `tuple()` (rewrite as a generator)
+ <a href='https://github.com/apache/airflow/blob/1546d7fafd549eab8b01b8f08104d2a1a9b956d3/dev/breeze/src/airflow_breeze/utils/packages.py#L374'>dev/breeze/src/airflow_breeze/utils/packages.py:374:12:</a> C409 Unnecessary list comprehension passed to `tuple()` (rewrite as a generator)
+ <a href='https://github.com/apache/airflow/blob/1546d7fafd549eab8b01b8f08104d2a1a9b956d3/tests/_internals/capture_warnings.py#L187'>tests/_internals/capture_warnings.py:187:55:</a> C409 Unnecessary list comprehension passed to `tuple()` (rewrite as a generator)
+ <a href='https://github.com/apache/airflow/blob/1546d7fafd549eab8b01b8f08104d2a1a9b956d3/tests/always/test_example_dags.py#L132'>tests/always/test_example_dags.py:132:25:</a> C409 Unnecessary list comprehension passed to `tuple()` (rewrite as a generator)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/5d90fd2e26155fa0c9089dd92b3b034f83f42c83/latch/functions/operators.py#L106'>latch/functions/operators.py:106:27:</a> C409 Unnecessary list comprehension passed to `tuple()` (rewrite as a generator)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/a98e1a9d2edcff82cca19bfa0ddd4dd133096e68/tests/utils/test_uri.py#L194'>tests/utils/test_uri.py:194:20:</a> C409 Unnecessary list comprehension passed to `tuple()` (rewrite as a generator)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/50d0d047eb3dedf3b801c3731a37d56c320bad3e/mypy/checkpattern.py#L390'>mypy/checkpattern.py:390:17:</a> C409 Unnecessary list comprehension passed to `tuple()` (rewrite as a generator)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C409 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @Skylion007 on `crates/ruff_linter/src/rules/flake8_comprehensions/snapshots/ruff_linter__rules__flake8_comprehensions__tests__C409_C409.py.snap`:275 on 2024-08-04 14:28_

This is not a safe transform, a set could be used here for deduplicating elements in the iterable before the tuple is created. I'm pretty sure this is only safe / sensible to flag for list comprehensions.

---

_@Skylion007 reviewed on 2024-08-04 14:28_

---

_Comment by @Skylion007 on 2024-08-04 14:32_

I'd argue this is only safe to flag for list comprehensions as set comprehensions will dedup and potentially reorder elements as well (not that you should rely on that nondeterm ordering but still).

---

_Comment by @dylwil3 on 2024-08-04 17:47_

Good point! Another thing I noticed is that the suggestion text doesn't make sense in the context of comprehensions. I'll fix both next time I'm in front of a computer.

Thanks!

---

_Comment by @dylwil3 on 2024-08-05 01:37_

Ok, should be good to go!

---

_@charliermarsh approved on 2024-08-05 02:08_

Thanks! (FYI I had to move this behavior to preview to adhere to our versioning policy since it's an expansion in scope.)

---

_Merged by @charliermarsh on 2024-08-05 02:14_

---

_Closed by @charliermarsh on 2024-08-05 02:14_

---

_Label `rule` added by @charliermarsh on 2024-08-05 02:15_

---

_Label `preview` added by @charliermarsh on 2024-08-05 02:15_

---

_Branch deleted on 2024-08-05 11:12_

---

_Comment by @hauntsaninja on 2024-08-15 23:02_

I find this kind of thing unfortunate, because `tuple(i for i in range(10000))` is like 50% slower than `tuple([i for i in range(10000)])`.

There's no way to configure ruff to disambiguate between the two.
Edit: I should probably open a new issue https://github.com/astral-sh/ruff/issues/12912 :-)

---

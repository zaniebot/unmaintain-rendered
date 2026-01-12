```yaml
number: 19339
title: "[`refurb`] Ignore decorated functions for `FURB118`"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-19305
created_at: 2025-07-14T21:00:59Z
updated_at: 2025-07-25T15:43:42Z
url: https://github.com/astral-sh/ruff/pull/19339
synced_at: 2026-01-12T15:56:37Z
```

# [`refurb`] Ignore decorated functions for `FURB118`

---

_@danparizher_

## Summary

Fixes #19305

---

_Comment by @github-actions[bot] on 2025-07-14 21:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -16 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/airflow-core/tests/unit/decorators/test_mapped.py#L34'>airflow-core/tests/unit/decorators/test_mapped.py:34:13:</a> FURB118 Use `operator.add` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/airflow-core/tests/unit/ti_deps/deps/test_mapped_task_upstream_dep.py#L113'>airflow-core/tests/unit/ti_deps/deps/test_mapped_task_upstream_dep.py:113:13:</a> FURB118 Use `operator.add` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/airflow-core/tests/unit/ti_deps/deps/test_mapped_task_upstream_dep.py#L199'>airflow-core/tests/unit/ti_deps/deps/test_mapped_task_upstream_dep.py:199:13:</a> FURB118 Use `operator.add` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/providers/amazon/tests/system/amazon/aws/example_ec2.py#L105'>providers/amazon/tests/system/amazon/aws/example_ec2.py:105:5:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/providers/amazon/tests/system/amazon/aws/example_emr.py#L128'>providers/amazon/tests/system/amazon/aws/example_emr.py:128:5:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/providers/amazon/tests/unit/amazon/aws/hooks/test_rds.py#L104'>providers/amazon/tests/unit/amazon/aws/hooks/test_rds.py:104:5:</a> FURB118 Use `operator.itemgetter("DBClusterSnapshotIdentifier")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/providers/amazon/tests/unit/amazon/aws/hooks/test_rds.py#L82'>providers/amazon/tests/unit/amazon/aws/hooks/test_rds.py:82:5:</a> FURB118 Use `operator.itemgetter("DBSnapshotIdentifier")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/providers/amazon/tests/unit/amazon/aws/hooks/test_rds.py#L87'>providers/amazon/tests/unit/amazon/aws/hooks/test_rds.py:87:5:</a> FURB118 Use `operator.itemgetter("DBSnapshotArn")` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/providers/standard/tests/unit/standard/decorators/test_python.py#L742'>providers/standard/tests/unit/standard/decorators/test_python.py:742:9:</a> FURB118 Use `operator.mul` instead of defining a function
- <a href='https://github.com/apache/airflow/blob/b7ce1757118dbb24ee21ef88c36d9bbb1f87bf94/task-sdk/tests/task_sdk/definitions/test_mappedoperator.py#L526'>task-sdk/tests/task_sdk/definitions/test_mappedoperator.py:526:9:</a> FURB118 Use `operator.add` instead of defining a function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/769b06a975aad2dd9a34c696f09bc1883f555bda/astropy/io/fits/hdu/compressed/tests/conftest.py#L100'>astropy/io/fits/hdu/compressed/tests/conftest.py:100:5:</a> FURB118 Use `operator.itemgetter(1)` instead of defining a function
- <a href='https://github.com/astropy/astropy/blob/769b06a975aad2dd9a34c696f09bc1883f555bda/astropy/io/fits/hdu/compressed/tests/conftest.py#L105'>astropy/io/fits/hdu/compressed/tests/conftest.py:105:5:</a> FURB118 Use `operator.itemgetter(2)` instead of defining a function
- <a href='https://github.com/astropy/astropy/blob/769b06a975aad2dd9a34c696f09bc1883f555bda/astropy/io/fits/hdu/compressed/tests/conftest.py#L95'>astropy/io/fits/hdu/compressed/tests/conftest.py:95:5:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
- <a href='https://github.com/astropy/astropy/blob/769b06a975aad2dd9a34c696f09bc1883f555bda/astropy/io/fits/hdu/compressed/tests/test_fitsio.py#L90'>astropy/io/fits/hdu/compressed/tests/test_fitsio.py:90:5:</a> FURB118 Use `operator.itemgetter(1)` instead of defining a function
- <a href='https://github.com/astropy/astropy/blob/769b06a975aad2dd9a34c696f09bc1883f555bda/astropy/io/fits/hdu/compressed/tests/test_fitsio.py#L95'>astropy/io/fits/hdu/compressed/tests/test_fitsio.py:95:5:</a> FURB118 Use `operator.itemgetter(0)` instead of defining a function
- <a href='https://github.com/astropy/astropy/blob/769b06a975aad2dd9a34c696f09bc1883f555bda/astropy/modeling/tests/test_separable.py#L189'>astropy/modeling/tests/test_separable.py:189:9:</a> FURB118 Use `operator.add` instead of defining a function
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB118 | 16 | 0 | 16 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-07-15 16:55_

---

_Label `bug` added by @ntBre on 2025-07-15 16:55_

---

_Label `rule` added by @ntBre on 2025-07-15 16:55_

---

_Renamed from "[`refurb`] Ignore decorated functions for FURB118" to "[`refurb`] Ignore decorated functions for `FURB118`" by @dylwil3 on 2025-07-25 15:41_

---

_@dylwil3 approved on 2025-07-25 15:43_

Excellent, thank you!

---

_Merged by @dylwil3 on 2025-07-25 15:43_

---

_Closed by @dylwil3 on 2025-07-25 15:43_

---

_Branch deleted on 2025-07-25 15:43_

---

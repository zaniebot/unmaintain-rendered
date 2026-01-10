```yaml
number: 20229
title: "Stabilize `os-symlink` (`PTH211`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/0.13.0
head: brent/pth211
created_at: 2025-09-04T13:22:59Z
updated_at: 2025-09-04T20:42:17Z
url: https://github.com/astral-sh/ruff/pull/20229
synced_at: 2026-01-10T17:46:21Z
```

# Stabilize `os-symlink` (`PTH211`)

---

_Pull request opened by @ntBre on 2025-09-04 13:22_

Summary
--

Rule and test/snapshot updated, the docs look good

My one hesitation here is that we could hold off stabilizing the rule until its fix is also ready for stabilization, but this is also the only preview PTH rule, so I think it's okay to stabilize the rule and later (probably in the next minor release) stabilize the fixes together.


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 13:23_

---

_Renamed from "Stabilize `PTH211`" to "Stabilize `os-symlink` (`PTH211`)" by @ntBre on 2025-09-04 13:24_

---

_Label `rule` added by @ntBre on 2025-09-04 13:24_

---

_Comment by @github-actions[bot] on 2025-09-04 13:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/airflow-core/src/airflow/dag_processing/manager.py#L854'>airflow-core/src/airflow/dag_processing/manager.py:854:25:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/airflow-core/src/airflow/dag_processing/manager.py#L860'>airflow-core/src/airflow/dag_processing/manager.py:860:21:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/airflow-core/src/airflow/utils/log/file_processor_handler.py#L127'>airflow-core/src/airflow/utils/log/file_processor_handler.py:127:25:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/airflow-core/src/airflow/utils/log/file_processor_handler.py#L133'>airflow-core/src/airflow/utils/log/file_processor_handler.py:133:21:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/airflow-core/tests/unit/utils/test_file.py#L107'>airflow-core/tests/unit/utils/test_file.py:107:9:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/airflow-core/tests/unit/utils/test_file.py#L165'>airflow-core/tests/unit/utils/test_file.py:165:9:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/scripts/ci/prek/check_shared_distributions_usage.py#L224'>scripts/ci/prek/check_shared_distributions_usage.py:224:17:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e9a895cee533dd91556f84baba06f60bbd75ece6/scripts/ci/prek/check_shared_distributions_usage.py#L244'>scripts/ci/prek/check_shared_distributions_usage.py:244:17:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b4e06a6fa01b81e53e33c0d9f458bebde12ac5bc/scripts/lib/puppet_cache.py#L48'>scripts/lib/puppet_cache.py:48:5:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/zulip/zulip/blob/b4e06a6fa01b81e53e33c0d9f458bebde12ac5bc/scripts/lib/zulip_tools.py#L55'>scripts/lib/zulip_tools.py:55:13:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/zulip/zulip/blob/b4e06a6fa01b81e53e33c0d9f458bebde12ac5bc/tools/lib/provision.py#L59'>tools/lib/provision.py:59:5:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/f1867d8b95241074256d45584a8777eb94864d44/astropy/config/paths.py#L210'>astropy/config/paths.py:210:17:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH211 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-09-04 14:33_

---

_Review requested from @dylwil3 by @ntBre on 2025-09-04 14:33_

---

_@dylwil3 approved on 2025-09-04 20:16_

I agree we can stabilize the fixes together in the next minor

---

_Merged by @ntBre on 2025-09-04 20:42_

---

_Closed by @ntBre on 2025-09-04 20:42_

---

_Branch deleted on 2025-09-04 20:42_

---

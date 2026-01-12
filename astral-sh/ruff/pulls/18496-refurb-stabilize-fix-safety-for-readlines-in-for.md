```yaml
number: 18496
title: "[`refurb`] Stabilize fix safety for `readlines-in-for` (`FURB129`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - fixes
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-readlines-fix-safe
created_at: 2025-06-06T13:57:16Z
updated_at: 2025-06-06T17:01:38Z
url: https://github.com/astral-sh/ruff/pull/18496
synced_at: 2026-01-12T15:56:20Z
```

# [`refurb`] Stabilize fix safety for `readlines-in-for` (`FURB129`)

---

_@dylwil3_

Note that the preview behavior was not documented (shame on us!) so the documentation was not modified.

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 13:57_

---

_Label `rule` added by @dylwil3 on 2025-06-06 13:57_

---

_Label `fixes` added by @dylwil3 on 2025-06-06 13:57_

---

_@dylwil3 reviewed on 2025-06-06 13:58_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/preview.rs`:132 on 2025-06-06 13:58_

Unrelated to the main change: just noticed this was missing a PR link and the prefix `is_`. This also accounts for the change in `flake8_simplify/rules/ast_with.rs`

---

_Comment by @github-actions[bot] on 2025-06-06 14:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +10 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/dev/airflow_perf/sql_queries.py#L145'>dev/airflow_perf/sql_queries.py:145:41:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/dev/airflow_perf/sql_queries.py#L145'>dev/airflow_perf/sql_queries.py:145:41:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/dev/breeze/src/airflow_breeze/global_constants.py#L555'>dev/breeze/src/airflow_breeze/global_constants.py:555:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/dev/breeze/src/airflow_breeze/global_constants.py#L555'>dev/breeze/src/airflow_breeze/global_constants.py:555:21:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/devel-common/src/sphinx_exts/redirects.py#L44'>devel-common/src/sphinx_exts/redirects.py:44:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/devel-common/src/sphinx_exts/redirects.py#L44'>devel-common/src/sphinx_exts/redirects.py:44:21:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/providers/google/tests/system/google/cloud/gcs/resources/transform_script.py#L26'>providers/google/tests/system/google/cloud/gcs/resources/transform_script.py:26:39:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/providers/google/tests/system/google/cloud/gcs/resources/transform_script.py#L26'>providers/google/tests/system/google/cloud/gcs/resources/transform_script.py:26:39:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
- <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/scripts/ci/pre_commit/newsfragments.py#L36'>scripts/ci/pre_commit/newsfragments.py:36:43:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/ff17dc6790d113c053f4a57845a9ea95b06f056a/scripts/ci/pre_commit/newsfragments.py#L36'>scripts/ci/pre_commit/newsfragments.py:36:43:</a> FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB129 | 10 | 0 | 0 | 10 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-06-06 14:42_

Ecosystem check looks good to me!

---

_@ntBre approved on 2025-06-06 16:02_

LGTM, thanks!

Will you add these to the PR summary as you merge them? I'm also a bit paranoid about last time. What do you think about building up a Python code block and ruff invocation in the PR summary that makes sure that everything we stabilized makes it into the release? I'm picturing something like this for this case:

```python
with open("file.txt") as fp:
    for line in fp.readlines():  # FURB129
        ...
```

```shell
ruff check example.py --no-cache \
    --select FURB129 # room for more \  --select ... later
```

---

_Comment by @dylwil3 on 2025-06-06 17:00_

Sounds good! We may need multiple code blocks in the end because for some stabilized behvaiors the file name matters (e.g. one of the behaviors cares whether the file is `__init__.py`).

---

_Merged by @dylwil3 on 2025-06-06 17:01_

---

_Closed by @dylwil3 on 2025-06-06 17:01_

---

_Branch deleted on 2025-06-06 17:01_

---

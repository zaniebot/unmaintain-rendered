```yaml
number: 18337
title: "[flake8_use_pathlib]: Replace os.symlink with Path.symlink_to (PTH211)"
type: pull_request
state: merged
author: fennr
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/pathlib_symlink
created_at: 2025-05-27T18:01:01Z
updated_at: 2025-05-28T10:39:06Z
url: https://github.com/astral-sh/ruff/pull/18337
synced_at: 2026-01-10T18:51:02Z
```

# [flake8_use_pathlib]: Replace os.symlink with Path.symlink_to (PTH211)

---

_Pull request opened by @fennr on 2025-05-27 18:01_

## Summary

/closes https://github.com/astral-sh/ruff/issues/18250

## Test Plan

Add snapshot


---

_Label `rule` added by @MichaReiser on 2025-05-28 07:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:936 on 2025-05-28 07:33_

```suggestion
        (Flake8UsePathlib, "211") => (RuleGroup::Preview, rules::flake8_use_pathlib::violations::OsSymlink),
```

---

_@MichaReiser reviewed on 2025-05-28 07:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:206 on 2025-05-28 07:36_

We should avoid raising this rule when `fd` is set to a non-None` value (see chmod for an example)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/violations.rs`:1237 on 2025-05-28 07:37_

I think it should be the opposite. But please verify it locally
```suggestion
/// Path("tmp/python").symlink_to("usr/bin/python")
```

---

_@MichaReiser requested changes on 2025-05-28 07:38_

Thanks. I think there's an error in the example and we have to account for symlink calls with an `fd` argument

---

_@fennr reviewed on 2025-05-28 08:21_

---

_Review comment by @fennr on `crates/ruff_linter/src/rules/flake8_use_pathlib/violations.rs`:1237 on 2025-05-28 08:21_

You are right. Corrected.

---

_@fennr reviewed on 2025-05-28 08:22_

---

_Review comment by @fennr on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:206 on 2025-05-28 08:22_

 Added check and test

---

_Review requested from @MichaReiser by @fennr on 2025-05-28 08:23_

---

_@MichaReiser approved on 2025-05-28 09:11_

---

_Comment by @github-actions[bot] on 2025-05-28 09:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+10 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6d366c8051dbe2c988f47f293d309ea92785e7a9/airflow-core/src/airflow/dag_processing/manager.py#L845'>airflow-core/src/airflow/dag_processing/manager.py:845:25:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/6d366c8051dbe2c988f47f293d309ea92785e7a9/airflow-core/src/airflow/dag_processing/manager.py#L851'>airflow-core/src/airflow/dag_processing/manager.py:851:21:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/6d366c8051dbe2c988f47f293d309ea92785e7a9/airflow-core/src/airflow/utils/log/file_processor_handler.py#L127'>airflow-core/src/airflow/utils/log/file_processor_handler.py:127:25:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/6d366c8051dbe2c988f47f293d309ea92785e7a9/airflow-core/src/airflow/utils/log/file_processor_handler.py#L133'>airflow-core/src/airflow/utils/log/file_processor_handler.py:133:21:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/6d366c8051dbe2c988f47f293d309ea92785e7a9/airflow-core/tests/unit/utils/test_file.py#L107'>airflow-core/tests/unit/utils/test_file.py:107:9:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/6d366c8051dbe2c988f47f293d309ea92785e7a9/airflow-core/tests/unit/utils/test_file.py#L167'>airflow-core/tests/unit/utils/test_file.py:167:9:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/26595799c7c2d0a8f8bc7c15de5dcc813fff93c9/scripts/lib/puppet_cache.py#L48'>scripts/lib/puppet_cache.py:48:5:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/zulip/zulip/blob/26595799c7c2d0a8f8bc7c15de5dcc813fff93c9/scripts/lib/zulip_tools.py#L55'>scripts/lib/zulip_tools.py:55:13:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/zulip/zulip/blob/26595799c7c2d0a8f8bc7c15de5dcc813fff93c9/tools/lib/provision.py#L59'>tools/lib/provision.py:59:5:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/2dab001c19df0d3a1f3f187f7fe70141e2c5fa24/astropy/config/paths.py#L214'>astropy/config/paths.py:214:17:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH211 | 10 | 10 | 0 | 0 | 0 |

</p>
</details>




---

_Label `preview` added by @MichaReiser on 2025-05-28 10:37_

---

_Merged by @MichaReiser on 2025-05-28 10:39_

---

_Closed by @MichaReiser on 2025-05-28 10:39_

---

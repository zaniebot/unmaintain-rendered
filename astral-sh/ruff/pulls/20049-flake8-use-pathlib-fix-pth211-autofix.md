```yaml
number: 20049
title: "[`flake8-use-pathlib`] Fix `PTH211` autofix"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pth211
created_at: 2025-08-22T18:21:13Z
updated_at: 2025-08-22T18:36:45Z
url: https://github.com/astral-sh/ruff/pull/20049
synced_at: 2026-01-12T15:56:53Z
```

# [`flake8-use-pathlib`] Fix `PTH211` autofix

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Part of #20009

## Test Plan
`cargo nextest run flake8_use_pathlib`


---

_Comment by @chirizxc on 2025-08-22 18:22_

@dylwil3 this [commit](https://github.com/astral-sh/ruff/pull/20009/commits/994f983d9643bbc3f3cc7bc03a56c396bab4998a) caused the misbehavior

---

_Comment by @chirizxc on 2025-08-22 18:25_

I think we can check if `target_is_directory` is actually a boolean before, as in this PR

---

_@dylwil3 requested changes on 2025-08-22 18:28_

Wow good catch - I guess I got confused about the preview vs non-preview snapshots or something... my bad!

---

_Comment by @github-actions[bot] on 2025-08-22 18:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +24 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/dag_processing/manager.py#L858'>airflow-core/src/airflow/dag_processing/manager.py:858:25:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/dag_processing/manager.py#L858'>airflow-core/src/airflow/dag_processing/manager.py:858:25:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/dag_processing/manager.py#L864'>airflow-core/src/airflow/dag_processing/manager.py:864:21:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/dag_processing/manager.py#L864'>airflow-core/src/airflow/dag_processing/manager.py:864:21:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/utils/log/file_processor_handler.py#L127'>airflow-core/src/airflow/utils/log/file_processor_handler.py:127:25:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/utils/log/file_processor_handler.py#L127'>airflow-core/src/airflow/utils/log/file_processor_handler.py:127:25:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/utils/log/file_processor_handler.py#L133'>airflow-core/src/airflow/utils/log/file_processor_handler.py:133:21:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/src/airflow/utils/log/file_processor_handler.py#L133'>airflow-core/src/airflow/utils/log/file_processor_handler.py:133:21:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/tests/unit/utils/test_file.py#L107'>airflow-core/tests/unit/utils/test_file.py:107:9:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/tests/unit/utils/test_file.py#L107'>airflow-core/tests/unit/utils/test_file.py:107:9:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/tests/unit/utils/test_file.py#L165'>airflow-core/tests/unit/utils/test_file.py:165:9:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/airflow-core/tests/unit/utils/test_file.py#L165'>airflow-core/tests/unit/utils/test_file.py:165:9:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/scripts/ci/prek/check_shared_distributions_usage.py#L204'>scripts/ci/prek/check_shared_distributions_usage.py:204:17:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/scripts/ci/prek/check_shared_distributions_usage.py#L204'>scripts/ci/prek/check_shared_distributions_usage.py:204:17:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/scripts/ci/prek/check_shared_distributions_usage.py#L224'>scripts/ci/prek/check_shared_distributions_usage.py:224:17:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/apache/airflow/blob/e790409e0c5888b94a69acfc60a01747032eab31/scripts/ci/prek/check_shared_distributions_usage.py#L224'>scripts/ci/prek/check_shared_distributions_usage.py:224:17:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/puppet_cache.py#L48'>scripts/lib/puppet_cache.py:48:5:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/puppet_cache.py#L48'>scripts/lib/puppet_cache.py:48:5:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L55'>scripts/lib/zulip_tools.py:55:13:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/scripts/lib/zulip_tools.py#L55'>scripts/lib/zulip_tools.py:55:13:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
+ <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/provision.py#L59'>tools/lib/provision.py:59:5:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/zulip/zulip/blob/533f177175f941248d611bcdb6f648064361ddc6/tools/lib/provision.py#L59'>tools/lib/provision.py:59:5:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/1fa72f146ea7042cd8176eaabef2412efa6624ff/astropy/config/paths.py#L210'>astropy/config/paths.py:210:17:</a> PTH211 [*] `os.symlink` should be replaced by `Path.symlink_to`
- <a href='https://github.com/astropy/astropy/blob/1fa72f146ea7042cd8176eaabef2412efa6624ff/astropy/config/paths.py#L210'>astropy/config/paths.py:210:17:</a> PTH211 `os.symlink` should be replaced by `Path.symlink_to`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH211 | 24 | 0 | 0 | 24 | 0 |

</p>
</details>




---

_@dylwil3 approved on 2025-08-22 18:34_

Alright, thank you this looks good!

---

_Label `fixes` added by @dylwil3 on 2025-08-22 18:34_

---

_Label `preview` added by @dylwil3 on 2025-08-22 18:34_

---

_Renamed from "[flake8-use-pathlib] Fix `PTH211` autofix" to "[`flake8-use-pathlib`] Fix `PTH211` autofix" by @dylwil3 on 2025-08-22 18:34_

---

_Merged by @dylwil3 on 2025-08-22 18:35_

---

_Closed by @dylwil3 on 2025-08-22 18:35_

---

_Branch deleted on 2025-08-22 18:36_

---

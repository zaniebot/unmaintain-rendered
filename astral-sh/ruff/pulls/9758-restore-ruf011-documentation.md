```yaml
number: 9758
title: Restore RUF011 documentation
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: release/0.2.0
head: zb/ruf001
created_at: 2024-02-01T16:43:10Z
updated_at: 2024-02-01T17:56:19Z
url: https://github.com/astral-sh/ruff/pull/9758
synced_at: 2026-01-12T15:55:30Z
```

# Restore RUF011 documentation

---

_@zanieb_

For consistency with other redirected rules as in https://github.com/astral-sh/ruff/pull/9755

Follow-up to #9428 


---

_Label `internal` added by @zanieb on 2024-02-01 16:43_

---

_@zanieb reviewed on 2024-02-01 16:44_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/ruff/rules/static_key_dict_comprehension.rs`:30 on 2024-02-01 16:44_

Renamed to avoid docs link collission

---

_@charliermarsh reviewed on 2024-02-01 16:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/mod.rs`:48 on 2024-02-01 16:45_

(I think this should be in the section above.)

---

_@charliermarsh approved on 2024-02-01 16:45_

---

_@zanieb reviewed on 2024-02-01 16:46_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/ruff/rules/mod.rs`:48 on 2024-02-01 16:46_

Was fixing :)

---

_Merged by @zanieb on 2024-02-01 17:48_

---

_Closed by @zanieb on 2024-02-01 17:48_

---

_Branch deleted on 2024-02-01 17:48_

---

_Comment by @github-actions[bot] on 2024-02-01 17:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 2 projects; 2 project errors; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/dev/stats/get_important_pr_candidates.py#L131'>dev/stats/get_important_pr_candidates.py:131:32:</a> PGH001 No builtin `eval()` allowed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/0db47322595f93065997c990920b3b89475a1f9e/ibis/common/typing.py#L206'>ibis/common/typing.py:206:69:</a> RUF100 Unused `noqa` directive (non-enabled: `S307`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Rule `PGH002` was removed and cannot be selected.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
warning: `RUF011` has been remapped to `B035`.
warning: `TCH006` has been remapped to `TCH010`.
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB131
	- FURB113
	- FURB132
```

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |
| PGH001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 2 projects; 2 project errors; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/fb27898a7706e2e38cf49aaea1a215afb4ca57f3/dev/stats/get_important_pr_candidates.py#L131'>dev/stats/get_important_pr_candidates.py:131:32:</a> PGH001 No builtin `eval()` allowed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/0db47322595f93065997c990920b3b89475a1f9e/ibis/common/typing.py#L206'>ibis/common/typing.py:206:69:</a> RUF100 Unused `noqa` directive (non-enabled: `S307`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Rule `PGH002` was removed and cannot be selected.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
warning: `RUF011` has been remapped to `B035`.
warning: `TCH006` has been remapped to `TCH010`.
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |
| PGH001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

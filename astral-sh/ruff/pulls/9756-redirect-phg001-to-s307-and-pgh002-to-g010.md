```yaml
number: 9756
title: "Redirect `PHG001` to `S307` and `PGH002` to `G010`"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
assignees: []
merged: true
base: release/0.2.0
head: zb/pgh-redirect
created_at: 2024-02-01T15:56:41Z
updated_at: 2024-02-01T17:47:47Z
url: https://github.com/astral-sh/ruff/pull/9756
synced_at: 2026-01-12T15:55:30Z
```

# Redirect `PHG001` to `S307` and `PGH002` to `G010`

---

_@zanieb_

Follow-up to #9754 and #9689. Alternative to #9714.
Replaces #7506 and #7507
Same ideas as #9755
Part of #8931 

---

_Review requested from @charliermarsh by @zanieb on 2024-02-01 15:58_

---

_Label `rule` added by @zanieb on 2024-02-01 15:58_

---

_@charliermarsh approved on 2024-02-01 15:58_

---

_Merged by @zanieb on 2024-02-01 17:40_

---

_Closed by @zanieb on 2024-02-01 17:40_

---

_Branch deleted on 2024-02-01 17:40_

---

_Comment by @github-actions[bot] on 2024-02-01 17:47_

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

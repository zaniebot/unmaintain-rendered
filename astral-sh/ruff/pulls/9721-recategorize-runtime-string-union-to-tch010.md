```yaml
number: 9721
title: "Recategorize `runtime-string-union` to `TCH010`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: release/0.2.0
head: charlie/t
created_at: 2024-01-30T21:05:35Z
updated_at: 2024-01-31T04:30:55Z
url: https://github.com/astral-sh/ruff/pull/9721
synced_at: 2026-01-10T22:57:09Z
```

# Recategorize `runtime-string-union` to `TCH010`

---

_Pull request opened by @charliermarsh on 2024-01-30 21:05_

## Summary

This rule was added to `flake8-type-checking` as `TC010`. We're about to stabilize it, so we might as well use the correct code.

See: https://github.com/astral-sh/ruff/issues/9573.


---

_Review requested from @zanieb by @charliermarsh on 2024-01-30 21:05_

---

_Marked ready for review by @charliermarsh on 2024-01-30 21:05_

---

_Merged by @charliermarsh on 2024-01-31 04:28_

---

_Closed by @charliermarsh on 2024-01-31 04:28_

---

_Branch deleted on 2024-01-31 04:28_

---

_Comment by @github-actions[bot] on 2024-01-31 04:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB131
	- FURB113
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'mccabe' -> 'lint.mccabe'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Selection of deprecated rule `TRY200` is not allowed when preview mode is enabled.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview mode is enabled.
```

</p>
</details>




---

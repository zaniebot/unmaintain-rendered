```yaml
number: 9759
title: Remove stale preview documentation from stabilized rule behaviors
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: release/0.2.0
head: charlie/preview-doc
created_at: 2024-02-01T18:41:20Z
updated_at: 2024-02-01T18:53:59Z
url: https://github.com/astral-sh/ruff/pull/9759
synced_at: 2026-01-12T15:55:30Z
```

# Remove stale preview documentation from stabilized rule behaviors

---

_@charliermarsh_

These behaviors were stabilized, so the docs referring to them as preview-only are incorrect.

---

_Label `documentation` added by @charliermarsh on 2024-02-01 18:43_

---

_Merged by @charliermarsh on 2024-02-01 18:47_

---

_Closed by @charliermarsh on 2024-02-01 18:47_

---

_Branch deleted on 2024-02-01 18:47_

---

_Comment by @github-actions[bot] on 2024-02-01 18:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

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
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

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




---

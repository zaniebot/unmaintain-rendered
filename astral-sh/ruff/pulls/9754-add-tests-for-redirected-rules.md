```yaml
number: 9754
title: Add tests for redirected rules
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: release/0.2.0
head: zb/deprecated-renamed
created_at: 2024-02-01T15:33:55Z
updated_at: 2024-02-01T16:46:38Z
url: https://github.com/astral-sh/ruff/pull/9754
synced_at: 2026-01-10T22:57:09Z
```

# Add tests for redirected rules

---

_Pull request opened by @zanieb on 2024-02-01 15:33_

Extends https://github.com/astral-sh/ruff/pull/9752 adding internal test rules for redirection

Fixes a bug where we did not see warnings for exact codes that are redirected (just prefixes)

---

_Renamed from "zb/deprecated renamed" to "Add tests for redirected rules" by @zanieb on 2024-02-01 15:40_

---

_Label `internal` added by @zanieb on 2024-02-01 15:41_

---

_Comment by @github-actions[bot] on 2024-02-01 15:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

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

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'mccabe' -> 'lint.mccabe'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Selection of deprecated rule `TRY200` is not allowed when preview is enabled.
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
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview is enabled.
```

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-01 16:09_

---

_@charliermarsh approved on 2024-02-01 16:22_

---

_Merged by @zanieb on 2024-02-01 16:46_

---

_Closed by @zanieb on 2024-02-01 16:46_

---

_Branch deleted on 2024-02-01 16:46_

---

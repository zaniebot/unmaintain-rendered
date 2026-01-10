```yaml
number: 13994
title: "Switch off the `single_match_else` Clippy lint"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ci
assignees: []
merged: true
base: main
head: clippy-lint
created_at: 2024-10-30T12:05:43Z
updated_at: 2024-10-30T12:24:40Z
url: https://github.com/astral-sh/ruff/pull/13994
synced_at: 2026-01-10T20:59:37Z
```

# Switch off the `single_match_else` Clippy lint

---

_Pull request opened by @AlexWaygood on 2024-10-30 12:05_

As discussed on Discord, this Clippy lint is pretty noisy and quite subjective. Sometimes it results in more concise/readable code, but sometimes you just want to use a `match` statement. Nobody seemed strongly in favour of keeping the lint.

---

_Review requested from @carljm by @AlexWaygood on 2024-10-30 12:05_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-30 12:05_

---

_@AlexWaygood reviewed on 2024-10-30 12:06_

---

_Review comment by @AlexWaygood on `Cargo.toml`:194 on 2024-10-30 12:06_

We don't have a `python.rs` file anymore, but it still seems like a bad idea to enable this lint while the `rustfmt` bug we encountered in #13250 is a thing. So I just updated the comment.

---

_Label `internal` added by @AlexWaygood on 2024-10-30 12:07_

---

_Label `ci` added by @AlexWaygood on 2024-10-30 12:07_

---

_Comment by @AlexWaygood on 2024-10-30 12:24_

From the conversation on Discord, I'm pretty sure we have consensus around this change

---

_Merged by @AlexWaygood on 2024-10-30 12:24_

---

_Closed by @AlexWaygood on 2024-10-30 12:24_

---

_Branch deleted on 2024-10-30 12:24_

---

_Comment by @github-actions[bot] on 2024-10-30 12:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>




---

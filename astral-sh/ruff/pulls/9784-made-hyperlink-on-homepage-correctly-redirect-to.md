```yaml
number: 9784
title: Made hyperlink on homepage correctly redirect to GitHub
type: pull_request
state: merged
author: trag1c
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-whos-using-ruff
created_at: 2024-02-02T11:44:33Z
updated_at: 2024-02-02T14:32:24Z
url: https://github.com/astral-sh/ruff/pull/9784
synced_at: 2026-01-10T22:57:09Z
```

# Made hyperlink on homepage correctly redirect to GitHub

---

_Pull request opened by @trag1c on 2024-02-02 11:44_

## Summary

Closes #9783. Feels hacky because of the different key so there *might* be a nicer way to do this 😄

## Test Plan
Tested locally with `mkdocs serve`.

---

_Comment by @github-actions[bot] on 2024-02-02 12:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_@charliermarsh approved on 2024-02-02 14:32_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-02-02 14:32_

---

_Comment by @charliermarsh on 2024-02-02 14:32_

It's hacky but the whole file / system is hacky.

---

_Merged by @charliermarsh on 2024-02-02 14:32_

---

_Closed by @charliermarsh on 2024-02-02 14:32_

---

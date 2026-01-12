```yaml
number: 9843
title: Bump version to v0.2.1
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2024-02-05T19:48:07Z
updated_at: 2024-02-05T22:19:28Z
url: https://github.com/astral-sh/ruff/pull/9843
synced_at: 2026-01-12T15:55:30Z
```

# Bump version to v0.2.1

---

_@charliermarsh_

_No description provided._

---

_Label `release` added by @charliermarsh on 2024-02-05 19:53_

---

_Marked ready for review by @charliermarsh on 2024-02-05 19:55_

---

_Review requested from @snowsignal by @charliermarsh on 2024-02-05 19:55_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-05 19:55_

---

_@MichaReiser approved on 2024-02-05 20:10_

---

_Comment by @github-actions[bot] on 2024-02-05 20:12_

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

_Merged by @charliermarsh on 2024-02-05 20:31_

---

_Closed by @charliermarsh on 2024-02-05 20:31_

---

_Branch deleted on 2024-02-05 20:31_

---

_Comment by @T-256 on 2024-02-05 22:19_

> ### Linter (stable)
> ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)
> 
> [sphinx-doc/sphinx](https://github.com/sphinx-doc/sphinx) (error)
> ```
> ruff failed
>   Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
> 	- FURB113
> 	- FURB131
> 	- FURB132
> ```


Perhaps a cli option to silently ignore this kind of error could help here. wdyt @zanieb ?

---

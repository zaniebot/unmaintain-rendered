```yaml
number: 9850
title: "ruff-ecosystem: Add indico/indico repo"
type: pull_request
state: merged
author: ThiefMaster
labels:
  - internal
assignees: []
merged: true
base: main
head: ecosystem-indico
created_at: 2024-02-06T00:24:00Z
updated_at: 2024-02-06T00:51:57Z
url: https://github.com/astral-sh/ruff/pull/9850
synced_at: 2026-01-12T15:55:30Z
```

# ruff-ecosystem: Add indico/indico repo

---

_@ThiefMaster_

It's a pretty big codebase using lots of different stuff, so a good candidate for finding obscure problems.

I didn't look more closely which options are used (I have the feeling `--select ALL` is not implied, since I see you adding it via `check_options` for certain entries but not for others), the repo itself has a pretty large ruff.toml - but assuming ecosystem just cares about differences between base and head of a PR, `ALL` most likely makes sense.

---

_Comment by @charliermarsh on 2024-02-06 00:32_

I'm gonna start it out without `ALL`, since it already has a bunch of rules enabled and it can actually be helpful _not_ to override the selectors when possible (since it can surface other kinds of bugs).

---

_Comment by @charliermarsh on 2024-02-06 00:32_

Thank you for this!

---

_Label `documentation` added by @charliermarsh on 2024-02-06 00:32_

---

_Label `documentation` removed by @charliermarsh on 2024-02-06 00:32_

---

_Label `internal` added by @charliermarsh on 2024-02-06 00:32_

---

_Merged by @charliermarsh on 2024-02-06 00:37_

---

_Closed by @charliermarsh on 2024-02-06 00:37_

---

_Comment by @github-actions[bot] on 2024-02-06 00:51_

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

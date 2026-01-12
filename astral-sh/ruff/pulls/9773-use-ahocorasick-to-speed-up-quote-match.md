```yaml
number: 9773
title: "Use `AhoCorasick` to speed up quote match"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/str
created_at: 2024-02-02T03:10:59Z
updated_at: 2024-02-02T14:57:40Z
url: https://github.com/astral-sh/ruff/pull/9773
synced_at: 2026-01-12T15:55:30Z
```

# Use `AhoCorasick` to speed up quote match

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When I was looking at the v0.2.0 release, this method showed up in a CodSpeed regression (we were calling it more), so I decided to quickly look at speeding it up. @BurntSushi suggested using Aho-Corasick, and it looks like it's about 7 or 8x faster:

```text
Parser/AhoCorasick      time:   [8.5646 ns 8.5914 ns 8.6191 ns]
Parser/Iterator         time:   [64.992 ns 65.124 ns 65.271 ns]
```

## Test Plan

`cargo test`


---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-02 03:11_

---

_Label `performance` added by @charliermarsh on 2024-02-02 03:11_

---

_@charliermarsh reviewed on 2024-02-02 03:11_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:183 on 2024-02-02 03:11_

This is also a bit faster than the iterator approach though both are quite fast (1ns vs. 800ps).

---

_Comment by @charliermarsh on 2024-02-02 03:14_

I bet we could speed this up even more but this feels like good bang for buck: https://lemire.me/blog/2023/07/14/recognizing-string-prefixes-with-simd-instructions/

---

_Comment by @github-actions[bot] on 2024-02-02 03:30_

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

_@MichaReiser approved on 2024-02-02 05:33_

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/str.rs`:162 on 2024-02-02 14:42_

Perfect. I'm glad you found `StartKind::Anchored`.

---

_@BurntSushi approved on 2024-02-02 14:43_

Sweet.

---

_Merged by @charliermarsh on 2024-02-02 14:57_

---

_Closed by @charliermarsh on 2024-02-02 14:57_

---

_Branch deleted on 2024-02-02 14:57_

---

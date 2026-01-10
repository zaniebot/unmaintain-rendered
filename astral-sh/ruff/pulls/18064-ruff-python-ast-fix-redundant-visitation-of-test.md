```yaml
number: 18064
title: "[ruff_python_ast] Fix redundant visitation of test expressions in elif clause statements"
type: pull_request
state: merged
author: swnb
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2025-05-13T06:58:57Z
updated_at: 2025-05-13T07:16:33Z
url: https://github.com/astral-sh/ruff/pull/18064
synced_at: 2026-01-10T18:51:01Z
```

# [ruff_python_ast] Fix redundant visitation of test expressions in elif clause statements

---

_Pull request opened by @swnb on 2025-05-13 06:58_

Fix redundant AST visitation of elif clause test expressions

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR addresses a bug in the AST traversal logic where the test expressions of elif clauses were being visited multiple times.

Removed the initial redundant visitation call to elif test expressions that originally occurred at
https://github.com/astral-sh/ruff/blob/0fb94c052e8e5e48743bf70931c4810bcc1fe53e/crates/ruff_python_ast/src/visitor/transformer.rs#L237
which duplicated the standard processing already implemented at 

https://github.com/astral-sh/ruff/blob/0fb94c052e8e5e48743bf70931c4810bcc1fe53e/crates/ruff_python_ast/src/visitor/transformer.rs#L110


## Test Plan

<!-- How was it tested? -->


---

_Label `internal` added by @MichaReiser on 2025-05-13 07:06_

---

_@MichaReiser approved on 2025-05-13 07:07_

---

_Merged by @MichaReiser on 2025-05-13 07:10_

---

_Closed by @MichaReiser on 2025-05-13 07:10_

---

_Comment by @github-actions[bot] on 2025-05-13 07:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
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
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
```

</p>
</details>




---

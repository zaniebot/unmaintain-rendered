```yaml
number: 10754
title: Rework docs for pydocstyle rules
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - docstring
assignees: []
merged: true
base: main
head: d406-docs
created_at: 2024-04-03T10:49:06Z
updated_at: 2024-04-03T21:34:03Z
url: https://github.com/astral-sh/ruff/pull/10754
synced_at: 2026-01-12T15:55:33Z
```

# Rework docs for pydocstyle rules

---

_@AlexWaygood_

## Summary

I was reading through ruff's pydocstyle rules as part of my work on rule recategorisation, and found the docs for the rules regarding docstring sections surprisingly hard to parse. Specifically, I found it hard to distinguish which rules were merely enforcing stylistic conventions, and which ones were guarding against incorrect reStructuredText syntax that would likely cause errors if you were using the docstrings to generate documentation with Sphinx. Additionally, many of the rules have docs that start with a paragraph that's a basic primer on how multiline docstrings work (which is essentially the same for all the rules), and only move on to information that's specific to the individual rule in later paragraphs. 

This PR has the following changes:
- Use a "newspaper style" of prose, where the topic is summarised first and explained in more depth in later paragraphs. For each rule, use the first sentence of the "Why is this bad?" section to give a clear description of why the antipattern is bad:
  - Is the rule just enforcing stylistic consistency, or is it also guarding against incorrect RST syntax?
  - Does it apply to all docstring conventions, or just one or two?
- Use "multiline" consistently everywhere, rather than "multi-line". https://learn.microsoft.com/en-us/style-guide/a-z-word-list-term-collections/m/multi
- Use slightly clearer and/or more concise language in several places.
- Give more complete explanations in a few places.
- Rework some examples to make the violations slightly more obvious.

## Test Plan

`pre-commit run -a`


---

_Label `documentation` added by @AlexWaygood on 2024-04-03 10:49_

---

_Label `docstring` added by @AlexWaygood on 2024-04-03 10:49_

---

_Comment by @github-actions[bot] on 2024-04-03 11:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>

```
Failed to clone mlflow/mlflow: error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
error: 242 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-03 20:15_

I only skimmed through the changes -- makes sense, trust you on this one!

---

_Merged by @AlexWaygood on 2024-04-03 21:34_

---

_Closed by @AlexWaygood on 2024-04-03 21:34_

---

_Branch deleted on 2024-04-03 21:34_

---

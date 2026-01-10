```yaml
number: 10182
title: Fix broken documentation links affected by namespace changes in lint rules
type: pull_request
state: merged
author: greenstar1151
labels:
  - documentation
assignees: []
merged: true
base: main
head: greenstar1151/fix-docs-broken-link-lint-namespace
created_at: 2024-03-01T10:27:13Z
updated_at: 2024-03-01T11:35:29Z
url: https://github.com/astral-sh/ruff/pull/10182
synced_at: 2026-01-10T22:47:01Z
```

# Fix broken documentation links affected by namespace changes in lint rules

---

_Pull request opened by @greenstar1151 on 2024-03-01 10:27_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

- This PR addresses broken links in the documentation that resulted from namespace changes to `lint`, introduced in version `0.2.0`.
- Short backstory:
  - While setting up `pydocstyle` rules for our Python codebase, I noticed that the [related section in the FAQ page](https://docs.astral.sh/ruff/faq/#does-ruff-support-numpy-or-google-style-docstrings) contained broken links. Although this was a minor issue, I wanted to prevent others (especially those unfamiliar with Ruff) from encountering the same inconvenience.

## Test Plan

<!-- How was it tested? -->

```
mkdocs serve -f mkdocs.public.yml
```

- Changed pages
  - RUF002, RUF003
    - [ruff/rules/ambiguous-unicode-character-docstring/](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-docstring/)
    - [ruff/rules/ambiguous-unicode-character-comment/](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-comment/)
    
    Fixed broken links to match [ruff/rules/ambiguous-unicode-character-string/](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-string/) (RUF001)
  - FAQ
    - [ruff/faq/#how-does-ruffs-import-sorting-compare-to-isort](https://docs.astral.sh/ruff/faq/#how-does-ruffs-import-sorting-compare-to-isort)
    - [ruff/faq/#does-ruff-support-numpy-or-google-style-docstrings](https://docs.astral.sh/ruff/faq/#does-ruff-support-numpy-or-google-style-docstrings)
  - The Ruff Linter
    - [ruff/linter/#fix-safety](https://docs.astral.sh/ruff/linter/#fix-safety)

---

_Comment by @github-actions[bot] on 2024-03-01 10:45_

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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:86 on 2024-03-01 11:35_

Oh I'm surprised that our doc generation script didn't warn about this one because it checks all options for validity. 

---

_@MichaReiser approved on 2024-03-01 11:35_

Thank you. I'm sorry that I missed those. Thanks for PRing a fix

---

_Label `documentation` added by @MichaReiser on 2024-03-01 11:35_

---

_Merged by @MichaReiser on 2024-03-01 11:35_

---

_Closed by @MichaReiser on 2024-03-01 11:35_

---

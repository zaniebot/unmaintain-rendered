```yaml
number: 12875
title: "Fallback to kernelspec to check if it's a Python notebook"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - notebook
assignees: []
merged: true
base: main
head: dhruv/kernel-spec
created_at: 2024-08-14T03:26:47Z
updated_at: 2024-08-14T07:06:12Z
url: https://github.com/astral-sh/ruff/pull/12875
synced_at: 2026-01-10T21:38:32Z
```

# Fallback to kernelspec to check if it's a Python notebook

---

_Pull request opened by @dhruvmanila on 2024-08-14 03:26_

## Summary

This PR adds a fallback logic for `is_python_notebook` to check the `kernelspec.language` field.

Reference implementation in VS Code: https://github.com/microsoft/vscode/blob/1c31e758985efe11bc0453a45ea0bb6887e670a4/extensions/ipynb/src/deserializers.ts#L20-L22

It's also required for the kernel to provide the `language` they're implementing based on https://jupyter-client.readthedocs.io/en/stable/kernels.html#kernel-specs reference although that's for the `kernel.json` file but is also included in the notebook metadata.

Closes: #12281

## Test Plan

Add a test case for `is_python_notebook` and include the test notebook for round trip validation.

The test notebook contains two cells, one is JavaScript (denoted via the `vscode.languageId` metadata) and the other is Python (no metadata). The notebook metadata only contains `kernelspec` and the `language_info` is absent.

I also verified that this is a valid notebook by opening it in Jupyter Lab, VS Code and using `nbformat` validator.


---

_Label `notebook` added by @dhruvmanila on 2024-08-14 03:26_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-14 03:27_

---

_@dhruvmanila reviewed on 2024-08-14 03:29_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:419 on 2024-08-14 03:29_

I'm not exactly sure why was the default value `true` in case `language_info` is absent but that doesn't seem correct to me. If we can't infer the language from either `language_info` or `kernelspec`, we'll now return `false` instead.

---

_Comment by @github-actions[bot] on 2024-08-14 03:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-08-14 05:01_

Need to look at what's going on with the notebooks in ecosystem output.

---

_@dhruvmanila reviewed on 2024-08-14 05:04_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:419 on 2024-08-14 05:04_

Ok, so the notebooks in the ecosystem diff doesn't have both `language_info` and `kernelspec` in which case we're not marking it as Python notebook. I think I'd need to revert this change to return `true` in this case.

---

_Review comment by @MichaReiser on `crates/ruff_notebook/src/notebook.rs`:422 on 2024-08-14 06:56_

I think defaulting to `true` is good for most notebooks. If this proves a problem, we can look at specific notebooks or decide to introduce an option.

---

_@MichaReiser approved on 2024-08-14 06:56_

Awesome. Thanks @dhruvmanila for pushing this changes before the release!

---

_Added to milestone `v0.6` by @MichaReiser on 2024-08-14 06:56_

---

_@dhruvmanila reviewed on 2024-08-14 07:05_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:422 on 2024-08-14 07:05_

Sounds good.

---

_Merged by @dhruvmanila on 2024-08-14 07:06_

---

_Closed by @dhruvmanila on 2024-08-14 07:06_

---

_Branch deleted on 2024-08-14 07:06_

---

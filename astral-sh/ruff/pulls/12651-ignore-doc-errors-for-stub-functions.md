```yaml
number: 12651
title: "Ignore `DOC` errors for stub functions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - docstring
assignees: []
merged: true
base: main
head: charlie/DOC
created_at: 2024-08-03T00:01:22Z
updated_at: 2024-08-03T12:17:57Z
url: https://github.com/astral-sh/ruff/pull/12651
synced_at: 2026-01-12T15:55:41Z
```

# Ignore `DOC` errors for stub functions

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12650.


---

_Renamed from "Ignore DOC errors for stub functions" to "Ignore `DOC` errors for stub functions" by @charliermarsh on 2024-08-03 00:01_

---

_Label `docstring` added by @charliermarsh on 2024-08-03 00:01_

---

_Comment by @github-actions[bot] on 2024-08-03 00:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -10 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L344'>src/bokeh/application/application.py:344:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/handler.py#L137'>src/bokeh/application/handlers/handler.py:137:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L75'>src/bokeh/application/handlers/lifecycle.py:75:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L124'>src/bokeh/colors/color.py:124:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L141'>src/bokeh/colors/color.py:141:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L175'>src/bokeh/colors/color.py:175:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L187'>src/bokeh/colors/color.py:187:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L199'>src/bokeh/colors/color.py:199:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/bases.py#L306'>src/bokeh/core/property/bases.py:306:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptor_factory.py#L124'>src/bokeh/core/property/descriptor_factory.py:124:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC202 | 10 | 0 | 10 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:653 on 2024-08-03 06:26_

Is this check now covered by is_stub?

---

_@MichaReiser approved on 2024-08-03 06:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:653 on 2024-08-03 11:45_

It's covered by the 

```rs
let Some(function_def) = definition.as_function_def() else {
    return;
};
```

immediately above 

---

_@AlexWaygood reviewed on 2024-08-03 11:45_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:678 on 2024-08-03 11:58_

Is this change just a stylistic preference?

---

_@AlexWaygood approved on 2024-08-03 11:58_

---

_@charliermarsh reviewed on 2024-08-03 12:13_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:678 on 2024-08-03 12:13_

Yeah, I prefer to have the enablement checks separated out from any lint-specific checks. I find it to be a clearer separation, and it would make it easier to identify and automatically rewrite them in the future.

---

_Merged by @charliermarsh on 2024-08-03 12:13_

---

_Closed by @charliermarsh on 2024-08-03 12:13_

---

_Branch deleted on 2024-08-03 12:13_

---

_@AlexWaygood reviewed on 2024-08-03 12:17_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:678 on 2024-08-03 12:17_

Makes sense, thanks 

---

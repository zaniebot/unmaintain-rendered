```yaml
number: 8708
title: "Use `indent-width` and `lint.tab-size` when checking indentation in `E` rules"
type: pull_request
state: closed
author: zanieb
labels:
  - help wanted
assignees: []
draft: true
base: main
head: logical-indent-size
created_at: 2023-11-15T23:18:20Z
updated_at: 2024-07-08T15:07:25Z
url: https://github.com/astral-sh/ruff/pull/8708
synced_at: 2026-01-10T21:47:02Z
```

# Use `indent-width` and `lint.tab-size` when checking indentation in `E` rules

---

_Pull request opened by @zanieb on 2023-11-15 23:18_

Closes https://github.com/astral-sh/ruff/issues/8705

Needs test cases still.
We should consider updating the formatter incompatibility documentation and checks.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/logical_lines.rs`:89 on 2023-11-15 23:22_

What happens if the user indents using tabs? What _should_ happen?

---

_@charliermarsh reviewed on 2023-11-15 23:23_

---

_Comment by @github-actions[bot] on 2023-11-15 23:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `help wanted` added by @zanieb on 2023-11-16 20:08_

---

_@NeilGirdhar reviewed on 2023-11-16 23:45_

---

_Review comment by @NeilGirdhar on `crates/ruff_linter/src/checkers/logical_lines.rs`:89 on 2023-11-16 23:45_

This should be `indent_width` instead of `tab_size`, right?

---

_@zanieb reviewed on 2023-11-17 03:15_

---

_Review comment by @zanieb on `crates/ruff_linter/src/checkers/logical_lines.rs`:89 on 2023-11-17 03:15_

`LinterSettings.tab_size` is inferred from `Settings.indent_width`.

---

_@NeilGirdhar reviewed on 2023-11-17 03:50_

---

_Review comment by @NeilGirdhar on `crates/ruff_linter/src/checkers/logical_lines.rs`:89 on 2023-11-17 03:50_

Ah okay, I thought that it was the tab size from the settings 😄 

---

_Comment by @MichaReiser on 2023-11-28 05:13_

@evanrittenhouse I believe you implemented the tab size option back then. Do you remember why the `E` rules didn't use the setting?

Sorry, I was wrong. Looking at the history, it must have been @JonathanPlasse 

---

_Comment by @evanrittenhouse on 2023-11-28 19:40_

Was going to say, I don't remember a tab size implementation 😆

---

_Comment by @JonathanPlasse on 2024-05-08 09:10_

> @evanrittenhouse I believe you implemented the tab size option back then. Do you remember why the `E` rules didn't use the setting?
> 
> Sorry, I was wrong. Looking at the history, it must have been @JonathanPlasse

I implemented it everywhere the line length comparison was used, E501, the one for max-doc-line-length, and rules that checks if the line length before applying fixes.
I did not touch logical lines code when implementing this.

---

_Comment by @charliermarsh on 2024-05-16 02:20_

@zanieb - should I see this through?

---

_Closed by @zanieb on 2024-07-08 15:07_

---

_Comment by @zanieb on 2024-07-08 15:07_

(I haven't had the time to dedicate to this. Anyone is welcome to take it up.)

---

```yaml
number: 19491
title: Skip notebook with errors in ecosystem check
type: pull_request
state: merged
author: ntBre
labels:
  - ci
assignees: []
merged: true
base: main
head: brent/fix-ecosystem-format
created_at: 2025-07-22T15:56:23Z
updated_at: 2025-07-22T16:29:40Z
url: https://github.com/astral-sh/ruff/pull/19491
synced_at: 2026-01-10T17:58:13Z
```

# Skip notebook with errors in ecosystem check

---

_Pull request opened by @ntBre on 2025-07-22 15:56_

Summary
--

I've been noticing this failure in the formatter ecosystem check and decided to
look into it. We fail to parse the [notebook](https://github.com/openai/openai-cookbook/blob/main/examples/mcp/databricks_mcp_cookbook.ipynb) because some of the `code` cells
have non-Python code in them. `ruff format` only reports one of these,
corresponding to a shell snippet, but `ruff check` emits some additional errors
about JS code later in the file too:

```
databricks_mcp_cookbook.ipynb:cell 21:1:11: SyntaxError: Simple statements must be separated by newlines or semicolons
databricks_mcp_cookbook.ipynb:cell 21:1:19: SyntaxError: Simple statements must be separated by newlines or semicolons
databricks_mcp_cookbook.ipynb:cell 21:1:50: SyntaxError: Simple statements must be separated by newlines or semicolons
databricks_mcp_cookbook.ipynb:cell 30:4:7: SyntaxError: Simple statements must be separated by newlines or semicolons
databricks_mcp_cookbook.ipynb:cell 30:4:41: E703 Statement ends with an unnecessary semicolon
databricks_mcp_cookbook.ipynb:cell 30:5:14: SyntaxError: Expected ':', found '{'
databricks_mcp_cookbook.ipynb:cell 30:6:9: SyntaxError: Expected ',', found '{'
databricks_mcp_cookbook.ipynb:cell 30:6:25: SyntaxError: Expected ',', found '='
databricks_mcp_cookbook.ipynb:cell 30:6:46: SyntaxError: Expected ',', found ';'
databricks_mcp_cookbook.ipynb:cell 30:6:47: SyntaxError: Expected '}', found newline
databricks_mcp_cookbook.ipynb:cell 30:7:1: SyntaxError: Unexpected indentation
databricks_mcp_cookbook.ipynb:cell 30:7:13: SyntaxError: Expected ':', found 'break'
databricks_mcp_cookbook.ipynb:cell 30:7:18: E703 Statement ends with an unnecessary semicolon
databricks_mcp_cookbook.ipynb:cell 30:8:28: SyntaxError: Simple statements must be separated by newlines or semicolons
databricks_mcp_cookbook.ipynb:cell 30:8:55: E703 Statement ends with an unnecessary semicolon
databricks_mcp_cookbook.ipynb:cell 30:9:18: SyntaxError: Expected an expression
databricks_mcp_cookbook.ipynb:cell 30:10:11: SyntaxError: Expected ',', found name
databricks_mcp_cookbook.ipynb:cell 30:10:16: SyntaxError: Expected ',', found '='
databricks_mcp_cookbook.ipynb:cell 30:10:22: SyntaxError: Expected ',', found name
databricks_mcp_cookbook.ipynb:cell 30:10:24: SyntaxError: Expected ',', found ';'
databricks_mcp_cookbook.ipynb:cell 30:11:27: SyntaxError: Expected ',', found '='
databricks_mcp_cookbook.ipynb:cell 30:11:34: SyntaxError: Expected ',', found name
databricks_mcp_cookbook.ipynb:cell 30:11:48: SyntaxError: Expected ',', found ';'
databricks_mcp_cookbook.ipynb:cell 30:11:49: SyntaxError: Expected '}', found NonLogicalNewline
databricks_mcp_cookbook.ipynb:cell 30:12:1: SyntaxError: Unexpected indentation
databricks_mcp_cookbook.ipynb:cell 30:12:16: E703 Statement ends with an unnecessary semicolon
databricks_mcp_cookbook.ipynb:cell 30:13:3: SyntaxError: Expected a statement
databricks_mcp_cookbook.ipynb:cell 30:13:4: SyntaxError: Expected a statement
databricks_mcp_cookbook.ipynb:cell 30:13:5: SyntaxError: Expected a statement
databricks_mcp_cookbook.ipynb:cell 30:13:5: E703 Statement ends with an unnecessary semicolon
databricks_mcp_cookbook.ipynb:cell 30:13:6: SyntaxError: Expected a statement
databricks_mcp_cookbook.ipynb:cell 30:14:1: SyntaxError: Expected a statement
databricks_mcp_cookbook.ipynb:cell 30:14:2: SyntaxError: Expected a statement
```

Test Plan
--

This PR


---

_Label `ci` added by @AlexWaygood on 2025-07-22 16:07_

---

_Comment by @github-actions[bot] on 2025-07-22 16:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @ntBre on 2025-07-22 16:10_

Looks like there are more notebooks with the same issues. I'll pull down the whole repo and try to find them all.

---

_@MichaReiser approved on 2025-07-22 16:13_

Thank you

---

_Comment by @ntBre on 2025-07-22 16:18_

It looks like the other notebooks were already in the `config_overrides` section, but I added them to `FormatOptions.exclude` too.

---

_Merged by @ntBre on 2025-07-22 16:29_

---

_Closed by @ntBre on 2025-07-22 16:29_

---

_Branch deleted on 2025-07-22 16:29_

---

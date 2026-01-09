---
number: 19250
title: Implement a robust docstring parser
type: issue
state: open
author: dhruvmanila
labels:
  - docstring
  - needs-design
assignees: []
created_at: 2025-07-10T06:12:11Z
updated_at: 2025-07-10T06:12:11Z
url: https://github.com/astral-sh/ruff/issues/19250
synced_at: 2026-01-07T13:12:16-06:00
---

# Implement a robust docstring parser

---

_Issue opened by @dhruvmanila on 2025-07-10 06:12_

Currently, we have two docstring parser which are specifically implemented for different purposes:
1. ty: https://github.com/astral-sh/ruff/blob/05d0daf50f52d6ea358ba5d11315eb7581851965/crates/ty_ide/src/docstring.rs
2. Ruff: https://github.com/astral-sh/ruff/tree/1ddda241f69d6e1aa651d279983a9738ddb2f892/crates/ruff_linter/src/docstrings

It will be useful to have a robust docstring parser that is error resilient and is able to parse multiple into multiple formats (Google, NumPy, Sphinx, etc.)

This would also make it easy to build rules on top of a well-defined docstring AST and would also resolve issues in the current implementation: https://github.com/astral-sh/ruff/issues?q=is%3Aissue%20state%3Aopen%20label%3Adocstring

This will first require a design proposal on what the AST would look like.

---

_Label `docstring` added by @dhruvmanila on 2025-07-10 06:12_

---

_Label `needs-design` added by @dhruvmanila on 2025-07-10 06:12_

---

_Referenced in [astral-sh/ruff#19194](../../astral-sh/ruff/pulls/19194.md) on 2025-07-10 06:12_

---

_Referenced in [astral-sh/ty#1608](../../astral-sh/ty/issues/1608.md) on 2025-11-21 16:36_

---

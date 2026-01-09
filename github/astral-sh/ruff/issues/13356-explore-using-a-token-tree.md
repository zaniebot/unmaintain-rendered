---
number: 13356
title: Explore using a token tree
type: issue
state: open
author: MichaReiser
labels:
  - parser
assignees: []
created_at: 2024-09-15T13:22:34Z
updated_at: 2024-09-15T13:22:34Z
url: https://github.com/astral-sh/ruff/issues/13356
synced_at: 2026-01-07T13:12:15-06:00
---

# Explore using a token tree

---

_Issue opened by @MichaReiser on 2024-09-15 13:22_

The ruff parser creates a flat-list of tokens. This is a very efficient representation but it has the downside that many lint rules implement the same logic to identify parenthesized ranges. 

An alternative to a flat token-list is to use a [token tree](https://fprijate.github.io/tlborm/mbe-syn-source-analysis.html#token-trees) where each parentheses-pair creates a sub-tree. This removes the need for lint rules to identify parenthesized ranges and allows the support error recovery similar to the parser when there are missing closing parentheses. 

---

_Label `parser` added by @MichaReiser on 2024-09-15 13:22_

---

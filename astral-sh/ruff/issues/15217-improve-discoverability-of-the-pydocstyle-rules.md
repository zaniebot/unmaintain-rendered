```yaml
number: 15217
title: "Improve discoverability of the `pydocstyle` rules as per the selected convention"
type: issue
state: open
author: dhruvmanila
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-01-02T05:29:34Z
updated_at: 2025-01-02T05:29:34Z
url: https://github.com/astral-sh/ruff/issues/15217
synced_at: 2026-01-12T15:54:54Z
```

# Improve discoverability of the `pydocstyle` rules as per the selected convention

---

_@dhruvmanila_

If a user has `D` in the rule selection, the rules that are actually selected depends on the docstring convention that's chosen. Currently, this information only lives at the bottom of this FAQ section: https://docs.astral.sh/ruff/faq/#does-ruff-support-numpy-or-google-style-docstrings. It would be useful to have this visible in an obvious place for which I see two options:
1. [`ruff.lint.pydocstyle.convention`](https://docs.astral.sh/ruff/settings/#lint_pydocstyle_convention) section
2. For each of the `Dxxx` rule page, specify how setting the `lint.pydocstyle.convention` may affect the selection of this rule

I think I'd prefer option (1) as the documentation is in a single place instead of distributed across multiple rule pages.

---

_Label `documentation` added by @dhruvmanila on 2025-01-02 05:29_

---

_Label `help wanted` added by @dhruvmanila on 2025-01-02 05:29_

---

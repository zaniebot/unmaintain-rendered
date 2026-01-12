```yaml
number: 9665
title: CPY001 with multiple authors
type: issue
state: open
author: jonyscathe
labels:
  - question
assignees: []
created_at: 2024-01-29T05:20:06Z
updated_at: 2024-01-29T07:26:44Z
url: https://github.com/astral-sh/ruff/issues/9665
synced_at: 2026-01-12T15:54:49Z
```

# CPY001 with multiple authors

---

_@jonyscathe_

No obvious way to have multiple copyright authors.  
It is quite plausible that one module would require a different copyright author in different files.
In `flake8-copyright` this is possible with the setting: `copyright-author = 'Author1|Author2'` as the copyright-author field is regex.
So that way if either Author1 or Author2 is in the file's copyright string there is no error.
Is there currently any way to get this working with ruff?

Currently using ruff==0.1.14

Ruff settings:
```
[tool.ruff]
extend-select = [
  'D213',
]
ignore = [
  'ANN101',
  'ANN401',
  'D107',
  'D203',
  'D212',
  'E203',
  'ISC003',
  'PLC1901',
]
line-length = 120
preview = true
select = ['ALL']
target-version = 'py312'

[tool.ruff.format]
line-ending = 'lf'
quote-style = 'single'

[tool.ruff.lint.flake8-copyright]
author = 'Author1|Author2'

[tool.ruff.lint.flake8-implicit-str-concat]
allow-multiline = false

[tool.ruff.lint.flake8-quotes]
inline-quotes = 'single'

[tool.ruff.lint.flake8-type-checking]
strict = true

[tool.ruff.lint.isort]
known-first-party = ['namespace.module_name']

[tool.ruff.lint.mccabe]
max-complexity = 21

[tool.ruff.lint.pylint]
max-args = 10
max-branches = 23
max-statements = 100
```

---

_Label `question` added by @MichaReiser on 2024-01-29 07:26_

---

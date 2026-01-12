```yaml
number: 9093
title: Disallow Implicit Str Concat
type: issue
state: closed
author: delulu52
labels:
  - needs-info
assignees: []
created_at: 2023-12-11T14:50:07Z
updated_at: 2023-12-11T19:17:57Z
url: https://github.com/astral-sh/ruff/issues/9093
synced_at: 2026-01-12T15:54:48Z
```

# Disallow Implicit Str Concat

---

_@delulu52_

So would desire to disallow implicit str concat (single line and multi line). I know via https://github.com/astral-sh/ruff/issues/8549 talks about how to remove multi line. But wondering

- How can do same for single line 
- When trying the above config, if I use common pre commit set up of (1) ruff check with auto fix (2) formatter. I get result of it being flagged initially but then formatter converting to single line, so then in subsequent runs it won't get flagged anymore (and actually just fixed to be single string)

---

_Comment by @dhruvmanila on 2023-12-11 14:57_

Hey, thanks for the issue. Can you please provide some context as I'm assuming it's a copy paste error on "When trying the above config" part? 😄

If possible, can you also provide a code snippet to test it against? Do you want to disallow implicit string concatenation when all the parts are on a single line (`"foo" "bar"`)?

---

_Label `waiting-on-author` added by @dhruvmanila on 2023-12-11 14:57_

---

_Comment by @delulu52 on 2023-12-11 17:00_

sorry for lack of context. example below should help

I have below pre-commit set up:

```yaml
repos:
- repo: https://github.com/astral-sh/ruff-pre-commit
  # ruff package version.
  rev: v0.1.7
  hooks:
    # Initial lint and auto-fix
    - id: ruff
      types_or: [ python, pyi, jupyter ]
      args: [ --fix ]
    # fromat python code
    - id: ruff-format
      types_or: [ python, pyi, jupyter ]
```

with following configuration in pyproject.toml (with rule = ISC selected)
```
[tool.ruff.lint.flake8-implicit-str-concat]
allow-multiline = false
```

with that if I have code below:
```python
x = [
    "hello"
    "there",
]
```

when pre-commit runs it will first have errors like
```
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

dd\__init__.py:19:1: B018 Found useless expression. Either assign it to a variable or remove it.
dd\__init__.py:21:1: B018 Found useless expression. Either assign it to a variable or remove it.
dd\__init__.py:24:5: ISC002 Implicitly concatenated string literals over multiple lines
Found 3 errors.

ruff-format..............................................................Failed
- hook id: ruff-format
- files were modified by this hook

warning: The following rules may cause conflicts when used with the formatter: `COM812`, `ISC001`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.  
1 file reformatted, 5 files left unchanged
```

this changes the formatting to below
```python
x = [
    "hello" "there",
]
```

so when i run the pre-commit again what happens is I get below, 
```
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1
- files were modified by this hook

dd\__init__.py:19:1: B018 Found useless expression. Either assign it to a variable or remove it.
dd\__init__.py:21:1: B018 Found useless expression. Either assign it to a variable or remove it.
Found 3 errors (1 fixed, 2 remaining).

ruff-format..............................................................Passed
```
new formatted code
```python
x = [
    "hellothere",
]

```

which effectually "hides" the implicit str concat error. 

so with the use of auto fix and formatter I am not quite seeing how possible to disallow implicit str concat (single and/or multi line)? 




---

_Comment by @charliermarsh on 2023-12-11 17:08_

I think your best bet here would be to mark `ISC001` as unfixable, to avoid the automatic concatenation that you're seeing:

```toml
[tool.ruff.linter]
extend-unfixable = ["ISC"]
```

---

_Comment by @charliermarsh on 2023-12-11 17:09_

(The behavior that you see, where `"hello" "there"` gets converted to `"hellothere"`, is actually happening in the linter via an autofix, not in the formatter.)

---

_Comment by @dhruvmanila on 2023-12-11 18:48_

For additional context, refer to the docs: https://docs.astral.sh/ruff/formatter/#format-suppression. Does this resolve your issue?

---

_Comment by @delulu52 on 2023-12-11 19:17_

@charliermarsh that works, thanks for help on this. 

---

_Closed by @delulu52 on 2023-12-11 19:17_

---

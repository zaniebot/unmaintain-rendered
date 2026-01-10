```yaml
number: 3749
title: Error messages when running Ruff on fixtures
type: issue
state: closed
author: JonathanPlasse
labels:
  - question
assignees: []
created_at: 2023-03-26T20:27:48Z
updated_at: 2023-03-26T20:34:10Z
url: https://github.com/astral-sh/ruff/issues/3749
synced_at: 2026-01-10T11:09:46Z
```

# Error messages when running Ruff on fixtures

---

_Issue opened by @JonathanPlasse on 2023-03-26 20:27_

When running `` I get these messages.
```console
❯ cargo run -p ruff_cli -- check --no-cache --statistics --select=ALL .
   Compiling ruff_cli v0.0.259 (/home/jonathan/Documents/github/ruff/crates/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 7.07s
     Running `target/debug/ruff check --no-cache --statistics --select=ALL .`
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
error: Failed to parse crates/ruff/resources/test/fixtures/pycodestyle/E101.py: Tabs not allowed as part of indentation after spaces at line 15 column 1
error: Failed to parse crates/ruff/resources/test/fixtures/pycodestyle/E999.py: unindent does not match any outer indentation level at line 3 column 0
error: Found non-ExprKind::Tuple argument to PEP 593 Annotation.
error: Failed to parse crates/ruff/resources/test/fixtures/pycodestyle/E11.py: expected an indented block at line 9 column 1
error: Failed to parse crates/ruff/resources/test/fixtures/pycodestyle/W19.py: invalid syntax. Got unexpected token "yes" at line 73 column 8
error: Failed to parse crates/ruff/resources/test/fixtures/pycodestyle/E20.py: invalid syntax. Got unexpected token 'x' at line 52 column 11
error: Failed to parse crates/ruff/resources/test/fixtures/pycodestyle/E27.py: expected an indented block at line 10 column 1
error: Failed to parse crates/ruff/resources/test/fixtures/flake8_commas/COM81.py: duplicate argument 'fine' in function definition at line 447 column 4
warning: Docstring at crates/ruff/resources/test/fixtures/pydocstyle/sections.py:510:5 contains implicit string concatenation; ignoring...
```
Is this normal?

---

_Comment by @charliermarsh on 2023-03-26 20:34_

Yup!

---

_Closed by @charliermarsh on 2023-03-26 20:34_

---

_Label `question` added by @charliermarsh on 2023-03-26 20:34_

---

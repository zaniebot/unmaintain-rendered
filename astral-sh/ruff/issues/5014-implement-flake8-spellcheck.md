---
number: 5014
title: Implement flake8-spellcheck 
type: issue
state: open
author: gaborbernat
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-06-11T15:05:01Z
updated_at: 2025-11-12T15:56:55Z
url: https://github.com/astral-sh/ruff/issues/5014
synced_at: 2026-01-10T01:22:44Z
---

# Implement flake8-spellcheck 

---

_Issue opened by @gaborbernat on 2023-06-11 15:05_

👍 or another spellcheck variant 😊 

---

_Comment by @calumy on 2023-06-11 18:39_

Similar to #1628

---

_Label `plugin` added by @charliermarsh on 2023-06-12 15:20_

---

_Comment by @charliermarsh on 2023-06-12 15:21_

I think my opinion here is mostly the same as it was before: I feel like typo correction is better handled by multi-language or language-agnostic tooling, like [`typos`](https://crates.io/crates/typos), which works on Python but other sources too.

---

_Comment by @gaborbernat on 2023-06-12 15:34_

I think that partially makes sense. However, they are some language specific constructs here, for example using snake casing to tokenize the source code, such as variable names or function names. Furthermore, it'll probably is a python specific extension/ allow list of words/ concepts. Finally would be nice if you could set up these tool from pypi like all other linting tools.

---

_Comment by @charliermarsh on 2023-06-12 15:36_

Yeah, I think the argument I agree with most here is that it'd be nice to have the spellcheck unified with the rest of the Ruff linting stack (i.e., not a separate tool).

FWIW, `typos` does handle casing as you would expect since it is designed to lint "source code":

```
❯ typos foo.py
error: `helo` should be `hello`
  --> foo.py:1:1
  |
1 | helo_goodbye = 1
  | ^^^^
  |
error: `helo` should be `hello`
  --> foo.py:2:1
  |
2 | heloGoodbye = 1
  | ^^^^
  |
```

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @asears on 2023-10-15 23:25_

This would be helpful for those currently leveraging flake8-spellcheck, the last plugin before moving to ruff completely.  flake8-spellcheck includes python & django dictionaries.  It hasn't been updated for about a year and a half.  It would be great to have a python-specific spell checker with the speed of ruff.

https://github.com/MichaelAquilina/flake8-spellcheck/blob/main/flake8_spellcheck/__init__.py

---

_Comment by @ashrub-holvi on 2024-08-16 13:54_

I had **no** experience with `typos` or `codespell` yet, but feels like checking source code just like text files may produce a lot of false errors.
So, perhaps good to have an option to check comments and docstrings only, because on code review we reading code much more careful than non-code parts.

---

_Referenced in [pyvista/pyvista#7110](../../pyvista/pyvista/pulls/7110.md) on 2025-01-23 07:10_

---

_Comment by @ashrub-holvi on 2025-11-12 14:40_

> good to have an option to check comments and docstrings

also identifiers, but I doubt it make sense to check values in literals, typos does that and it produces false-positives

---

_Comment by @ashrub-holvi on 2025-11-12 15:56_

I played with different tools, but for introduce them to existing codebase it's too much work for fighting with false-positives and I gave up. My guess that if `ruff` would take only identifiers and exclude identifiers which comes from third-party libs and for every identifier call `typos` API it will be useful and will not create too many false-positives.

---

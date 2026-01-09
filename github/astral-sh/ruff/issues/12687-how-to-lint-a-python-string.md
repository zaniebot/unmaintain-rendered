---
number: 12687
title: How to lint a Python string?
type: issue
state: closed
author: shner-elmo
labels:
  - question
assignees: []
created_at: 2024-08-05T11:44:42Z
updated_at: 2024-09-17T03:01:23Z
url: https://github.com/astral-sh/ruff/issues/12687
synced_at: 2026-01-07T13:12:15-06:00
---

# How to lint a Python string?

---

_Issue opened by @shner-elmo on 2024-08-05 11:44_

Hello, I'm working on a script that generate Python code and returns it as a string (doesn't write to disk), and I was wondering whats the best way I can lint the string with `ruff`?

for example:
```py
from ruff import lint_code

code = "def f(x): x*x"
assert lint_code(code) == """def f(x):\n    x * x"""
```

---

_Comment by @MichaReiser on 2024-08-05 12:43_

Ruff doesn't provide a Python API today. https://github.com/astral-sh/ruff/issues/659 tracks the feature request for a Pyo3 API. 

Your options are:

* Use the `CLI` and pass the code via stdin `ruff check --stdin-filename name.py -`
* Use the wasm API
* Contribute a Pyo3 ApI

---

_Label `question` added by @MichaReiser on 2024-08-05 12:43_

---

_Closed by @MichaReiser on 2024-09-09 12:02_

---

_Comment by @shner-elmo on 2024-09-17 03:01_

FYI this is how I ended up doing it:

https://gist.github.com/shner-elmo/b2639a4d1e04ceafaad120acfb31213c

---

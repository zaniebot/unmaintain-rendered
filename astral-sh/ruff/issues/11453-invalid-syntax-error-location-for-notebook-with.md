```yaml
number: 11453
title: "Invalid syntax error location for Notebook with `ruff format`"
type: issue
state: closed
author: david-waterworth
labels:
  - bug
  - notebook
assignees: []
created_at: 2024-05-16T23:52:17Z
updated_at: 2025-03-04T17:00:32Z
url: https://github.com/astral-sh/ruff/issues/11453
synced_at: 2026-01-10T11:09:53Z
```

# Invalid syntax error location for Notebook with `ruff format`

---

_Issue opened by @david-waterworth on 2024-05-16 23:52_

I'm getting the error `error: Failed to parse notebooks/tickets v2.ipynb:1:1:5: Invalid decimal integer literal` when I run `ruff format .` on a project (previously I was using whatever the default vscode formatter is). 

I assume the :1:1:5 is a reference to a cell but I cannot parse it, does that mean cell 1, line 1 column 5? And is it 0 or 1 based - either way the message doesn't help as the first cell contains

```
%load_ext autoreload
%autoreload 2
```
and the second
```
from datetime import datetime, timezone
...
```

Also `pre-commit` doesn't seem to complain, not does `format on save` (vscode setting) which I've configured to use Ruff as the default formatter, it's only when I try and format from the cmd line. I tried escaping the space with `\ ` and surrounding the filename with " " 

I'm using ruff 0.4.4 (global pipx installation) and my settings are:

```
[tool.ruff]
extend-include = ["*.ipynb"]

# Same as Black.
line-length = 120
lint.ignore-init-module-imports = true

exclude = []
lint.select = [
    "E",  # pycodestyle errors (settings from FastAPI, thanks, @tiangolo!)
    "W",  # pycodestyle warnings
    "F",  # pyflakes
    "I",  # isort
    "C",  # flake8-comprehensions
    "B",  # flake8-bugbear
]
lint.ignore = [
    "E501",  # line too long, handled by black
    "C901",  # too complex
]
lint.per-file-ignores = { "*.ipynb" = ["E402", "F704", "B018"] }

[tool.ruff.lint.isort]
order-by-type = true
relative-imports-order = "closest-to-furthest"
extra-standard-library = ["typing"]
section-order = ["future", "standard-library", "third-party", "first-party", "local-folder"]
known-first-party = []

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"
docstring-code-format = true
docstring-code-line-length = "dynamic"
```


---

_Comment by @charliermarsh on 2024-05-17 03:19_

Are you able to share the raw notebook file?

---

_Label `needs-info` added by @charliermarsh on 2024-05-17 03:19_

---

_Comment by @david-waterworth on 2024-05-17 04:17_

I figured it out, right down the bottom there was as cell I missed containing 

```
559/02408930-81c7-47d7-b2b0-ee38cb3f6e62
```

Which is clearly invalid - it's actually a fragment of a uri I added at some stage to test something and I should have set the cell to markdown or commented it out. The linter was flagging as an error, I guess the only real issue is the message is a bit cryptic, ideally `ruff format` would report the cell number at least, and maybe that this is a linter error - the image below shows what ruff reports in the "Problems" panel of vscode but `ruff format` is leaving out some context (also the message below doesn't include the cell number)

![image](https://github.com/astral-sh/ruff/assets/5028974/0c8f53c8-9e84-43bb-9611-e7b8dfc8df87)




---

_Comment by @dhruvmanila on 2024-05-17 06:31_

Thanks for the details. So, yeah the `ruff format` command does not include the cell details while the `ruff check` command does:

```
error: Failed to parse notebooks/syntax_error.ipynb:4:1:5: Invalid decimal integer literal
```

I can look into it.

---

_Label `needs-info` removed by @dhruvmanila on 2024-05-17 06:31_

---

_Label `notebook` added by @dhruvmanila on 2024-05-17 06:31_

---

_Label `cli` added by @dhruvmanila on 2024-05-17 06:31_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-05-17 06:31_

---

_Renamed from "error: Failed to parse notebook Invalid decimal integer literal" to "Invalid syntax error location for Notebook with `ruff format`" by @dhruvmanila on 2024-05-17 06:34_

---

_Label `cli` removed by @dhruvmanila on 2024-05-17 06:39_

---

_Label `bug` added by @dhruvmanila on 2024-05-17 06:39_

---

_Comment by @david-waterworth on 2024-05-17 06:51_

Note the `ruff check` command isn't really including the cell details either - see below, it has line and column number but not cell number. It works in the vscode "Problems" panel because it's linked, i.e. I can click on the message and it'll jump to the correct cell but the message itself would ideally also contain the cell number (i.e. the first message should have suffix [Cell 24, Line 2, Col 28] if that's possible

![image](https://github.com/astral-sh/ruff/assets/5028974/48a61b05-c979-49ee-893c-c5bdca8b6c61)


---

_Comment by @dhruvmanila on 2024-05-17 07:16_

I meant that the output in the CLI does provide the correct cell number: 
```console
$ ruff check ~/playground/ruff/notebooks/syntax_error.ipynb 
error: Failed to parse /Users/dhruv/playground/ruff/notebooks/syntax_error.ipynb:4:1:5: Invalid decimal integer literal
/Users/dhruv/playground/ruff/notebooks/syntax_error.ipynb:cell 4:1:5: E999 SyntaxError: Invalid decimal integer literal
```

Regarding the VS Code problems tab, I don't know if that's even possible. I'll need to look into it.

---

_Comment by @david-waterworth on 2024-05-17 07:47_

Ah right yeah, I don't think it's an issue in the vs code tab, as I mentioned you can click to jump to the problem anyway

---

_Closed by @MichaReiser on 2025-03-04 17:00_

---

```yaml
number: 795
title: "--fix for D202 removing docstring instead of line afterwards"
type: issue
state: closed
author: actionless
labels:
  - bug
assignees: []
created_at: 2022-11-17T19:25:45Z
updated_at: 2022-11-17T20:32:40Z
url: https://github.com/astral-sh/ruff/issues/795
synced_at: 2026-01-12T15:54:40Z
```

# --fix for D202 removing docstring instead of line afterwards

---

_@actionless_

```python
def function() -> None:
    '''
    Function displays a question and reads a single character
    from STDIN as an answer. Then returns the character as lower character.
    Valid answers are passed as 'answers' variable (the default is in capital).
    Invalid answer will return an empty string.
    '''

    pass
```

```console
ruff ruff_docstring_D300.py --fix
```

```python
def function() -> None:
    '''
    '''

    pass
```

---

_Label `bug` added by @charliermarsh on 2022-11-17 19:31_

---

_Comment by @charliermarsh on 2022-11-17 19:45_

I don't think D300 is fixable right now so wondering what's going on here. Can you include your `pyproject.toml`?

---

_Comment by @actionless on 2022-11-17 19:53_

```toml
[tool.ruff]
line-length = 100
# Add "Q" to the list of enabled codes.
select = ["E", "F", "Q", "I", "D", "U", "N", "S", "C", "B", "A", "T", "ANN", "YTT", "RUF", "M"]
ignore = [
	# enable back later:
	"Q000",
	"D100",
	"D101",
	"D102",
	"D103",

	"D105",
	"D107",
	"D203", # conflicts with D211
	"D205",
	"D212", # conflicts with D213
	"D400",
	"D415",

	"ANN002",
	"ANN003",
	"ANN101",
	"ANN204",

	"T201",
	"T203",
]
```

---

_Comment by @charliermarsh on 2022-11-17 19:55_

When I run `cargo run foo.py --fix` on that file, I get:

```py
def function() -> None:
    '''
    from STDIN as an answer. Then returns the character as lower character.
    Valid answers are passed as 'answers' variable (the default is in capital).
    Invalid answer will return an empty string.
    '''

    pass
```

It deletes the first line, in trying to fix D202. So also a bug, but a little different than your output?

---

_Renamed from "--fix for D300 removing docstring instead of replacing quotes" to "--fix for D202 removing docstring instead of line afterwards" by @actionless on 2022-11-17 19:57_

---

_Comment by @actionless on 2022-11-17 19:57_

@charliermarsh thanks for clarifying, i've updated the description

---

_Comment by @charliermarsh on 2022-11-17 19:58_

Thanks! The tests didn't catch this because we only had single-line docstrings... and so the start and end line were the same 🤦 Fix will go out shortly.

---

_Closed by @charliermarsh on 2022-11-17 20:02_

---

_Comment by @actionless on 2022-11-17 20:10_

thanks! i'm building git version of ruff to check it 👍

---

_Comment by @actionless on 2022-11-17 20:32_

it works now 🔥

---

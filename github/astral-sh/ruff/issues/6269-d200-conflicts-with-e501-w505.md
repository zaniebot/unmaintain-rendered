---
number: 6269
title: "`D200` conflicts with `E501`/`W505`."
type: issue
state: open
author: ngnpope
labels:
  - docstring
  - needs-decision
assignees: []
created_at: 2023-08-02T09:35:20Z
updated_at: 2025-06-25T02:17:33Z
url: https://github.com/astral-sh/ruff/issues/6269
synced_at: 2026-01-07T13:12:15-06:00
---

# `D200` conflicts with `E501`/`W505`.

---

_Issue opened by @ngnpope on 2023-08-02 09:35_

I have an issue where `D200` is raised, but after fixing it I get `E501`/`W505`.  `D200` should take into account the quote style, e.g. single or triple quotes, and prefixes, e.g. `r` for raw strings, and calculate the expected length appropriately, not raising if the fixed version would exceed `max-doc-length` or `max-length`.

Using the following configuration file -- `bug.toml`:

```toml
select = ["D200", "E501", "W505"]
line-length = 88

[pycodestyle]
max-doc-length = 88
```

And the following code -- `bug.py`:

```python
# Expect nothing for the following docstrings:

def fslsq87():
    "This is a single line docstring with single quotes padded to 87 characters. XXXXX"

def fslsq88():
    "This is a single line docstring with single quotes padded to 88 characters. XXXXXX"

def fsltq87():
    """This is a single line docstring with triple quotes padded to 87 characters. X"""

def fsltq88():
    """This is a single line docstring with triple quotes padded to 88 characters. XX"""

def fmltq83():
    """
    This is a multi line docstring with triple quotes padded to 83 characters. XXXX
    """

def fmltq87():
    """
    This is a multi line docstring with triple quotes padded to 87 characters. XXXXXXXX
    """

def fmltq88():
    """
    This is a multi line docstring with triple quotes padded to 88 characters. XXXXXXXXX
    """

def frslsq87():
    r"This is a raw single line docstring with single quotes padded to 87 characters. "

def frslsq88():
    r"This is a raw single line docstring with single quotes padded to 88 characters. X"

def frsltq87():
    r"""This is a raw single line docstring with triple quotes padded to 87 chara..."""

def frsltq88():
    r"""This is a raw single line docstring with triple quotes padded to 88 charac..."""

def frmltq82():
    r"""
    This is a raw multi line docstring with triple quotes padded to 82 characters.
    """

def frmltq87():
    r"""
    This is a raw multi line docstring with triple quotes padded to 87 characters. XXXX
    """

def frmltq88():
    r"""
    This is a raw multi line docstring with triple quotes padded to 88 characters. XXXXX
    """

# Expect D200 for the following docstrings:

def fmltq82():
    """
    This is a multi line docstring with triple quotes padded to 82 characters. XXX
    """

def frmltq81():
    r"""
    This is a raw multi line docstring with triple quotes padded to 81 characters
    """

# Expect E501/W505 for the following docstrings:

def fslsq89():
    "This is a single line docstring with single quotes padded to 89 characters. XXXXXXX"

def fsltq89():
    """This is a single line docstring with triple quotes padded to 89 characters. XXX"""

def fmltq89():
    """
    This is a multi line docstring with triple quotes padded to 89 characters. XXXXXXXXXX
    """

def frslsq89():
    r"This is a raw single line docstring with single quotes padded to 89 characters. XX"

def frsltq89():
    r"""This is a raw single line docstring with triple quotes padded to 89 characters"""

def frmltq89():
    r"""
    This is a raw multi line docstring with triple quotes padded to 89 characters. XXXXXX
    """
```

I get the following output:

```console
$ ruff --version
ruff 0.0.282

$ ruff check --config=bug.toml bug.py
bug.py:16:5: D200 [*] One-line docstring should fit on one line
bug.py:21:5: D200 [*] One-line docstring should fit on one line
bug.py:26:5: D200 [*] One-line docstring should fit on one line
bug.py:43:5: D200 [*] One-line docstring should fit on one line
bug.py:48:5: D200 [*] One-line docstring should fit on one line
bug.py:53:5: D200 [*] One-line docstring should fit on one line
bug.py:60:5: D200 [*] One-line docstring should fit on one line
bug.py:65:5: D200 [*] One-line docstring should fit on one line
bug.py:72:89: W505 Doc line too long (89 > 88 characters)
bug.py:72:89: E501 Line too long (89 > 88 characters)
bug.py:75:89: W505 Doc line too long (89 > 88 characters)
bug.py:75:89: E501 Line too long (89 > 88 characters)
bug.py:78:5: D200 [*] One-line docstring should fit on one line
bug.py:79:89: W505 Doc line too long (89 > 88 characters)
bug.py:79:89: E501 Line too long (89 > 88 characters)
bug.py:83:89: W505 Doc line too long (89 > 88 characters)
bug.py:83:89: E501 Line too long (89 > 88 characters)
bug.py:86:89: W505 Doc line too long (89 > 88 characters)
bug.py:86:89: E501 Line too long (89 > 88 characters)
bug.py:89:5: D200 [*] One-line docstring should fit on one line
bug.py:90:89: W505 Doc line too long (89 > 88 characters)
bug.py:90:89: E501 Line too long (89 > 88 characters)
Found 22 errors.
[*] 10 potentially fixable with the --fix option.
```

I would expect the following output:

```console
$ ruff check --config=bug.toml bug.py
bug.py:60:5: D200 [*] One-line docstring should fit on one line
bug.py:65:5: D200 [*] One-line docstring should fit on one line
bug.py:72:89: W505 Doc line too long (89 > 88 characters)
bug.py:72:89: E501 Line too long (89 > 88 characters)
bug.py:75:89: W505 Doc line too long (89 > 88 characters)
bug.py:75:89: E501 Line too long (89 > 88 characters)
bug.py:79:89: W505 Doc line too long (89 > 88 characters)
bug.py:79:89: E501 Line too long (89 > 88 characters)
bug.py:83:89: W505 Doc line too long (89 > 88 characters)
bug.py:83:89: E501 Line too long (89 > 88 characters)
bug.py:86:89: W505 Doc line too long (89 > 88 characters)
bug.py:86:89: E501 Line too long (89 > 88 characters)
bug.py:90:89: W505 Doc line too long (89 > 88 characters)
bug.py:90:89: E501 Line too long (89 > 88 characters)
Found 14 errors.
[*] 2 potentially fixable with the --fix option.
```

_Note that the same ought to apply at module level too (but where indentation is zero), but as `D200` only applies to the first string in the module, I'd have had to create lots of tiny files..._

Hopefully this above example will provide a good comprehensive test case 😉

---

_Label `needs-decision` added by @charliermarsh on 2023-08-16 04:06_

---

_Label `docstring` added by @charliermarsh on 2023-08-16 04:06_

---

_Referenced in [astral-sh/ruff#7414](../../astral-sh/ruff/issues/7414.md) on 2023-09-15 18:21_

---

_Referenced in [astral-sh/ruff#10272](../../astral-sh/ruff/issues/10272.md) on 2024-03-07 09:33_

---

_Comment by @Kaelten on 2025-06-25 02:17_

Been hitting this too, I also can't seem to put in a ignore for the line too long bit either, but will try more of that soon. 

---

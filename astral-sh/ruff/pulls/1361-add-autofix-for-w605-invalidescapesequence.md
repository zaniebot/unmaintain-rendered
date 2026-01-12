```yaml
number: 1361
title: "Add autofix for W605 [InvalidEscapeSequence]"
type: pull_request
state: merged
author: Sawbez
labels: []
assignees: []
merged: true
base: main
head: master
created_at: 2022-12-24T18:13:40Z
updated_at: 2023-03-10T18:02:13Z
url: https://github.com/astral-sh/ruff/pull/1361
synced_at: 2026-01-12T15:55:06Z
```

# Add autofix for W605 [InvalidEscapeSequence]

---

_@Sawbez_

I added the autofix, updated the README, and updated the insta tests.

---

_Merged by @charliermarsh on 2022-12-24 18:46_

---

_Closed by @charliermarsh on 2022-12-24 18:46_

---

_Branch deleted on 2022-12-24 19:00_

---

_Comment by @cclauss on 2023-03-10 10:02_

This solution does not seem pythonic to me.  I believe the use of `r"raw strings"` would be more intuitive and safer.
* https://docs.python.org/3/library/re.html
> The solution is to use Python’s raw string notation for regular expression patterns; backslashes are not handled in any special way in a string literal prefixed with 'r'. So r"\n" is a two-character string containing '\' and 'n', while "\n" is a one-character string containing a newline. Usually, patterns will be expressed in Python code using this raw string notation.
* https://www.flake8rules.com/rules/W605.html
* https://bugs.python.org/issue27364

There is also a Windows path corner case when the **fixer should not run** or the fixer should add `noqa: W605` because `\\` is not helpful:
```
"""
Windows files of interest:
* C:\documents\README.md
* D:\readme\README.txt
* Z:\net_disk\README.rst
"""  # noqa: W605
```

---

_Comment by @andersk on 2023-03-10 17:24_

@cclauss Docstrings are not exempt from the normal Python syntax rules about string literals. If you want to include a Windows path in a docstring, you still need to escape the backslashes ( `"""… C:\\documents\\README.md …"""`) or write the docstring as raw (`r"""… C:\documents\README.txt …"""`). Otherwise not just Ruff but also Python itself will warn (if you have warnings enabled), and this may become a syntax error in the future. Also pydoc and other documentation generators will interpret the valid `\r` and `\n` escape sequences in your docstring as written and render something like

```
        Windows files of interest:
        * C:\documents\README.md
        * D: eadme\README.txt
        * Z:
    et_disk\README.rst
```

So ignoring W605 is not a good plan.

---

_Comment by @cclauss on 2023-03-10 17:28_

The leading `r` on a docstring is also acceptable to Python and friends.
```
r"""
Windows files of interest:
* C:\documents\README.md
* D:\readme\README.txt
* Z:\net_disk\README.rst
"""
```
I have no problem with `W605` being raised.  That logic is correct.
My problem is with the `--fix` which does not use `r"raw strings"`.

---

_Comment by @andersk on 2023-03-10 17:35_

It’s acceptable to Ruff too. Just because the autofixer wants to fix it one way doesn’t mean you’re not allowed to fix it a different way!

The main reason the autofixer has to go with `\\` is that it can’t read your mind and know that you intend to remove the effects of the valid escape sequences that might also be present, such as your `\r` and `\n`. It aims to never change the meaning of your code.

---

_Comment by @cclauss on 2023-03-10 17:38_

Please reread the first few paragraphs of https://docs.python.org/3/library/re.html

If my string is being flagged for having an invalid escape sequence then the fixer does not need to read my mind.  It just needs to suggest that I do what the Python docs recommend in that situation.  Those docs explicitly say that substituting ever more backslashes only confuses the reader.

---

_Comment by @andersk on 2023-03-10 17:48_

What I’m saying is that if the autofixer were to change `"""C:\documents, D:\readme"""` to `r"""C:\documents, D:\readme"""` as you suggest, that would be a parsed as a *different string*. It would change the meaning of your code. It might happen to be a good change in this case, but Ruff can’t know that, and either way it doesn’t want to automatically change the meaning of your code.

```pycon
❯ python3 -Wd
Python 3.10.10 (main, Feb  7 2023, 12:19:31) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> s = """C:\documents, D:\readme"""
<stdin>:1: DeprecationWarning: invalid escape sequence '\d'
>>> s == """C:\\documents, D:\readme"""
True
>>> s == r"""C:\documents, D:\readme"""
False
```

---

_Comment by @cclauss on 2023-03-10 18:02_

Nice example.  I understand now.  Thanks.

---

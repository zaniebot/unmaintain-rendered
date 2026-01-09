---
number: 10396
title: "problematic formatting: indentation change in docstring"
type: issue
state: closed
author: kaddkaka
labels:
  - docstring
  - formatter
assignees: []
created_at: 2024-03-13T21:24:28Z
updated_at: 2024-03-19T17:44:23Z
url: https://github.com/astral-sh/ruff/issues/10396
synced_at: 2026-01-07T13:12:15-06:00
---

# problematic formatting: indentation change in docstring

---

_Issue opened by @kaddkaka on 2024-03-13 21:24_

black and ruff format might destroy ascii art in dosctring when re-indenting docstring lines. (so I'm not sure if this issue should be filed here or at the black project)

Example code:
```py
def dosctring_ascii_alignment():
    """Ascii art: [____,aaaa,bbbb]
                   ^         ^
    """
    pass
```

result after `ruff format` (with version `0.3.2`):
```py
def dosctring_ascii_alignment():
    """Ascii art: [____,aaaa,bbbb]
    ^         ^
    """
    pass
```

as seen, the carets `^` are now misaligned.

In my mind it would be better to not touch docstrings - but I guess there might be some reason for it?

---

_Renamed from "problematic forwarding: indentation change in docstring" to "problematic formatting: indentation change in docstring" by @kaddkaka on 2024-03-13 21:24_

---

_Comment by @ilius on 2024-03-13 23:07_

I would totally agree.
Docstrings are just strings that happen to be the first expression in the function / class.
Formatter would not normally modify strings.
As the Zen of Python says: "Special cases aren't special enough to break the rules."

What benefit does it have to justify it?

---

_Comment by @charliermarsh on 2024-03-14 15:51_

Ah yeah, I'd suggest just disabling formatting for the docstring in this case.

On a whole, it is a net positive (by far, in my opinion) for the formatter to correct docstring indentation than to disable it entirely due to special cases like (e.g.) ASCII art. You are of course welcome to file an issue with Black if you'd like.


---

_Label `docstring` added by @charliermarsh on 2024-03-14 15:51_

---

_Label `formatter` added by @charliermarsh on 2024-03-14 15:51_

---

_Closed by @charliermarsh on 2024-03-14 15:51_

---

_Comment by @ilius on 2024-03-14 22:04_

Oh I didn't know there was am option to disable it thanks!


    docstring-code-format = false


https://github.com/astral-sh/ruff/blob/main/docs/formatter.md
https://github.com/astral-sh/ruff/blob/main/docs/configuration.md

BTW: there are a few parameters missing in `docs/formatter.md`.
Should I add them as PR?

---

_Comment by @zanieb on 2024-03-14 23:27_

To avoid confusion.. `docstring-code-format` is for code blocks in docstrings, not the docstrings themselves. You're probably looking to just use `# fmt: skip`

```
def foo():
    """
        test
    """  # fmt: skip
```

I'm not sure it's worth enumerating all configuration options in the formatter overview but feel free to pull request anything you think would be helpful and we can discuss it there.

---

_Comment by @kaddkaka on 2024-03-15 05:37_

`#fmt` is not really an option since I would have to look over thousands of docstring to try and find where to add that waiver.

Breaking information like this is not good and cane in the worst case be very confusing/misleading.

Ruff already has some options that black does not, but I might open a PR on black side then. 

---

_Referenced in [psf/black#4276](../../psf/black/issues/4276.md) on 2024-03-15 05:44_

---

_Comment by @kaddkaka on 2024-03-19 17:30_

The ruff source code seems to try and keep indentation alignment in some cases, for example this code is formatted according to ruff:
```py
def dosctring_ascii_alignment():
    """Ascii art: [____,aaaa,bbbb]
    ^         ^
    """
    pass


def dosctring_ascii_alignment():
    """
    Ascii art: [____,aaaa,bbbb]
                     ^^^^
    """
```
As seen in the 2nd function, the docstring is *not* modified. 

---

_Referenced in [astral-sh/ruff#10550](../../astral-sh/ruff/issues/10550.md) on 2024-03-25 00:54_

---

_Referenced in [astral-sh/ruff#11101](../../astral-sh/ruff/issues/11101.md) on 2024-04-23 08:37_

---

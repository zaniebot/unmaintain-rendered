```yaml
number: 1200
title: "I001: Support for magic trailing commas"
type: issue
state: closed
author: Jackenmen
labels:
  - isort
assignees: []
created_at: 2022-12-11T22:04:15Z
updated_at: 2023-11-29T21:27:03Z
url: https://github.com/astral-sh/ruff/issues/1200
synced_at: 2026-01-12T15:54:41Z
```

# I001: Support for magic trailing commas

---

_@Jackenmen_

Black auto-explodes imports, collections, function calls, etc. when they contain a trailing comma:
https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#the-magic-trailing-comma

This means that even though this would fit into one line:
```py
from seven_dwwarfs import (
    Grumpy,
    Happy,
    Sleepy,
    Bashful,
    Sneezy,
    Dopey,
    Doc,
)
```
Black will NOT convert it into this:
```py
from seven_dwwarfs import Grumpy, Happy, Sleepy, Bashful, Sneezy, Dopey, Doc
```
As long as the import list contains a trailing comma.

Ruff currently does not have the magic trailing comma handling as far as I can tell and will collapse the import list if it can fit within the line length limit.

See relevant fixed (but not released) issue in isort: https://github.com/PyCQA/isort/issues/1683

---

_Label `isort` added by @charliermarsh on 2022-12-11 22:09_

---

_Comment by @charliermarsh on 2022-12-11 22:11_

Yeah this would be nice to support.


---

_Comment by @colin99d on 2022-12-23 21:42_

I am going to attempt this one.

---

_Comment by @charliermarsh on 2022-12-23 21:57_

Sweet. We'll probably need some logic to detect whether an import block ends in a trailing comma, then propagate that through as we normalize and combine the import blocks in the `isort` module, then thread through a flag to `format_import_from` to take the `format_multi_line` path if we have a trailing comma.

---

_Closed by @charliermarsh on 2022-12-26 14:40_

---

_Comment by @jaxwagner on 2023-11-29 20:36_

Hi! I'm encountering some strange behavior where the formatter will:
- add the comma if there is a single argument that takes up multiple lines
- remove the comma if single arg on one line
- add the comma if multiple args on multiple lines
in particular, the first bullet seems strange to me. I have the following configs:

```
...
[lint.isort]
...
split-on-trailing-comma = false
...
[format]
skip-magic-trailing-comma = true
```

is this expected behavior? Thanks so much!

---

_Comment by @charliermarsh on 2023-11-29 20:39_

@jaxwagner - Are you able to include a Python snippet to demonstrate the behavior you're seeing, and what you find surprising? Happy to help if so :)


---

_Comment by @jaxwagner on 2023-11-29 20:43_

```
def _func(
    input_values: Sequence[
        Union[CxxxxxxxxxxxxxxxxxxxxxxT, PxxxxxxxxxxxT, CxxxxxxxxxxxxT, BcccT]
    ],
) -> Type:
```
here's an example where the comma was added. this only happens when there is a single argument in the function that spans multiple lines. is this expected? thanks so much @charliermarsh ! :) 

in this case, there was an existing comma that was taken away:

```
async def cxxxxxxxxxxxxxxxxxxxxxxxxx_js(
    txxxxxxxxxxxir: Vxxxxxxxxxxxxxxxxxxxxy  # used to be a comma at end of this line
) -> bool:
```

---

_Comment by @charliermarsh on 2023-11-29 21:05_

@jaxwagner - Ahh yes, that first example is intended. However, if you shorten the annotation, the formatter _should_ re-collapse it given that you have `skip-magic-trailing-comma = true` set.

I can't reproduce that second example -- for me, the formatter is always adding a comma there (https://play.ruff.rs/b777c7bf-78e5-492d-aa3a-0869b930d565).

---

_Comment by @jaxwagner on 2023-11-29 21:21_

here's an example where the comma is removed:
https://play.ruff.rs/8c27c3f5-1f36-48b5-b4d0-dac32a05ce02

i'm seeing a ton like this

---

_Comment by @charliermarsh on 2023-11-29 21:23_

Ooh interesting, thank you! It's the trailing comment that makes the difference: https://play.ruff.rs/0c1f8900-2880-402e-a68b-0537d73e7b61

I'll file a ticket to explore this (it may be working as intended but it deserves a second look).

---

_Comment by @jaxwagner on 2023-11-29 21:27_

thanks @charliermarsh!

---

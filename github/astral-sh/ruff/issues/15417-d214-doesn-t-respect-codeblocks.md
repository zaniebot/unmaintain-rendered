---
number: 15417
title: "`D214` doesn't respect codeblocks"
type: issue
state: open
author: ezorita
labels:
  - bug
  - docstring
assignees: []
created_at: 2025-01-10T22:05:04Z
updated_at: 2025-01-11T16:32:15Z
url: https://github.com/astral-sh/ruff/issues/15417
synced_at: 2026-01-07T13:12:16-06:00
---

# `D214` doesn't respect codeblocks

---

_Issue opened by @ezorita on 2025-01-10 22:05_

When format on save is enabled in vscode, an indentation will be added to the nested docstring every time the file is saved:

```
def func():
    """This function does nothing.

    It must be documented using a Google-style docstring, though, like this other function:
    ```
    def func() -> str:
        '''This function does nothing.

        Args:
            param1 (type): Description of param1.
            param2 (type): Description of param2.

        Returns:
            str: Description of the return value.
        '''
    ```
    """
    pass

```

It can be fixed adding `#noqa: D214` but I wonder if this could be avoided in the first place.

---

_Comment by @zanieb on 2025-01-10 23:31_

This sounds like a `ruff format` bug, we may transfer this issue to the Ruff repository.

cc @MichaReiser 

---

_Comment by @MichaReiser on 2025-01-11 08:48_

I don't think this is a formatter bug because the formatter doesn't respect the `noqa: D214`. I also pasted the example and the formatted output into the playground and both result in the same formatting ([playground](https://play.ruff.rs/2d2941f3-a17b-4403-9150-d9cd9e72c1b7))

What I can reproduce is that the D214 fix here is somewhat nonsensical because it doesn't understand the code block and applying the fix dedents `Args` and `Returns` by one level which in turn indents `param1`, `param2`, and `str` by an extra level (but just once). The formatter than correctly aligns the inner docstring which then results in `D214` complaining about an over indent (See this [playground](https://play.ruff.rs/fed975bb-13e2-4af3-94a0-2c1af84780d4)). 


The underlying problem here isn't the formatter but that `D214` (and probably other docstring rules) doesn't respect code blocks



---

_Renamed from "Endless indentation in nested docstring" to "`D214` doesn't respect codeblocks" by @MichaReiser on 2025-01-11 08:49_

---

_Label `bug` added by @MichaReiser on 2025-01-11 08:49_

---

_Label `docstring` added by @AlexWaygood on 2025-01-11 16:32_

---

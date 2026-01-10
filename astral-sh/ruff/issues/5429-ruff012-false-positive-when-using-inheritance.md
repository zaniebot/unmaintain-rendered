```yaml
number: 5429
title: "RUFF012: False positive when using inheritance "
type: issue
state: open
author: frenck
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-06-28T21:02:44Z
updated_at: 2025-11-28T15:42:09Z
url: https://github.com/astral-sh/ruff/issues/5429
synced_at: 2026-01-10T11:09:47Z
```

# RUFF012: False positive when using inheritance 

---

_Issue opened by @frenck on 2023-06-28 21:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

ruff version: 0.0.275

I'm running into a false positive, as it ignores inheritance.

Example:

```py
class MyBaseClass:

    example: ClassVar[set[str]]

class NotMyBaseClass(MyBaseClass):

    example = {"O", "M", "G", "Puppies!"}
```

RUF012 will now trigger on the `NotMyBaseClass.example`: ```RUF012 Mutable class attributes should be annotated with `typing.ClassVar` ```

However, this is already the case...

../Frenck


---

_Label `bug` added by @charliermarsh on 2023-06-28 21:21_

---

_Label `type-inference` added by @charliermarsh on 2023-06-28 21:21_

---

_Comment by @charliermarsh on 2023-06-28 21:22_

Yeah we can't get this right at present. It's a bug. The workaround would just be to re-annotate:

```python
class NotMyBaseClass(MyBaseClass):
    example: ClassVar[set[str]] = {"O", "M", "G", "Puppies!"}
```

---

_Comment by @frenck on 2023-06-28 21:24_

> The workaround would just be to re-annotate:

Right... I understood that (and noticed), but that is kind of counterproductive / not a solution IMHO.



---

_Comment by @charliermarsh on 2023-06-28 21:26_

Totally, I agree that it's not a good solution, but it's probably the best I can suggest immediately apart from "turn off the rule" :)

---

_Comment by @frenck on 2023-06-28 21:27_

Re-annotation also overwrites the base. So, if the typing of the base class is changed, the children won't be updated (and might end up being incompatible, nothing warns for that at this time as it seems).

So, this can actually cause bugs instead of preventing them. From that perspective, I would even argue the rule currently encourages something really bad.

../Frenck

---

_Comment by @charliermarsh on 2023-06-28 21:31_

FWIW Mypy does warn on this, oddly Pyright does not:

```py
from typing import ClassVar

class MyBaseClass:
    example: ClassVar[set[str]]

class NotMyBaseClass(MyBaseClass):
    example: ClassVar[list[str]] = ["O", "M", "G", "Puppies!"]
```

---

_Comment by @frenck on 2023-06-28 21:32_

👍 relied on Pyright in my previous answer, ok, that makes it a little less worse 😄 

---

_Comment by @charliermarsh on 2023-06-28 21:33_

Haha yeah. I'm not sure. I guess I need to think on it. Like, we could probably support this when both classes are defined in the same file, and soon (TM), when both classes are defined locally (as opposed to in third-party modules). But I'm wondering if the rule is too unreliable right now to be worth including in Ruff.


---

_Comment by @charliermarsh on 2023-07-05 19:20_

In your case, do the base classes tend to be defined in the same file? In the same package (i.e., defined locally)? In third-party packages?

---

_Comment by @frenck on 2023-07-06 06:57_

> In your case, do the base classes tend to be defined in the same file

Nope, they are not.

In the same package (i.e., defined locally)? In third-party packages?

Yes, they are defined locally, as in, not originating from a third-party package.

../Frenck

---

_Comment by @nikkwong on 2024-01-10 06:01_

There's also a false negative, as no type error is raised when members that don't follow the types defined in a parent class:

```python
class MyBaseClass:
    @abstractmethod
    def my_method(self, arg: str) -> Dict:
        pass

# no type errors raised:
class NotMyBaseClass(MyBaseClass):
    def my_method(self):
        return "not a dict"
```

---

_Comment by @KerberosMorphy on 2025-11-28 15:42_

I was looking to report something similar so I'll join it here:

<img width="454" height="249" alt="Image" src="https://github.com/user-attachments/assets/4f71e571-5202-40ec-ac82-cb5cc36e4111" />

No error is detected in the `Test2` class for the `field` attribute.

---

```yaml
number: 3508
title: UP035 --fix not working for quoted types
type: issue
state: closed
author: torarvid
labels:
  - fixes
assignees: []
created_at: 2023-03-14T11:37:30Z
updated_at: 2023-03-22T16:45:54Z
url: https://github.com/astral-sh/ruff/issues/3508
synced_at: 2026-01-12T15:54:43Z
```

# UP035 --fix not working for quoted types

---

_@torarvid_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello hello 👋🏻 

When running `ruff --fix test.py` (Ruff v0.0.255) for the following test file:
```python
from typing import List

class A:
    myList: List[str]
    quotedList: "List[str]"
```
it will fix the first (non-quoted) variant, but still only report the second (quoted) variant. Unsure if this is a bug or expected behavior, but I thought I would report it anyways 🙂 

Keep up the awesome work 🎉 

---

_Comment by @torarvid on 2023-03-14 13:03_

This test file is even simpler (no `class A:` needed):
```python
from typing import List

myList: List[str] = []
quotedList: "List[str]" = []
```

---

_Comment by @JonathanPlasse on 2023-03-14 14:49_

This is the expected behavior. `pyupgrade` does not fix it either.
No forward references are auto-fixable as there are tricky corner cases.

---

_Label `autofix` added by @charliermarsh on 2023-03-14 15:43_

---

_Comment by @charliermarsh on 2023-03-14 15:46_

@JonathanPlasse - I can't remember why this is hard :joy: Do you have an example of a tricky corner case?

---

_Comment by @JonathanPlasse on 2023-03-14 20:46_

After spending some time in the issues, here it is!
I think that is all the problems we encountered.
- #2613.
  > Breaking pypa/build too: https://github.com/pypa/build/pull/576/files
  > ```python
  > - PathType = Union[str, 'os.PathLike[str]']
  > + PathType = Union[str, os.PathLike[str]]
  > 
  > - _TProjectBuilder = TypeVar('_TProjectBuilder', bound='ProjectBuilder')
  > + _TProjectBuilder = TypeVar('_TProjectBuilder', bound=ProjectBuilder)
  > ```
  > These are both broken. os.PathLike[str] is not valid on Python 3.7 (and it's not an annotation). TypeVar can't have the quotes removed, the thing it's bound to doesn't exist yet. And it's also not an annotation.
- #826
  > ```python
  > if TYPE_CHECKING:
  >     from app.models.model_a import ModelA
  >     from app.models.model_b import ModelB
  > 
  > ModelAOrB = Union["ModelA", "ModelB"]
  > ```
  > and it gets fixed into
  > ```python
  > if TYPE_CHECKING:
  >     from app.models.model_a import ModelA
  >     from app.models.model_b import ModelB
  > 
  > ModelAOrB = "ModelA" | "ModelB"
  > ```
  > which is not the same type as before

---

_Comment by @charliermarsh on 2023-03-14 21:02_

Thank you! :)

I'm wondering if we could fix (but leave quoted) instances like `quotedList: "List[str]" = []`, i.e., change that to `quotedList: "list[str]" = []` 🤔 

---

_Comment by @JonathanPlasse on 2023-03-14 21:11_

Yes, this makes sense.

---

_Comment by @charliermarsh on 2023-03-18 04:07_

Some of these are tricky (e.g., `quotedList: "Li" "st[str]" = []` is a valid annotation), but we should be able to fix the common cases.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-18 04:08_

---

_Closed by @charliermarsh on 2023-03-22 16:45_

---

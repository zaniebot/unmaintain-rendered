```yaml
number: 1405
title: ty allows Final class attribute to be overridden by property
type: issue
state: closed
author: benedekaibas
labels: []
assignees: []
created_at: 2025-10-20T15:38:30Z
updated_at: 2025-12-22T13:13:28Z
url: https://github.com/astral-sh/ty/issues/1405
synced_at: 2026-01-12T15:54:25Z
```

# ty allows Final class attribute to be overridden by property

---

_@benedekaibas_

### Summary

ty fails to detect when a `Final` class attribute is overridden by a property in a subclass while other type checkers detect it correctly. I have tested this code on various type checkers including: `zuban`, `pyrefly`, `ty`, and `mypy`


```python
from typing import Final

class Parent:
    id: Final[str] = "xyz"

class Child(Parent):
    @property
    def id(self) -> str:  # Overriding Final
        return "abc"

if __name__ == "__main__":
    print(Child().id)
```

### Version

ty 0.0.1-alpha.23


This issue might be worth taking a look at in my point of view!


---

_Comment by @AlexWaygood on 2025-10-20 15:50_

Hmm, the body of your issue seems to be about something different to what the title of your issue would imply

---

_Comment by @benedekaibas on 2025-10-20 15:52_

> Hmm, the body of your issue seems to be about something different to what the title of your issue would imply

Sorry for that, I have pasted the wrong code example since I have more. The correct title should be: Ty allows Final class attribute to be overridden by property

I have changed it to the correct title. Sorry for the confusion!

---

_Renamed from "Ty allows instantiation of abstract classes with unimplemented methods (false negative)" to "Ty allows Final class attribute to be overridden by property" by @benedekaibas on 2025-10-20 15:53_

---

_Comment by @AlexWaygood on 2025-10-20 15:55_

thanks!

---

_Renamed from "Ty allows Final class attribute to be overridden by property" to "ty allows Final class attribute to be overridden by property" by @MichaReiser on 2025-10-20 16:14_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 07:24_

---

_Comment by @AlexWaygood on 2025-12-22 13:13_

Looking at this again, I think this is the same as https://github.com/astral-sh/ty/issues/871

---

_Closed by @AlexWaygood on 2025-12-22 13:13_

---

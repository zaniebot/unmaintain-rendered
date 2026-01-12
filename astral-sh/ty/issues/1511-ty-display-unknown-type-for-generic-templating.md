```yaml
number: 1511
title: ty display Unknown type for generic templating function
type: issue
state: closed
author: viplazylmht
labels:
  - question
  - generics
assignees: []
created_at: 2025-11-10T10:13:42Z
updated_at: 2025-11-10T10:15:27Z
url: https://github.com/astral-sh/ty/issues/1511
synced_at: 2026-01-12T15:54:25Z
```

# ty display Unknown type for generic templating function

---

_@viplazylmht_

### Question

I am writing a generic templating function, which takes in a class, and return an instance of the class. 
```python
from typing import Type, TypeVar
T = TypeVar("T")

class Point:
  def __init__(self, x: int, y: int):
    self.x = x
    self.y = y
  def __repr__(self):
     return "<Point(x={}, y={})".format(self.x, self.y)

def instantiate(klass: Type[T], *args, **kwargs) -> T:
    return klass(*args, **kwargs)

the_point = instantiate(Point, 1, 4)
```

I am expecting the type of the  *the_point* instance shown by ty is **Point**, but got **Unknown** instead.
<img width="1290" height="628" alt="Image" src="https://github.com/user-attachments/assets/94748016-86c9-4adb-aac1-e036e60a181f" />

Is there any way to modify my function or update the ty to make it work? please help!

https://play.ty.dev/5e414d13-e79b-421d-aff1-523a7c826790


### Version

VS Code Extension (Version 2025.51.13022037)

---

_Label `question` added by @viplazylmht on 2025-11-10 10:13_

---

_Comment by @sharkdp on 2025-11-10 10:15_

Thank you for reporting this. This will be fixed by https://github.com/astral-sh/ty/issues/501.

---

_Closed by @sharkdp on 2025-11-10 10:15_

---

_Label `generics` added by @sharkdp on 2025-11-10 10:15_

---

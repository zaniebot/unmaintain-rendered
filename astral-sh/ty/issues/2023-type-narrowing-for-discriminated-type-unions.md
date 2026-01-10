```yaml
number: 2023
title: Type narrowing for discriminated type unions
type: issue
state: closed
author: konst-aa
labels:
  - question
assignees: []
created_at: 2025-12-17T17:29:55Z
updated_at: 2025-12-17T17:47:46Z
url: https://github.com/astral-sh/ty/issues/2023
synced_at: 2026-01-10T01:53:59Z
```

# Type narrowing for discriminated type unions

---

_Issue opened by @konst-aa on 2025-12-17 17:29_

### Question

Hello all, I've been really liking ty so far, it's substantially faster than any other python tooling I've used, and it's very stable

However, there have been some regressions since switching from mypy. The most prominent one is how ty handles discriminated type unions. When checked with mypy ([gist](https://mypy-play.net/?mypy=latest&python=3.12&gist=7ddb75c6262bd3c443be41feabf6b789)), this snippet gets a type error in the second case of the if statement. `ClassB` does not have the attribute `n`. However, ty does not recognize that `arg` has been constrainted to `ClassB`, and instead warns:

```2. Attribute `n` may be missing on object of type `ClassA | ClassB | ClassC` [possibly-missing-attribute]```

My colleagues and I use many discriminated unions in our codebase, so we have to `type: ignore` a fair bit. Do you plan to support this functionality? or are such unions unstandardized

```python3
from enum import StrEnum
from typing import Any, assert_never, Literal


class MyEnum(StrEnum):
    A = "A"
    B = "B"
    C = "C"

class ClassA:
    t: Literal[MyEnum.A] = MyEnum.A
    n: int

class ClassB:
    t: Literal[MyEnum.B] = MyEnum.B

class ClassC:
    t: Literal[MyEnum.C] = MyEnum.C

def fn(arg: ClassA | ClassB | ClassC):
    if arg.t is MyEnum.A:
        arg.n
    elif arg.t is MyEnum.B:
        print(arg.t)
        arg.n
    elif arg.t is MyEnum.C:
        print("C")
    else:
        assert_never(arg.t)
```


I hope this isn't a duplicate. I did a few searches and couldn't find a similar issue, though

### Version

ty 0.0.1-alpha.34 (Homebrew 2025-12-13)

---

_Label `question` added by @konst-aa on 2025-12-17 17:29_

---

_Comment by @arthur-st on 2025-12-17 17:33_

I'm afraid we may have reported the same thing at the same time, but it's all good. 👍 

#2022 

---

_Comment by @AlexWaygood on 2025-12-17 17:47_

Hey, thanks for the report. We do indeed intend to support this! Please see #1479

---

_Closed by @AlexWaygood on 2025-12-17 17:47_

---

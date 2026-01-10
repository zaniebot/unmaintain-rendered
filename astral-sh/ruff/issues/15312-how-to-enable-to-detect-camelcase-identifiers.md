```yaml
number: 15312
title: How to enable to detect camelCase identifiers?
type: issue
state: closed
author: rinarakaki
labels:
  - question
assignees: []
created_at: 2025-01-07T03:47:28Z
updated_at: 2025-01-07T14:52:15Z
url: https://github.com/astral-sh/ruff/issues/15312
synced_at: 2026-01-10T11:09:56Z
```

# How to enable to detect camelCase identifiers?

---

_Issue opened by @rinarakaki on 2025-01-07 03:47_

I want it to give warning messages when camelCase is used for an identifier (and it's hard to find the way partly because in ruff the word 'camel case' is used to mean PascalCase which makes it difficult to distinguish).
I appreciate it if anyone could teach me how to do it. Thanks.

---

_Comment by @InSyncWithFoo on 2025-01-07 07:04_

There are [multiple `N` rules](https://docs.astral.sh/ruff/rules/#pep8-naming-n) for this:

```python
from lorem import ipsum as fooBar  # N812: Lowercase `ipsum` imported as non-lowercase `fooBar`


fooBarGlobal = 42  # N816: Variable `fooBarGlobal` in global scope should not be mixedCase


class fooBarClass:  # N801: Class name `fooBarClass` should use CapWords convention
    fooBarClassVar = 42  # N815: Variable `fooBarClassVar` in class scope should not be mixedCase

    def fooBarMethod(  # N802: Function name `fooBarMethod` should be lowercase
        self,
        fooBarParameter  # N803: Argument name `fooBarParameter` should be lowercase
    ): ...


def fooBarFunction():  # N802: Function name `fooBarFunction` should be lowercase
    fooBarLocal = 42  # N806: Variable `fooBarLocal` in function should be lowercase


# etc.
```

Is that what you want?

---

_Label `question` added by @MichaReiser on 2025-01-07 07:20_

---

_Comment by @rinarakaki on 2025-01-07 14:52_

Thanks for the answer!

---

_Closed by @rinarakaki on 2025-01-07 14:52_

---

```yaml
number: 2157
title: "Explicitly including a protocol as a base class doesn't verify class implementation is actually compatible"
type: issue
state: closed
author: Ragzouken
labels: []
assignees: []
created_at: 2025-12-22T13:08:38Z
updated_at: 2025-12-22T14:02:32Z
url: https://github.com/astral-sh/ty/issues/2157
synced_at: 2026-01-10T01:56:41Z
```

# Explicitly including a protocol as a base class doesn't verify class implementation is actually compatible

---

_Issue opened by @Ragzouken on 2025-12-22 13:08_

### Summary

Looking at the documentation / faq / existing issues I can't see this reported/explained anywhere else, but I may not have looked for the right thing.

This [example from the python typing documentation](https://typing.python.org/en/latest/reference/protocols.html#defining-subprotocols-and-subclassing-protocols) doesn't seem to work as expected:
```python
from typing import Protocol

class SomeProto(Protocol):
    attr: int  # Note, no right hand side
    # If the body is literally just "...", explicit subclasses will
    # be treated as abstract classes if they don't implement the method.
    def method(self) -> str: ...

class ExplicitSubclass(SomeProto):
    pass

ExplicitSubclass()  # error: Cannot instantiate abstract class 'ExplicitSubclass'
                    # with abstract attributes 'attr' and 'method'
```

mypy gives an error as expected:
```bash
> mypy .\test.py    
test.py:16: error: Cannot instantiate abstract class "ExplicitSubclass" with abstract attributes "attr" and "method"  [abstract]
Found 1 error in 1 file (checked 1 source file)
```

ty gives no error, which is not correct (`ExplicitSubclass().method()` will return `None` which is not of type `str`):
```
> ty check .\test.py
All checks passed!
```



### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Comment by @AlexWaygood on 2025-12-22 14:02_

Thank you! I agree we don't have an issue specifically open for this right now, but this is basically the same as https://github.com/astral-sh/ty/issues/1877, so I'll close this in favour of that one and add a comment saying that we also need to treat `Protocol` methods as implicitly abstract 👍

---

_Closed by @AlexWaygood on 2025-12-22 14:02_

---

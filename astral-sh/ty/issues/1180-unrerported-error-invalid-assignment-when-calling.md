```yaml
number: 1180
title: "Unrerported `error[invalid-assignment]` when calling class functions"
type: issue
state: closed
author: KevinDuarte
labels: []
assignees: []
created_at: 2025-09-13T19:18:47Z
updated_at: 2025-09-15T07:27:57Z
url: https://github.com/astral-sh/ty/issues/1180
synced_at: 2026-01-10T02:06:25Z
```

# Unrerported `error[invalid-assignment]` when calling class functions

---

_Issue opened by @KevinDuarte on 2025-09-13 19:18_

### Summary

Hello, I am using `ty==0.0.1a20` and `Python 3.10.12`, and have found that ty does not catch some errors when related to class-functions. I have the minimal example:
```
class foo():
    def a(self) -> tuple[int, int, int, int]:
        return 0, 0, 0, 0
    
    def b(self) -> None:
        x, y, z = self.a()
```

When calling `ty check test.py` on the above, no error is thrown. However, the error `error[invalid-assignment]: Too many values to unpack` should be thrown. In the case, when it is not tied to a class function, e.g.
```
def a() -> tuple[int, int, int, int]:
    return 0, 0, 0, 0

x, y, z = a()
```
then the error is correctly thrown. This seems like a bug, which I have not seen previously reported.

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @sharkdp on 2025-09-15 07:26_

Thank you for reporting this.

ty currently still infers `Unknown` for an un-annotated `self` in methods, which is why the full `self.a()` expression is inferred as `Unknown` (which is why there is no error on the invalid assignment). We hope to fix this soon. See https://github.com/astral-sh/ty/issues/159

---

_Closed by @sharkdp on 2025-09-15 07:26_

---

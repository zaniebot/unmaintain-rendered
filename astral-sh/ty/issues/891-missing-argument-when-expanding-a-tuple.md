```yaml
number: 891
title: missing-argument when expanding a tuple
type: issue
state: closed
author: PetterS
labels: []
assignees: []
created_at: 2025-07-25T13:57:33Z
updated_at: 2025-07-28T07:35:42Z
url: https://github.com/astral-sh/ty/issues/891
synced_at: 2026-01-10T02:06:24Z
```

# missing-argument when expanding a tuple

---

_Issue opened by @PetterS on 2025-07-25 13:57_

### Summary

Example code:
```
import random

def test(r: tuple[int, int]):
    x = random.randint(*r)
    print(x)
```
ty `0.0.1-alpha.15 (0369a3598 2025-07-18)` says
```
error[missing-argument]: No arguments provided for required parameters `a`, `b` of bound method `randint`
 --> ty_test.py:4:9
  |
3 | def test(r: tuple[int, int]):
4 |     x = random.randint(*r)
  |         ^^^^^^^^^^^^^^^^^^
5 |     print(x)
  |
info: rule `missing-argument` is enabled by default
```

Interestingly, this works in the playground, but I'm not sure exactly which version is running there..

### Version

0.0.1-alpha.15 (0369a3598 2025-07-18)

---

_Comment by @dcreager on 2025-07-25 14:12_

Thanks for your report!

This was just fixed in https://github.com/astral-sh/ruff/pull/18996. It works in the playground because the playground runs the last `main` branch from the `ty` repo, so it already incorporated the fix. A new version is literally moments away, and should resolve this for you!

---

_Comment by @sharkdp on 2025-07-25 14:17_

Can confirm that this is fixed in 0.0.1-alpha.16:
```
▶ uvx ty --version                   
ty 0.0.1-alpha.16

~                                                                                                                                           
▶ uvx ty check test.py               
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!
```

---

_Closed by @sharkdp on 2025-07-25 14:17_

---

_Comment by @PetterS on 2025-07-28 07:35_

Cheers!

---

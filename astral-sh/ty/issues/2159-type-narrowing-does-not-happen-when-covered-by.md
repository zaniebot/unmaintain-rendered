```yaml
number: 2159
title: Type narrowing does not happen when covered by multiple branches
type: issue
state: closed
author: omerhadari
labels:
  - narrowing
assignees: []
created_at: 2025-12-22T13:57:38Z
updated_at: 2025-12-22T16:43:19Z
url: https://github.com/astral-sh/ty/issues/2159
synced_at: 2026-01-10T01:56:41Z
```

# Type narrowing does not happen when covered by multiple branches

---

_Issue opened by @omerhadari on 2025-12-22 13:57_

### Summary

Hi! Wasn't sure how to word the name of this issue.

The following code:
```Python
from typing import reveal_type


def check(a: str | None) -> None:
    if a:
        print("hey")
    elif a:
        print("ya")
    else:
        a = "a"
    reveal_type(a)
```

produces the following output in `ty`:
```
info[revealed-type]: Revealed type
 --> bla.py:9:17
  |
7 |     else:
8 |         a = "a"
9 |     reveal_type(a)
  |                 ^ `str | None`
  |

Found 1 diagnostic
```

and in `mypy`:
```
bla.py:9: note: Revealed type is "builtins.str"
```

The specific example is a bit weird I realize, but there are real use cases in the code base where I found this for example:


```Python
def check(conf: dict[str, str] | None, flag: bool) -> None:
    if conf and flag:
        print("hey")
    elif conf and not flag:
        print("ya")
    else:
        return
    print(conf.get("key"))
```

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Label `narrowing` added by @AlexWaygood on 2025-12-22 14:26_

---

_Comment by @AlexWaygood on 2025-12-22 14:31_

I think this may be https://github.com/astral-sh/ty/issues/690

---

_Comment by @omerhadari on 2025-12-22 14:38_

> I think this may be [#690](https://github.com/astral-sh/ty/issues/690)

I think it is actually, I searched but missed it. You can close this one (or I can) :)

Thank you!

edit: I do think it should be a single issue, but maybe the following quote from the original issue doesn't cover the specific case I encountered:
> we just discard any narrowing constraint that is present on only one path.

It's not just when the narrowing constraint is present on only one path, but also when it is present on all paths (but there is more than one boolean expression, e.g. with `elif`)

---

_Closed by @carljm on 2025-12-22 16:43_

---

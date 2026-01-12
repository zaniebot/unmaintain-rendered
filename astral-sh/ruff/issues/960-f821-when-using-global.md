```yaml
number: 960
title: "F821 when using `global` "
type: issue
state: closed
author: patrick91
labels:
  - bug
assignees: []
created_at: 2022-11-29T12:51:45Z
updated_at: 2022-12-09T22:50:07Z
url: https://github.com/astral-sh/ruff/issues/960
synced_at: 2026-01-12T15:54:40Z
```

# F821 when using `global` 

---

_@patrick91_

I'm getting this error when running ruff on a code like this:

```
import textwrap
from typing import List

import strawberry


def test_entities_type_when_no_type_has_keys():
    global Review

    @strawberry.federation.type
    class User:
        username: str

    @strawberry.federation.type(extend=True)
    class Product:
        upc: str = strawberry.federation.field(external=True)
        reviews: List["Review"]
```

(we use global as a work-around for some typing issues) :)

This is the codebase I'm using Ruff against: https://github.com/strawberry-graphql/strawberry/pull/2367

---

_Comment by @charliermarsh on 2022-11-29 15:28_

I'll look into this and see what I can do :)

---

_Comment by @charliermarsh on 2022-11-30 00:45_

I guess the issue is that `del Review` removes `Review` from the scope, and so when we go to evaluate the forward annotation, it's gone.

Could I help with removing the `global` / `del`? How are you testing that code?

---

_Label `question` added by @charliermarsh on 2022-11-30 01:18_

---

_Comment by @patrick91 on 2022-11-30 10:37_

The code needs global to allow get_type_hints to work at runtime, since it tries to find the forward ref from the global scope[1].

Maybe I can remove the `del`, but in theory the code is fine, no? 

For now, I disabled that error for those files though, I'm fine with ignoring it for those tests 😊

[1] Reference: https://mail.python.org/archives/list/typing-sig@python.org/thread/SNKJB2U5S74TWGDWVD6FMXOP63WVIGDR/#A5JFR7RDE7NNKESM2DWYM3ZT5ZYGOLCF

---

_Comment by @charliermarsh on 2022-11-30 14:05_

Ohhh I see, it's relied on for runtime behavior. Got it! I thought it was for Mypy.

---

_Label `question` removed by @charliermarsh on 2022-12-01 21:16_

---

_Label `bug` added by @charliermarsh on 2022-12-01 21:16_

---

_Comment by @charliermarsh on 2022-12-09 22:50_

This actually got fixed in #1154!

---

_Closed by @charliermarsh on 2022-12-09 22:50_

---

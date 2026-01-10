```yaml
number: 6619
title: "Feature Request: Offer validation of exported items in `__all__`"
type: issue
state: closed
author: kkirsche
labels: []
assignees: []
created_at: 2023-08-16T15:20:28Z
updated_at: 2023-08-16T16:21:34Z
url: https://github.com/astral-sh/ruff/issues/6619
synced_at: 2026-01-10T11:09:48Z
```

# Feature Request: Offer validation of exported items in `__all__`

---

_Issue opened by @kkirsche on 2023-08-16 15:20_

This feature request is to ask if it would be possible to provide optional support for validating that the contents are `__all__` in an `__init__.py` file or equivalent are actually imported or cannot be determined due to a `from package import *` method.

For example:

```python
from package.module.client import A
from package.module.enums import B
from package.module.server import C

__all__ = ["A", "B", "C", "D"]
```

In this example, it would be great if ruff was able to warn the developer that `D` is not imported and because there are no `*` imports, it doesn't appear to exist 

---

_Comment by @edgarrmondragon on 2023-08-16 15:24_

I think this is covered by [F405](https://beta.ruff.rs/docs/rules/undefined-local-with-import-star-usage/) and [F822](https://beta.ruff.rs/docs/rules/undefined-export/).

---

_Comment by @kkirsche on 2023-08-16 15:26_

Huh, I haven't triggered those before for it. Let me see if I can trigger those or if I had a misconfigured ruff instance. Thanks for letting me know about those.

I think the reason it usually doesn't trigger tools is that "A", "B", "C" are strings and most tools only look for the types because they're parsing the AST rather than the content. I'll give that a shot though!

---

_Comment by @edgarrmondragon on 2023-08-16 16:06_

FWIW

```python
# myfile.py
from package.module.client import A
from package.module.enums import B
from package.module.server import C

__all__ = ["A", "B", "C", "D"]
```

```console
$ ruff check --isolated --select F myfile.py
myfile.py:5:1: F822 Undefined name `D` in `__all__`
Found 1 error.
```

---

```python
# myfile.py
from package.module.client import A
from package.module.enums import B
from package.module.server import *

__all__ = ["A", "B", "C", "D"]
```

```console
$ ruff check --isolated --select F myfile.py
myfile.py:3:1: F403 `from package.module.server import *` used; unable to detect undefined names
myfile.py:5:1: F405 `C` may be undefined, or defined from star imports
myfile.py:5:1: F405 `D` may be undefined, or defined from star imports
Found 3 errors.
```


---

_Comment by @kkirsche on 2023-08-16 16:21_

I'll close this then and look into my config. Thank you and sorry for the overhead caused by this issue.

---

_Closed by @kkirsche on 2023-08-16 16:21_

---

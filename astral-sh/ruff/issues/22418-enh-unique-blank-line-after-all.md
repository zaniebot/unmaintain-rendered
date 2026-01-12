```yaml
number: 22418
title: "[ENH] Unique blank line after `__all__`?"
type: issue
state: closed
author: eliegoudout
labels:
  - question
assignees: []
created_at: 2026-01-06T16:01:01Z
updated_at: 2026-01-08T01:03:59Z
url: https://github.com/astral-sh/ruff/issues/22418
synced_at: 2026-01-12T15:54:58Z
```

# [ENH] Unique blank line after `__all__`?

---

_@eliegoudout_

### Summary

Hello,

I noticed that `ruff format` does not enforce a number of blank lines after `_all_` declaration. Indeed, both the following are okay according to `ruff`:
```python
"""Sample module header with unique blank line."""

import __main__

__all__ = ["foo"]

import inspect
from types import MappingProxyType

import numpy as np
```
and
```python
"""Sample module header with double blank line."""

import __main__

__all__ = ["foo"]


import inspect
from types import MappingProxyType

import numpy as np
```

I personally suggest that we enforce only one blank line for header consistency (first example).

Any feedback would be appreciated.

All the best!
Élie


---

_Label `question` added by @amyreese on 2026-01-06 16:04_

---

_Comment by @amyreese on 2026-01-06 17:41_

I believe this is expected behavior given the way ruff formatting works with imports.

You can see similar behavior of allowing 1 or 2 blank lines between imports, without an `__all__` assignment between them:

```
$ cat foo.py
import thing


import sys

import x as y

$ uvx ruff format --diff foo.py
1 file already formatted
```

However, adding a third blank line will cause ruff to normalize that back to two blank lines:

```
$ cat foo.py
import thing



import sys

import x as y

$ uvx ruff format --diff foo.py
--- foo.py
+++ foo.py
@@ -1,7 +1,6 @@
 import thing


-
 import sys

 import x as y

1 file would be reformatted
> [1]
```

---

_Comment by @MichaReiser on 2026-01-07 08:45_

While this is related to import formatting because the example uses `__all__` between imports,  it's more about that Ruff doesn't apply any specific blank-line rules for `__all__`. For example, Ruff leaves the following code unchanged (without adding any blank lines around `__all__`)

```py
"""Sample module header with unique blank line."""

a = 1
__all__ = ["foo"]

b = 2
```

There's a tough balance that a formatter has to strike: It must be opinionated enough so that the code style looks consistent but it shouldn't be too opinionated, or many users dislike the style and the tool becomes useless (or less useful) to them. There are multiple open issues requesting less strict blank-line rules in Ruff, and I'm highly empathetic toward them.

I find the blank line rules too strict in general, and we should not introduce any new ones unless they have very strong community support. I don't think this is the case here. 



---

_Comment by @eliegoudout on 2026-01-08 01:03_

Ok, thank you both for your feedbacks! All the best :)

---

_Closed by @eliegoudout on 2026-01-08 01:03_

---

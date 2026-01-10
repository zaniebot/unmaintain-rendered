---
number: 3059
title: "isort: bug in sorting sub-package modules"
type: issue
state: closed
author: spaceone
labels:
  - isort
assignees: []
created_at: 2023-02-20T14:42:00Z
updated_at: 2023-03-29T03:53:40Z
url: https://github.com/astral-sh/ruff/issues/3059
synced_at: 2026-01-10T01:22:41Z
---

# isort: bug in sorting sub-package modules

---

_Issue opened by @spaceone on 2023-02-20 14:42_

`.isort.cfg`
```ini
[settings]
known_first_party=foo.bar,baz
known_third_party=foo,blub,blah
```

valid isort-compatible code:
```python
import sys

import blah
import blub
import foo
from foo import bar, baz

import baz
import foo.bar
from foo.bar import blah, blub
```

`pyproject.toml`:
```toml
[tool.ruff]
select = ["I"]
[tool.ruff.isort]
known-first-party = ["foo.bar", "baz"]
known-third-party = ["foo", "blub", "blah"]
```

```console
$ ruff --fix --diff foo.py
--- foo.py
+++ foo.py
@@ -3,8 +3,8 @@
 import blah
 import blub
 import foo
+import foo.bar
 from foo import bar, baz
+from foo.bar import blah, blub
 
 import baz
-import foo.bar
-from foo.bar import blah, blub

Would fix 1 error.
```


---

_Renamed from "isort: bugs in sorting third party modules" to "isort: bug in sorting sub-package modules" by @spaceone on 2023-02-20 14:42_

---

_Comment by @charliermarsh on 2023-02-20 14:44_

I'm tempted to call this unsupported. Isn't the issue here that you're marking a module and one of its submodules as third- and first-party respectively?

---

_Comment by @spaceone on 2023-02-20 14:48_

ruff should be isort compatible, right?

we have a main package `foo` with many different sub-packages as `foo.bar` or `foo.blub`.
In the code of `foo.bar` we want that `foo.bar` is a first-party module and everything else (`foo` and `foo.blub`) is just a third-party module.

---

_Label `isort` added by @charliermarsh on 2023-02-20 14:54_

---

_Comment by @charliermarsh on 2023-02-20 17:23_

The intent is to be `isort`-compatible, but we do deviate in a small number of cases, and this to me seems like a situation where there isn't a clear definition of correct behavior. I'd be open to doing what `isort` does rather than our current behavior, but I won't personally prioritize it.

---

_Referenced in [astral-sh/ruff#3064](../../astral-sh/ruff/pulls/3064.md) on 2023-02-20 18:26_

---

_Comment by @spaceone on 2023-02-20 18:39_

I provided a fix in #3064. Isort has a test case for it: https://github.com/PyCQA/isort/blob/main/tests/unit/test_isort.py#L2213-L2304 → `test_placement_control()` and `test_custom_sections()` 

---

_Comment by @astaric on 2023-03-27 08:15_

One usecase where we encounter the same issue are namespace packages. We have different packages (in different repositories) share the same namespace package.

With the following config, isort lists `namespace.packageA` imports in the last section while all other namespace imports are listed in the third party section above.
```
known_first_party = namespace.packageA
default_section = THIRDPARTY
```

I played around with `known_first_party` and `known_third_party` options but I cannot get packages with the same namespace prefix to end up in different sections.

---

_Referenced in [astral-sh/ruff#3768](../../astral-sh/ruff/pulls/3768.md) on 2023-03-28 12:41_

---

_Closed by @charliermarsh on 2023-03-29 03:53_

---

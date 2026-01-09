---
number: 4666
title: "E124 / E125 / E128 / E129 / E303: Mentioned by pycodestyle but not ruff"
type: issue
state: closed
author: buhtz
labels: []
assignees: []
created_at: 2023-05-26T09:20:14Z
updated_at: 2023-05-26T10:09:34Z
url: https://github.com/astral-sh/ruff/issues/4666
synced_at: 2026-01-07T13:12:14-06:00
---

# E124 / E125 / E128 / E129 / E303: Mentioned by pycodestyle but not ruff

---

_Issue opened by @buhtz on 2023-05-26 09:20_

Hello,
maybe I misunderstand the intention of ruff. So please take my apology if this issue doesn't make sense.

I have errors mentioned by pycodestyle (`2.6.0`) but not by ruff (`0.0.270`) on Debian 11.

I created example code to illustrate the details:

```
#!/usr/bin/env python3
class Foo:
    def __delattr__(self, name):
        pass


    def bar(self):
        if (True
            and False):
            self.one('two')

    def one(self,
        text: str
        ):
        pass

```

`ruff` keeps quite about this file and has no problem with it.
But `pycodestyle` says:

```
lint.py:7:5: E303 too many blank lines (2)
lint.py:9:5: E129 visually indented line with same indent as next logical line
lint.py:13:9: E128 continuation line under-indented for visual indent
lint.py:14:9: E124 closing bracket does not match visual indentation
lint.py:14:9: E125 continuation line with same indent as next logical line
```

---

_Comment by @tjkuson on 2023-05-26 09:35_

Yeah, they aren't implemented in ruff yet (#2402). My interpretation of the mentioned issue is that the rules should be implemented in the future.

---

_Comment by @buhtz on 2023-05-26 10:06_

Ah thanks. Then I close this as a duplicate of #2402 .
Thanks a lot for your work.

---

_Closed by @buhtz on 2023-05-26 10:06_

---

_Comment by @tjkuson on 2023-05-26 10:09_

No worries! Happy to help.

Also, I am only a minor contributor. I would hate to steal valour from those who do most of the work on this tool!

---

_Referenced in [sphinx-doc/sphinx#11902](../../sphinx-doc/sphinx/pulls/11902.md) on 2024-01-21 20:28_

---

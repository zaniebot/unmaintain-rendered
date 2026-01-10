```yaml
number: 6869
title: ANN202 wrongly thinks public methods are private
type: issue
state: closed
author: sueskind
labels:
  - question
assignees: []
created_at: 2023-08-25T10:16:23Z
updated_at: 2024-08-14T07:54:13Z
url: https://github.com/astral-sh/ruff/issues/6869
synced_at: 2026-01-10T11:09:49Z
```

# ANN202 wrongly thinks public methods are private

---

_Issue opened by @sueskind on 2023-08-25 10:16_

- Ruff version: 0.0.285
- Selected rule: `ANN`

Given the following code in a file `_test.py`

```python
class A:
    def method(self):
        pass
```

I get the error

```
_test.py:2:9: ANN202 Missing return type annotation for private function `method`
```

which implies that the `method()` is private. But actually the module itself is private, not the method. This also happens when the module is in a deeper package, where at least one parent in the hierarchy is private, e.g. `some/package/_private/another/test.py`.

This issue may occur fairly often, as it is common in Python to restrict the public API to internal modules via underscore `_`. However, this does not mean that the method itself is private.

---

_Comment by @charliermarsh on 2023-08-25 14:00_

I believe this is intentional as it aligns with the definition of public and private used by [pydocstyle](https://github.com/PyCQA/pydocstyle). I do understand the argument against it, but I find the inverse to be confusing too -- if we reported these methods as public, we'd likely get folks reporting that they should be treated as private since they're in private modules.

---

_Comment by @sueskind on 2023-08-25 15:16_

Thank you for your explanation, you are probably right, especially if the current convention used by pydocstyle is like this.

I guess the underlying issue is that there are no clear definitions in Python what "public" and "private" means, as these keywords don't exist like in other languages...

If above described behavior is indeed wanted an expected, this issue can be closed.

---

_Closed by @zanieb on 2023-08-25 15:58_

---

_Label `question` added by @zanieb on 2023-08-25 15:58_

---

_Comment by @chriss1245 on 2024-08-14 07:54_

I have another question regarding this. I have the following code which does not return anything. I prefer to avoid writing with the arrow notation when functions do not return. I think that is clear enough. But the lintern forces me to use the def lol()  -> None:  notation.  

---

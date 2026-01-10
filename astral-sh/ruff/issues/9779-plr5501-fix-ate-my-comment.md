```yaml
number: 9779
title: "PLR5501 fix ate my comment!"
type: issue
state: closed
author: akx
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-02-02T09:07:18Z
updated_at: 2024-12-06T04:01:02Z
url: https://github.com/astral-sh/ruff/issues/9779
synced_at: 2026-01-10T11:09:52Z
```

# PLR5501 fix ate my comment!

---

_Issue opened by @akx on 2024-02-02 09:07_

### plr5501-testcase.py

```python
class Class:
    def can_do_thing(self, thing) -> bool:
        if self.thing_id == thing.id:
            return True
        if self.x:
            if self.y.z(thing):
                return True
        else:
            # This comment should not be eaten by automated fixes,
            # because it explains important context about the next if block.
            # I would be quite sad if it vanished, because I'm quite fond of it.
            if self.thing.color == thing.color_id:
                return True
        return False
```
### `ruff --select=PLR5501 --isolated --diff plr5501-testcase.py`
```diff
--- plr5501-testcase.py
+++ plr5501-testcase.py
@@ -5,10 +5,6 @@
         if self.x:
             if self.y.z(thing):
                 return True
-        else:
-            # This comment should not be eaten by automated fixes,
-            # because it explains important context about the next if block.
-            # I would be quite sad if it vanished, because I'm quite fond of it.
-            if self.thing.color == thing.color_id:
-                return True
+        elif self.thing.color == thing.color_id:
+            return True
         return False
```

# Other info

* Ruff 0.2.0 on a Mac.
* I really, really liked that comment. 😭 
* I guess the best thing to do is to move the comment inside the `elif`?

---

_Label `bug` added by @AlexWaygood on 2024-02-02 11:01_

---

_Label `autofix` added by @AlexWaygood on 2024-02-02 11:01_

---

_Comment by @zanieb on 2024-02-02 16:28_

Sorry! We're trying to figure out what fix safety looks like with dropping comments. Retaining comments is really hard and we'd need to invest quite a bit to never drop them, but we're hoping to have a way to treat fixes as unsafe if there's a comment in the edit range.

---

_Comment by @zanieb on 2024-02-02 16:35_

Tracking this in https://github.com/astral-sh/ruff/issues/9790

---

_Comment by @InSyncWithFoo on 2024-12-06 02:49_

This seems to have been resolved by #13573.

---

_Comment by @zanieb on 2024-12-06 04:01_

Thanks!

---

_Closed by @zanieb on 2024-12-06 04:01_

---

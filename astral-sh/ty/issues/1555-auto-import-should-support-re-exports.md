```yaml
number: 1555
title: Auto-import should support re-exports
type: issue
state: closed
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-11-14T13:42:15Z
updated_at: 2025-12-04T18:21:28Z
url: https://github.com/astral-sh/ty/issues/1555
synced_at: 2026-01-12T15:54:25Z
```

# Auto-import should support re-exports

---

_@BurntSushi_

At present, ty's auto-import feature doesn't understand re-exports. While this can manifest in a number of ways, one practical real world problem is that it makes auto-import with numpy effectively useless:

https://github.com/user-attachments/assets/1b81a0e0-776b-48c8-8809-eb6bbc339fe5

In the demo above, I _want_ to use `numpy.array`. But that corresponds to none of the options presented to me. Instead, it provides options for `array` within the sub-modules of `numpy`, but that's not what I want.

This is because `numpy` crafts its public API by re-exporting symbols from sub-modules. While ty itself understands this (e.g., `numpy.arra<CURSOR>` works as expected), auto-import does not. This is because auto-import does its own AST visit to try and very quickly extract exported symbols from dependencies without needing to do full type checking on all of them.

What we'd like to do is implement logic for detecting the [library interface](https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols). We already have this in ty, but reusing it _outside_ of ty is difficult. And auto-import already has its own AST visitor anyway. We may not be able to fully support some things without proper type checking (handling `__all__` correctly in all cases comes to mind), but we should be able to handle most cases. And where we can't, we should probably prefer false positives over false negatives when possible.

(I've already started work on this but paused it to work on some lower hanging polishing for completions.)

---

_Added to milestone `Stable` by @BurntSushi on 2025-11-14 13:42_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-11-14 13:42_

---

_Label `server` added by @BurntSushi on 2025-11-14 13:42_

---

_Label `completions` added by @BurntSushi on 2025-11-14 13:42_

---

_Closed by @BurntSushi on 2025-12-04 18:21_

---

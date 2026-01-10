```yaml
number: 2282
title: "Support dynamic type guards with `hasattr` in a loop"
type: issue
state: open
author: phistep
labels:
  - wish
  - narrowing
assignees: []
created_at: 2025-12-30T23:32:44Z
updated_at: 2026-01-06T01:59:58Z
url: https://github.com/astral-sh/ty/issues/2282
synced_at: 2026-01-10T01:56:41Z
```

# Support dynamic type guards with `hasattr` in a loop

---

_Issue opened by @phistep on 2025-12-30 23:32_

### Summary

Would it be possible to support iterating over attribute names and type guarding in the loop body?

This works, but is cumbersome

```python
import glfw

if not hasattr(glfw, attr := "window_hint_string"):
    raise RuntimeError(f"GLFW is missing required attribute: {attr}")
if not hasattr(glfw, attr := "set_window_opacity"):
    raise RuntimeError(f"GLFW is missing required attribute: {attr}")
if not hasattr(glfw, attr := "set_window_attrib"):
    raise RuntimeError(f"GLFW is missing required attribute: {attr}")

# no warning
glfw.window_hint_string(glfw.COCOA_FRAME_NAME, title)
```

`glwf`is annative  extension module that defines some members conditionally , see https://github.com/FlorianRhiem/pyGLFW/issues/87#issuecomment-3700678303

I would rather want to write it like this
```python
for attr in ["window_hint_string", "set_window_opacity", "set_window_attrib"):
    if not hasattr(glfw, attr):
        raise RuntimeError(f"GLFW is missing required attribute: {attr}")

# warning[possibly-missing-attribute]: Member `window_hint_string` may be missing on module `glfw`
glfw.window_hint_string(glfw.COCOA_FRAME_NAME, title)
```

Unsure if this is realistic or even possible given that one would have to actually execute the code. But for simple patterns like this, I assume that `ty` could infer all possible values of `attr` statically and doe the expansion?

(side note: `warning[possibly-missing-attribute]` made me find this bug before it happened, thank you! :​­) https://github.com/phistep/VHSh/blob/a7f930bc47f9d4eae2be3ef329c79ef29849a259/src/vhsh/window.py#L57-L59

### Version

```
~/Library/Application Support/Zed/languages/ty/ty-0.0.7/ty-aarch64-apple-darwin/ty
```

Bundled with Zed `0.217.3+stable.105.80433cb239e868271457ac376673a5f75bc4adb1`

---

_Comment by @MeGaGiGaGon on 2025-12-30 23:48_

One less cumbersome way to do this would be with a [`runtime_checkable`](https://docs.python.org/3/library/typing.html#typing.runtime_checkable) [`Protocol`](https://docs.python.org/3/library/typing.html#typing.Protocol), since the way it checks at runtime is the same as a bunch of `hasattr`s. The downside with that is it may not currently work, since #1858 is still open, but should work in the future.


---

_Label `narrowing` added by @mtshiba on 2025-12-31 02:12_

---

_Label `wish` added by @carljm on 2026-01-06 01:47_

---

_Comment by @carljm on 2026-01-06 01:59_

This would be very cool, but also quite difficult, I think. It requires not only knowing that `attr` is of type `Literal["window_hint_string", "set_window_opacity", "set_window_attrib"]` (which we already know, if you use a tuple instead of list literal to iterate over), but also knowing that the code inside the loop is executed once for each of those values. This would require significant special-casing of control flow understanding for the particular case of iterating over certain kinds of iterable literals.

Also, no other type checker is able to handle this.

I'll keep the issue open, but it's not likely to be a near-term priority.

---

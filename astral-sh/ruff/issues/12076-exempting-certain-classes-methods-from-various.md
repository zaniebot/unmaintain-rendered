```yaml
number: 12076
title: Exempting certain classes/methods from various things
type: issue
state: open
author: benjamin-kirkbride
labels:
  - rule
assignees: []
created_at: 2024-06-27T18:46:36Z
updated_at: 2024-09-05T02:26:59Z
url: https://github.com/astral-sh/ruff/issues/12076
synced_at: 2026-01-12T15:54:51Z
```

# Exempting certain classes/methods from various things

---

_@benjamin-kirkbride_

A lot of frameworks provide classes/functions that have a set number of arguments. It is idiomatic and/or required to have them in place even when not used (ARG002), and they often have too many args (PLR0913).

An example of that from Pyglet:

```python
def on_mouse_drag(x, y, dx, dy, buttons, modifiers):
    pass
```

It is common to only need a couple of those arguments, but they all must be there for it to function properly. It is possible to `_` the args you don't need to get around ARG002, but that is not idiomatic and confuses thing.

Other examples exist that I don't have immediately on hand, things like Django class based views.

My proposal is a way to exempt classes (in the case of the init) and method names from these (and possibly other related?) checks.

Why not just disable those lints altogether? Because I want my own custom functions and methods to be held to a certain standard. Why not disable those checks on the functions where they come up? Because I will end up with dozens or hundreds of noqa comments throughout a medium sized program.

---

_Comment by @benjamin-kirkbride on 2024-06-27 18:47_

matching a regex pattern for this would be awesome, but that may be too much to ask :sweat_smile: 

In Pyglet for instance, exempting `on_mouse_*` and `on_key_*` go a huge way towards fixing this.

---

_Label `wish` added by @MichaReiser on 2024-06-28 07:09_

---

_Label `wish` removed by @MichaReiser on 2024-06-28 07:09_

---

_Label `rule` added by @MichaReiser on 2024-06-28 07:09_

---

_Comment by @fschulze on 2024-08-10 15:10_

Something along the lines of exceptions for mypy could also be helpful in many cases:

```python
def on_mouse_drag(x, y, dx, dy, buttons, modifiers):  # noqa: ARG002[x,y,buttons,modifiers]
    pass
```
The above should complain if ``dx`` or ``dy`` are unused, but not the others and it should fail if ``x``, ``y``, ``buttons`` or ``modifiers`` are used. That way one can be very specific and any changes will show up.

---

_Comment by @eddielu on 2024-09-05 02:26_

Another example is django (especially DRF) custom views, where we need to keep a pk in the function parameter for DRF to work, but in reality, we use self.get_object() to get the instance. In this case, our code looks really ugly because we have so many # noqa: ARG002 everywhere.

```
def clone_lunch(self, request, pk):  # noqa: ARG002
  instance = self.get_object()
```

---

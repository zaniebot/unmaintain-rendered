```yaml
number: 169
title: "Change range for `unresolved-attribute` error to attribute"
type: issue
state: open
author: MichaReiser
labels:
  - diagnostics
  - attribute access
assignees: []
created_at: 2025-03-18T10:13:48Z
updated_at: 2025-11-13T16:08:50Z
url: https://github.com/astral-sh/ty/issues/169
synced_at: 2026-01-12T15:54:22Z
```

# Change range for `unresolved-attribute` error to attribute

---

_@MichaReiser_

The `unresolved-attribute` diagnostic currently highlights the entire attribute expression. I find this very noisy and the error, most of the time is with the attribute and not with the object. We could also use a secondary range to highlight the object's definition. 

We should revisit the range of our unresolved-attribute diagnostics. 

---

_Label `diagnostics` added by @MichaReiser on 2025-03-18 10:13_

---

_Label `good first issue` added by @MichaReiser on 2025-03-18 10:13_

---

_Label `good first issue` removed by @MichaReiser on 2025-03-18 12:44_

---

_Comment by @AlexWaygood on 2025-03-18 12:48_

I personally prefer it how it is because:
- It is the attribute expression as a whole that leads to a (possible) exception being raised at runtime, and that involves the left-hand side of the `.` operator just as much as the right-hand side
- The current behaviour makes it consistent with how we treat other operators such as `+` and `-`, where we highlight the whole expression (both the left-hand and right-hand sides of the operator)
- The current behaviour is consistent with how CPython itself highlights the range in its exception tracebacks:

  ```pytb
  >>> x.foo.bar
  Traceback (most recent call last):
    File "<python-input-7>", line 1, in <module>
      x.foo.bar
      ^^^^^
  AttributeError: 'object' object has no attribute 'foo'
  ```

That said, I generally prefer not to use a language server at all in my editor when I'm writing Python (I generally use a lightweight text editor and run my type checker via the command line), and the highlighted range is perhaps most important for users of IDEs. So I'm happy to weigh other people's opinions more strongly here.

---

_Comment by @MichaReiser on 2025-03-21 09:55_

Here an example where I think the current highlighting is very problematic:

<img width="788" alt="Image" src="https://github.com/user-attachments/assets/b22a255f-8484-4eb6-bcbd-5a4edabee579" />

([Playground](https://playknot.ruff.rs/1b231df0-81d7-4de5-9781-2d2712fb7df8))

Only the last attribute access is invalid but red knot highlights the entire attribute access. We should at least change it so that it only highlights `flags.is_` and not the entire `out.flags.is_` expression

It becomes unnecessarily noisy when the object expression becomes more complicated:

```python
returns_output().flags.set(key, Flags.EXPLICIT_NEST, recursive=False)
^^^^^^^^^^^^^^^^^^^^^^^^^^

def returns_output() -> Output:
    pass

```

---

_Comment by @sharkdp on 2025-03-23 08:57_

One edge case would be a class with an invalid `__getattr__` method (e.g. invalid signature). In this case, it might be better to highlight the accessed instance, rather than the attribute.

---

_Label `diagnostics` added by @MichaReiser on 2025-05-07 15:21_

---

_Renamed from "[red-knot] Change range for `unresolved-attribute` error to attribute" to "Change range for `unresolved-attribute` error to attribute" by @MichaReiser on 2025-05-07 15:26_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:05_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 16:08_

---

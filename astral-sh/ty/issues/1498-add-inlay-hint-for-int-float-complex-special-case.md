```yaml
number: 1498
title: Add inlay hint for int/float/complex special case
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - server
assignees: []
created_at: 2025-11-07T02:21:19Z
updated_at: 2025-11-14T08:20:33Z
url: https://github.com/astral-sh/ty/issues/1498
synced_at: 2026-01-12T15:54:25Z
```

# Add inlay hint for int/float/complex special case

---

_@MeGaGiGaGon_

It would be nice if ty added an inlay hint on typings that use the int/float/complex special case. Currently, a type hint like this:
```py
x: float
```
Gets no inlay hint, while `x` is actually `int | float`. Ty does not try to hide this if you do `reveal_type(x)`, so it would be nice if for consistency an inlay hint was added also saying that.

Here's roughly what I would like to see, where the text in the comments would be the new inlay hints:

```py
a: float# | int
b: complex# | float | int
c: complex | float# | int
```

---

What follows are some additional thoughts on more complex cases, but even just working only with a single `float`/`complex` type hint would be a big improvement.

One wrinkle is what would happen on more complex unions where the special case is not the last item? (using `js` highlighting for inline comments)
```js
d: float | str// | int
e: float/* | int*/ | str
```
And what if it's float and complex, but split apart?
```js
f: complex | list | float | str// | int
f: complex | list | float/* | int*/ | str
g: complex/* | int*/ | str | float
```
What if it's an existing type with one of the union members?
```py
type MyType = int | None
h: MyType | complex# | float
i: MyType | complex# | float | int
```

---

_Label `server` added by @AlexWaygood on 2025-11-07 03:20_

---

_Comment by @MichaReiser on 2025-11-07 14:15_

I very well understand that `float` being an alias for `float | int` is surprising. I was surprised to learn about it. But I'm not convinced that adding an inlay to `x: float` is helpful. In fact, I think it would be rather confusing because it reads as `x: float | int` and I'd be confused whether I'm supposed to add `int` to the union. Inlays also have the downside that they take up a significant amount of space in your editor and finding the right level of verbosity is already a challenge. 

We do show `float | int` if you hover `x`, mirroring what reveal type does. I think this is sufficient and avoids repeating content. Can you say more why you want inlays in addition to hover?



---

_Closed by @MichaReiser on 2025-11-14 08:20_

---

---
number: 21581
title: Formatter should avoid splitting arguments into their own line
type: issue
state: open
author: injust
labels:
  - formatter
  - style
assignees: []
created_at: 2025-11-22T21:18:53Z
updated_at: 2025-11-25T18:38:09Z
url: https://github.com/astral-sh/ruff/issues/21581
synced_at: 2026-01-07T13:12:16-06:00
---

# Formatter should avoid splitting arguments into their own line

---

_Issue opened by @injust on 2025-11-22 21:18_

### Summary

This is a silly contrived example, but unfortunately I cannot share the original code.

`aa` and `bb` are identical, except the comparisons are reordered. Both lines are 61 characters long.

```python
foo = 5

aa = float("16.0") <= foo <= float("20.0") and 5 <= foo <= 10
bb = 5 <= foo <= 10 and float("16.0") <= foo <= float("20.0")
```

When the line length is set to 60, this is the result (https://play.ruff.rs/56dffb27-828b-4cff-8903-5153da7ad31b?secondary=Format):

```python
foo = 5

aa = (
    float("16.0") <= foo <= float("20.0") and 5 <= foo <= 10
)
bb = 5 <= foo <= 10 and float("16.0") <= foo <= float(
    "20.0"
)
```

When the line length is set to 59, this is the result(https://play.ruff.rs/a9d7d7fb-1535-4600-9dde-1bd94bca876f?secondary=Format):

```python
foo = 5

aa = (
    float("16.0") <= foo <= float("20.0")
    and 5 <= foo <= 10
)
bb = 5 <= foo <= 10 and float("16.0") <= foo <= float(
    "20.0"
)
```

In both cases, `aa` is obviously easier to read. Ideally, the formatter would format `bb` the same way.

In other words, putting an expression on a new line (in parentheses) or splitting on `and` should be preferred over splitting a function call's arguments into their own line.

However, this would be a deviation from Black, so I'm not really sure what could be done.

---

_Renamed from "Formatter should avoid splitting arguments from the last function call into a new line" to "Formatter should avoid splitting arguments into their own line" by @injust on 2025-11-22 21:26_

---

_Label `formatter` added by @amyreese on 2025-11-24 18:41_

---

_Label `style` added by @MichaReiser on 2025-11-25 18:30_

---

_Comment by @MichaReiser on 2025-11-25 18:38_

I agree that the inconsistency is a bit weird. The idea behind formatting it the way it is, is to avoid unnecessary parentheses, which is something that Black and Ruff both try very hard in expression contexts. 

Taking `aa`, the reason it's formatted differently is that breaking the `float("16.0")` over the boolean operation would look strange:


```py
aa = float(
	"16.0"
) <= foo <= float("20.0") and 5 <= foo <= 10
```

This is much harder to read because it requires scanning multiple lines.

On the other hand, the `bb` case is easier to read because there's nothing that comes after the closing parentheses of `float("20")` and it allows us to avoid an extra set of parentheses.

I think what makes this specific instance harder to read is that the comparsions both have a lower and upper bound. This in combination with `and` results in an overall fairly complicated expression that isn't easy to parse (at least, I find it not easy).



---

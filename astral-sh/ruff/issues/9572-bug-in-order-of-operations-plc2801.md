```yaml
number: 9572
title: Bug in order of operations (PLC2801)
type: issue
state: closed
author: RubenVanEldik
labels:
  - bug
assignees: []
created_at: 2024-01-18T09:27:19Z
updated_at: 2024-03-04T10:25:18Z
url: https://github.com/astral-sh/ruff/issues/9572
synced_at: 2026-01-10T11:09:51Z
```

# Bug in order of operations (PLC2801)

---

_Issue opened by @RubenVanEldik on 2024-01-18 09:27_

Hi all!

While updating from Ruff 0.1.11 to 0.1.13 I found a small bug in the newly created rule `PLC2801`.

I had the code:

```py
a = 2
b = -a.__sub__(1)
# b == -1
```

With the auto linter Ruff fixed this to:

```py
a = 2
b = -a - 1
# b == -3
```

The order of operations is changed, to fix this Ruff needs to add some parenthesis (`b = -(a-1)`). 

I have no experience with Rust, so I am not able to help, but I wanted to let you know of the issue. Thank you for making Rust! It's an absolutely amazing tool!!

---

_Renamed from "Bug in PLC2801" to "Bug in order of operations (PLC2801)" by @RubenVanEldik on 2024-01-18 09:27_

---

_Comment by @charliermarsh on 2024-01-18 13:39_

Oh thank you, that's a great catch!

---

_Label `bug` added by @charliermarsh on 2024-01-18 13:39_

---

_Comment by @MichaReiser on 2024-01-18 13:53_

Does the fix also fix dunder calls in binarh expressions like `a * 5.__add__(3)`? If so, we'll need to add parentheses there too to preserve the correct operator precedence

---

_Comment by @RubenVanEldik on 2024-01-18 15:56_

This code:

```py
    a = 1
    2 * a.__add__(3)
```

Does get linted to:

```py
    a = 1
    2 * a + 3
```

So parenthesis are indeed required. However, the parentheses are, of course, not always required. As `2 + a.__mul__(3)` is correctly linted to `2 + a * 3`.



---

_Comment by @RubenVanEldik on 2024-01-18 16:01_

I have no clue how Ruff works under the hood, but if you add the parentheses around everything, they will get removed by another Ruff rule when it's only that expression. Because Ruff lints `a = (2 * b)` to `a = 2 * b`. So adding parentheses will only affect lines with multiple operators

---

_Comment by @MichaReiser on 2024-01-18 16:08_

Thanks @RubenVanEldik Yeah,  we might even need to add parentheses around `b + a.__mul__(c)` if we want to mark the fix as safe for the case where `__mul__` and `__add__` are overridden in a way that doesn't follow normal operator semantics :(

---

_Comment by @diceroll123 on 2024-01-19 06:57_

Fascinating... 🤔 

---

_Comment by @RubenVanEldik on 2024-01-19 07:11_

@MichaReiser, I'm not entirely sure, but I don't think you can override the operator precedence. Something like `b + a.__mul__(c).__add__(d).__mul__(e)` Can always be correctly rewritten by replacing the dunder calls from the left and adding brackets around each dunder call: `b + (((a * c) + d) * e)`. I think the main challenge would be not adding unnecessary parentheses, for example, this example is also correct as `b + (a * c + d) * e.

Not adding unnecessary parentheses would be nice, but not a necessity, I think. Also, because sometimes it's good to add parentheses, even though they are not strictly necessary, just to add clarity to operator precedence. 

---

_Comment by @konstin on 2024-01-19 09:13_

While i agree that this assumption that `b + a.__mul__(c).__add__(d).__mul__(e)` equals `b + (((a * c) + d) * e)` _should_ hold, it unfortunately does not in the general case
```python
class A:
    def __radd__(self, other):
        return other + 1


print(int(1).__add__(A()))
print(int(1) + A())
```
prints
```
NotImplemented
2
```
This is a limitation of the rule in general. The missing parentheses in the OP is of course still a bug and should be fixed.

---

_Comment by @diceroll123 on 2024-01-19 09:28_

It's easy to autofix when the unary is not surrounded by parentheses, since we can just use the operand value as the TextRange start, but autofixing it for something like `-(-a).__sub__(1)` is where my skill issues with Ruff start to show. 🤔 

---

_Comment by @RubenVanEldik on 2024-01-19 12:58_

@konstin, good catch!! It seems that this automatic fix should be under the --unsafe-fixes flag then. Most of the time the Ruff fix should work, but as you showed it could create issues in specific scenarios. 

---

_Closed by @MichaReiser on 2024-03-04 10:25_

---

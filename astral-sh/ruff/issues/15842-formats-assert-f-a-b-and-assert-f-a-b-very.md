```yaml
number: 15842
title: "formats `assert f(a,) == (b)` and `assert f(a,) == b` very differently"
type: issue
state: open
author: rdaysky
labels:
  - formatter
  - style
assignees: []
created_at: 2025-01-31T01:43:56Z
updated_at: 2025-02-04T00:56:16Z
url: https://github.com/astral-sh/ruff/issues/15842
synced_at: 2026-01-12T15:54:55Z
```

# formats `assert f(a,) == (b)` and `assert f(a,) == b` very differently

---

_@rdaysky_

### Description

ruff format transforms
```
assert f(a,) == (b)
assert f(a,) == b
```
into
```
assert f(
    a,
) == (b)
assert (
    f(
        a,
    )
    == b
)
```
which doesn’t make too much sense.

---

_Comment by @dhruvmanila on 2025-01-31 04:11_

Thanks for the report. Can you say why it doesn't make sense?

@MichaReiser will be able to provide more context on why this is the case but he's currently on leave and will be able to reply next week.

Regardless, it matches what Black does https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACpAGFdAD2IimZxl1N_WlPTQ06-Nur1lMjjikbZBUWzXuAQNKhFphrTQvWGzojFvt24KBUXaI3nTaBMACVKkaNvMxbDMiDTeDauQNv4yFlbtT0lJmLmdtWcjJOxsFmoi2ajLXi1CwAAAAAAM4BGDhyTg2EAAX2qAQAAAJAqwvaxxGf7AgAAAAAEWVo%3D

---

_Label `question` added by @dhruvmanila on 2025-01-31 04:11_

---

_Label `formatter` added by @dhruvmanila on 2025-01-31 04:11_

---

_Comment by @rdaysky on 2025-02-03 00:54_

It doesn’t make sense for two reasons:
1. Why should a short expression and the same expression but parenthesized behave differently?
2. Why should outer parentheses appear when the expression can be satisfactorily formatted without them?

---

_Comment by @dhruvmanila on 2025-02-03 03:42_

I think one part of the reason is the trailing comma that's present in the function call arguments. That indicates the formatter to break the expression, even though it might short and under the configured line length, into multiple lines and keep them as multiline expression. You can opt-out of this behavior by setting [`format.skip-magic-trailing-comma`](https://docs.astral.sh/ruff/settings/#format_skip-magic-trailing-comma) to `true`. Refer to the [playground](https://play.ruff.rs/372ce9ec-23fe-42ed-9b23-487eb1b474c2) to checkout this behavior.

---

_Comment by @rdaysky on 2025-02-03 12:23_

But the magic trailing comma is an immensely useful parameter that can’t be turned off without major impact on the rest of the code.

In any case, the magic comma in `assert f(a,) == b` should insert newlines around `a`, not around `b`.

---

_Label `question` removed by @MichaReiser on 2025-02-03 13:19_

---

_Label `style` added by @MichaReiser on 2025-02-03 13:19_

---

_Comment by @MichaReiser on 2025-02-03 13:19_

What you see here is also not specific to `assert`; you can observe the same in any clause header (`if,` `while,` `match` ...) [playground](https://play.ruff.rs/adda069e-1846-41e8-a9f0-ec8b38be85cd)


Specifically, Ruff supports two layouts for clause headers (which are also used for assert conditions):

1. A layout that avoids adding parentheses around the condition and instead breaks after existing parentheses (`[`, `{`, `(`). This is what you see for `assert f(a,) == (b)`. However, this layout is only applied if the parentheses are at the very right (or very start 
2. If the parentheses aren't at the very start (which isn't for calls because the leftmost part is an identifier), then Ruff/Black parenthesizes the entire expression instead. You see this for `f(a,) = b` because it neither starts nor ends with parentheses.


You can also see this if you flip the operands:

```py
assert f(a,) == b
assert b == f(a, )
```

The code for this can be found here:

https://github.com/astral-sh/ruff/blob/f3b918de95f9bea0a9ab42587f09547bf6ffdb0b/crates/ruff_python_formatter/src/expression/mod.rs#L527-L610


The layout 1 exists to avoid unnecessary parentheses in common cases and it obviously don't pick the ideal solution in this case. However, this is also a very trivial example where the parentheses around `b` are unnecessary. Respecting the parentheses is useful because you normally want to keep the parenthesized expressions together

```python
assert f(
    a,
) == (
    aaaaaaaaaaaaaa
    and bbbbbbbbbbbbbb
    and cccccccccccc
    and dddddddddddddddddd
    and eeeeeeeeeeee
)
assert f(
    a,
) == (aaaaaaaaaaaaaa and bbbbbbbbbbbbbb and cccccccccccc and dddddddddddddddddd)
```

I do agree that there's some room for improvement here so I'll keep the issue open but I don't expect to change this anytime soon because changing this behavior is a fairly significant break with Black's style guide and there are other "bad formattings" that I want to improve first.

---

_Comment by @rdaysky on 2025-02-04 00:56_

The same issue in Black: https://github.com/psf/black/issues/2581

---

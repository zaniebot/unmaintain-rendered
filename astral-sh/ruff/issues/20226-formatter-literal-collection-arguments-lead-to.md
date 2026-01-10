```yaml
number: 20226
title: "Formatter: literal collection arguments lead to awful looking GNU-style indents"
type: issue
state: closed
author: xmo-odoo
labels:
  - formatter
  - style
assignees: []
created_at: 2025-09-04T12:13:08Z
updated_at: 2025-09-15T09:07:33Z
url: https://github.com/astral-sh/ruff/issues/20226
synced_at: 2026-01-10T11:09:59Z
```

# Formatter: literal collection arguments lead to awful looking GNU-style indents

---

_Issue opened by @xmo-odoo on 2025-09-04 12:13_

### Summary

If a collection literal long enough to exceed `line-length` is the parameter to a function, ruff does not just line-break inside the collection literal, it moves the collection literal indented inside the function call *then* indents individual items, in something of a [GNU-style indent](https://en.wikipedia.org/wiki/Indentation_style#GNU).
```python
foo(
    [
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
    ]
)
```
This would look a lot better and takes less space by folding the collection delimiters with the function's:
```python
foo([
    'thisisalongvalue',
    'thisisalongvalue',
    'thisisalongvalue',
    'thisisalongvalue',
    'thisisalongvalue',
    'thisisalongvalue',
    'thisisalongvalue',
])
```
https://play.ruff.rs/38c1f00a-1f1b-42d8-a0c5-83eb8c722295

### Version

0.12.11

---

_Label `formatter` added by @ntBre on 2025-09-04 12:52_

---

_Label `style` added by @ntBre on 2025-09-04 12:52_

---

_Comment by @ferdnyc on 2025-09-08 06:42_

@xmo-odoo 

...Not sure how I feel about that.

It would involve some special-casing, because consider the reformatting of this:

```python
foo('thisisalongargumenttothefunction', 'thisisashorterargument',
    [
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
    ]
)
```

Ruff will make that:

```python
foo(
    "thisisalongargumenttothefunction",
    "thisisashorterargument",
    [
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
    ],
)
```

As, I'd argue, it should. Which means the "gnu-style indent" is actually the product of two Ruff reformattings happening together: The rule that chops function arguments down when they don't fit on a single line creates the line break after the `(`, and before the `)`. The rule for reformatting long collections creates the breaks after `[` and before `]`.

So now we have a situation where the formatting (as you propose it) would _differ_ based on whether the collection is the **only** argument to the function or not? Presumably, overriding the chop-down-long-argument-lists rule, and applying only the reformatting for collections (just like it would be applied for an assignment, e.g.:)

```python
# post-ruff formatting
bar = [
    "thisisalongvalue",
    "thisisalongvalue",
    "thisisalongvalue",
    "thisisalongvalue",
    "thisisalongvalue",
    "thisisalongvalue",
    "thisisalongvalue",
]
```

(Granted, there's already some special-casing. As I showed above, Ruff adds a trailing comma after the collection _only_ if it's NOT the only argument. But comma-vs-no-comma is a less severe divergence between cases than the proposed special-casing would be.)

---

_Comment by @ntBre on 2025-09-08 14:09_

I'll be curious to hear what @MichaReiser thinks when he gets back from PTO next week. A special case for this would at least be in line with rustfmt, as one [example](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=bde59efe73022fce11cbbcce584aefea). It looks like our current formatting matches `black`, though.

---

_Comment by @ferdnyc on 2025-09-08 21:47_

`rustfmt` turns out to be [surprisingly configurable](https://rust-lang.github.io/rustfmt/?version=v1.8.0) and un-Black-like. (Well, it was surprising to me.) There are some interesting knobs in there, too.

But, there doesn't seem to be **any** configuration that will convince it to output this:

```rust
foo(
    [
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
        "thisisalongvalue",
    ]
);
```

So, that's a point in favor.


---

_Comment by @MichaReiser on 2025-09-15 09:07_

The requested style is supported in preview mode (`format.preview = true`) if the nested list (or any other syntax that has its own parentheses) is the only call argument. We added this style in https://github.com/astral-sh/ruff/issues/8279, but it hasn't been stabilized yet because it would break black compatibility. We'll revisit this decision as part of Ruff's 2026 style guide.

---

_Closed by @MichaReiser on 2025-09-15 09:07_

---

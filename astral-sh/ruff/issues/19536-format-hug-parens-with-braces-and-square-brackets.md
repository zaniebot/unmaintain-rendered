```yaml
number: 19536
title: "format: `hug_parens_with_braces_and_square_brackets` preview style does not affect item access"
type: issue
state: open
author: codethief
labels:
  - formatter
  - preview
  - style
assignees: []
created_at: 2025-07-24T18:45:08Z
updated_at: 2025-07-25T06:42:53Z
url: https://github.com/astral-sh/ruff/issues/19536
synced_at: 2026-01-10T11:09:59Z
```

# format: `hug_parens_with_braces_and_square_brackets` preview style does not affect item access

---

_Issue opened by @codethief on 2025-07-24 18:45_

### Summary

The following snippet,

```
obj_with_getitem[{
    "a": "asdf",
    "b": "asdfsdf",
}]
```

gets formatted as

```python
obj_with_getitem[
    {
        "a": "asdf",
        "b": "asdfsdf",
    }
]
```

Instead, I would expect the code to remain unchanged when preview style is enabled, due to [`hug_parens_with_braces_and_square_brackets`](https://github.com/astral-sh/ruff/issues/8279).

I ran into this when using the (admittedly still experimental) [PEP-764-style inline typed dictionaries](https://peps.python.org/pep-0764/),

```python
type MyType = TypedDict[{
    "foo": str,
    "bar": int,
    "baz": TypedDict[{
        "a": str,
        "b": int,
        "c": int,
    }],
}]
```

which look a lot worse (don't match a corresponding dictionary's indentation structure) when the brackets don't hug the braces:

```python
type MyType = TypedDict[
    {
        "foo": str,
        "bar": int,
        "baz": TypedDict[
            {
                "a": str,
                "b": int,
                "c": int,
            }
        ],
    }
]
```


Irrespective of the outcome of PEP 764, though, I think `hug_parens_with_braces_and_square_brackets` should apply to item access, too.

Playground: https://play.ruff.rs/46d31ae7-8a31-4729-8359-7c73d634fcac?secondary=Format


### Version

ruff 0.11.10

---

_Renamed from "format: hug_parens_with_braces_and_square_brackets preview style does not affect item access" to "format: `hug_parens_with_braces_and_square_brackets` preview style does not affect item access" by @codethief on 2025-07-24 19:20_

---

_Comment by @codethief on 2025-07-24 21:29_

FWIW, in black the `hug_parens_with_braces_and_square_brackets` style also affects item access, see https://github.com/psf/black/issues/4715 .


---

_Label `formatter` added by @MichaReiser on 2025-07-25 06:42_

---

_Label `preview` added by @MichaReiser on 2025-07-25 06:42_

---

_Label `style` added by @MichaReiser on 2025-07-25 06:42_

---

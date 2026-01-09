---
number: 8330
title: "Formatter: method call chain with comment, differs from black output"
type: issue
state: closed
author: robmoss
labels:
  - formatter
assignees: []
created_at: 2023-10-29T23:23:52Z
updated_at: 2023-11-15T23:17:04Z
url: https://github.com/astral-sh/ruff/issues/8330
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: method call chain with comment, differs from black output

---

_Issue opened by @robmoss on 2023-10-29 23:23_

Black 23.10.1 and Ruff 0.1.3 produced slightly different outputs for a method call chain that includes a comment, and I couldn't find a relevant issue in the issue tracker. I've reduced to it to a much simpler example where both tools produce identical formatting for `x = ...` but different formatting for `y = ...` — black doesn't insert a newline before the closing `)` and `.f()`.

It isn't clear to me which tool is producing the "best" result. I find black's decision here slightly odd, and if I add a second method call after the `# Comment goes here: black and ruff differ` line, black **does** insert a newline before `.f()`. So perhaps I should file this is an issue with black?

```py
x = (
    very_long_function_name_with_many_arguments(100, 200, 300, 400, 500, 600, 700, 800, 900, 1000).f().g()
    # Comment goes here: no difference
    .h()
)

y = (
    very_long_function_name_with_many_arguments(100, 200, 300, 400, 500, 600, 700, 800, 900, 1000).f()
    # Comment goes here: black and ruff differ
    .h()
)
```

## Black output

```py
x = (
    very_long_function_name_with_many_arguments(
        100, 200, 300, 400, 500, 600, 700, 800, 900, 1000
    )
    .f()
    .g()
    # Comment goes here: no difference
    .h()
)

y = (
    very_long_function_name_with_many_arguments(
        100, 200, 300, 400, 500, 600, 700, 800, 900, 1000
    ).f()
    # Comment goes here: black and ruff differ
    .h()
)
```

## Ruff output

```py
x = (
    very_long_function_name_with_many_arguments(
        100, 200, 300, 400, 500, 600, 700, 800, 900, 1000
    )
    .f()
    .g()
    # Comment goes here: no difference
    .h()
)

y = (
    very_long_function_name_with_many_arguments(
        100, 200, 300, 400, 500, 600, 700, 800, 900, 1000
    )
    .f()
    # Comment goes here: black and ruff differ
    .h()
)
```


---

_Label `formatter` added by @MichaReiser on 2023-10-30 00:16_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-30 00:16_

---

_Comment by @MichaReiser on 2023-10-30 00:17_

Interesting, thank's for reporting. 

To me, ruff's formatting seems more consistent and produces fewer changes when inserting/removing `.g()` (a method call) from the call chain, which is what both Black and Ruff optimize for.

I'm not quite sure why black avoids inserting the newline when omitting `g`, and it could be a bug on their side. But it's also likely that there is reason for the deviation that just isn't obvious from this example.  It might be worth filing an issue on their side.

---

_Referenced in [psf/black#3998](../../psf/black/issues/3998.md) on 2023-10-30 02:59_

---

_Comment by @robmoss on 2023-11-15 22:29_

There have been quite a few similar issues filed with Black recently, and they've all been given the label `T:bug`. So I think it's fair to say that ruff's formatting isn't an issue here.

---

_Closed by @robmoss on 2023-11-15 22:29_

---

_Comment by @charliermarsh on 2023-11-15 23:17_

@robmoss - Thanks for following up!

---

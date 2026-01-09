---
number: 21271
title: "`multi-line-implicit-string-concatenation (ISC002)` should use parens."
type: issue
state: closed
author: css-optivoy
labels:
  - question
assignees: []
created_at: 2025-11-04T11:10:56Z
updated_at: 2025-11-10T09:13:31Z
url: https://github.com/astral-sh/ruff/issues/21271
synced_at: 2026-01-07T13:12:16-06:00
---

# `multi-line-implicit-string-concatenation (ISC002)` should use parens.

---

_Issue opened by @css-optivoy on 2025-11-04 11:10_

### Summary

[ISC002](https://docs.astral.sh/ruff/rules/multi-line-implicit-string-concatenation/#example) states:

> For string literals that wrap across multiple lines, [PEP 8](https://peps.python.org/pep-0008/#maximum-line-length) recommends the use of **implicit string concatenation within parentheses** instead of using a backslash for line continuation, as the former is more readable than the latter.

And the example on the page shows wrapping the string in parens.

However, what the formatter does in practice seems to be the opposite where running `ruff format` on:

```python
print(
    "Eggs",
    "Bacon",
    "Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam " \
    "Spam Spam Spam Spam",
)
```

will produce:

```python
print(
    "Eggs",
    "Bacon",
    "Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam "
    "Spam Spam Spam Spam",
)
```

While syntactically valid, this seems counter to the stated function of this check, and the result (I would argue), is indeed _less_ readable than the original.

I would expect this check to always wrap multi-line implicit string concatenation within parentheses, as stated.
Ideally even ones that do not use backslash.

At the very least I feel this should be a configurable option.


Thank you.

---

_Label `question` added by @MichaReiser on 2025-11-04 13:08_

---

_Comment by @MichaReiser on 2025-11-04 13:13_

`ruff format` is independent of the `ISC` rules and it is unopinionated on whether you format your multiline implicit concatenated string in parentheses (or not)


```py
print(
    "Eggs",
    "Bacon",
    "Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam " \
    "Spam Spam Spam Spam",
)
```

gets formatted as

```py
print(
    "Eggs",
    "Bacon",
    "Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam "
    "Spam Spam Spam Spam",
)
```


and 

```py
print(
    "Eggs",
    "Bacon",
    ("Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam "
    "Spam Spam Spam Spam"),
)
```

is formatted to 

```
print(
    "Eggs",
    "Bacon",
    (
        "Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam Spam "
        "Spam Spam Spam Spam"
    ),
)
```

There are two reasons for this:

* The formatter tries to avoid introducing new parentheses in expression positions. There's an argument that the formatter should add an extra level of indent on the second line, to make it visually clear that the string "continues" on the next line
* A formatter is a tricky balance between being opinionated and avoiding being too opinionated. Being too opinionated makes the formatter less appealing to users who don't agree with the formatter style. That's what ISC002 is for. It gives a way to enforce a more opinionated formatting for the people who explicitly prefer one formatting over the other

---

_Comment by @css-optivoy on 2025-11-05 15:29_

Ah, right. This is not the first time I mix up the formatter and the checker.
I tend to run the checker with --fix first and then the formatter, which obscures exactly which does what.
Thank you for clarifying.

I guess what I'm really after is for the checker to be able to fix `ISC002` so that I can run something like:
```bash
ruff check ./str_concat_multiline.py --select ISC002 --fix --unsafe-fixes --fixable ISC002
```

---

_Closed by @MichaReiser on 2025-11-07 14:37_

---

_Comment by @css-optivoy on 2025-11-10 09:10_

@MichaReiser Just to clarify, does closed as 'completed' mean that the requested new fix was added?
I had a look at the recent commit history but didn't see anything, but perhaps I'm looking in the wrong place.

---

_Comment by @MichaReiser on 2025-11-10 09:13_

No, we didn't make any changes but we'd accept a PR adding a fix for ISC002.

I marked it as completed because I got the impression that I answered your question. 


---

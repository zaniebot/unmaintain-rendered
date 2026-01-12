```yaml
number: 6384
title: "Parenthesizing starred `yield` inner-expression causes a syntax error"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-08-07T08:56:12Z
updated_at: 2023-08-16T13:41:09Z
url: https://github.com/astral-sh/ruff/issues/6384
synced_at: 2026-01-12T15:54:46Z
```

# Parenthesizing starred `yield` inner-expression causes a syntax error

---

_@MichaReiser_

```python
def test():
    (
        yield 
        #comment 1
        * # comment 2
        # comment 3
        test # comment 4
    )
```

Get's formatted as 

```python
def test():
    (
        yield (
            # comment 1
            # comment 2
            *# comment 3
            test
        )  # comment 4
    )
```

This is invalid syntax because the `*` needs to be in front of the parenthesized expression.

Open question: Should yield be calling into `needs_parentheses` and parenthesize the expression if it is always (and otherwise never add parentheses) instead of using `maybe_parenthesize_expression` (which also implies a different breaking logic than parenthesizing the expression directly).

Unrelated Nit: The `comment 3` placement is a bit strange

---

_Label `bug` added by @MichaReiser on 2023-08-07 08:56_

---

_Label `formatter` added by @MichaReiser on 2023-08-07 08:56_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-07 08:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-13 16:03_

---

_Comment by @charliermarsh on 2023-08-15 00:00_

The input code here isn't valid syntax either. You can't have a starred expression after a `yield`. Do you recall where this came up?

---

_Comment by @MichaReiser on 2023-08-15 07:13_

> The input code here isn't valid syntax either. You can't have a starred expression after a `yield`. Do you recall where this came up?

Ruff and black parse this just fine

```python
def test():
    (
        yield 
        #comment 1
        * # comment 2
        # comment 3
        test # comment 4
    )
```
But I get a parse error in Python, and I must admit I don't know why. The full grammar specification tells me that I can have a `*` expression after the `yield` keyword and the spec doesn't mention that trivia is forbidden. 

```
yield_expr:
    | 'yield' 'from' expression 
    | 'yield' [star_expressions] 

star_expressions:
    | star_expression (',' star_expression )+ [','] 
    | star_expression ',' 
    | star_expression
```

Does Python not support `yield *`?

---

_Comment by @charliermarsh on 2023-08-15 16:03_

It's allowed if the right-hand side is a tuple, like `yield 1, *x` or `yield *x, 1`, but I think the target itself cannot be a starred expression. I can't find this defined formally anywhere, I just spent a while looking for it. This is also true of assignments (`x = *y` is invalid, `x = *y, 1` is valid) and other statements.

---

_Comment by @charliermarsh on 2023-08-15 16:05_

Given:

```python
def test():
    (
        yield 
        #comment 1
        * # comment 2
        # comment 3
        test, # comment 4
        1
    )
```

We produce:

```python
def test():
    (
        yield (
            # comment 1
            (
                # comment 2
                *# comment 3
                test,  # comment 4
                1,
            )
        )
    )
```

Which is valid syntax with the same AST, though the placement of `# comment 3` is odd.

---

_Comment by @charliermarsh on 2023-08-15 16:06_

For completeness, Black formats that as:

```python
def test():
    (
        yield
        # comment 1
        *  # comment 2
        # comment 3
        test,  # comment 4
        1,
    )
```

---

_Comment by @charliermarsh on 2023-08-15 16:06_

I can work on improving the comments.

---

_Comment by @charliermarsh on 2023-08-15 16:43_

I guess the way to think of it is that with `yield *starred`, there's nothing to unpack `*starred` into, i.e., no containing tuple into which the elements should be unpacked?

---

_Comment by @MichaReiser on 2023-08-15 17:00_

I don't know. I assumed they would work like JavaScript's `yield *generator` where it makes the function to return every item of `generator` before continuing.

---

_Comment by @charliermarsh on 2023-08-15 17:03_

I think that would be `yield from generator` in Python.

---

_Closed by @charliermarsh on 2023-08-16 13:41_

---

```yaml
number: 7004
title: "Formatter: Trailing end of line comments in binary like expressions"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-30T08:48:23Z
updated_at: 2023-09-12T06:39:58Z
url: https://github.com/astral-sh/ruff/issues/7004
synced_at: 2026-01-10T11:09:49Z
```

# Formatter: Trailing end of line comments in binary like expressions

---

_Issue opened by @MichaReiser on 2023-08-30 08:48_

```python
# Input
if (
    (1 + 2) or # test
    (3 + 4) or # other
    (4 + 5) # more
): pass


if (
    (1 and 2) + # test
    (3 and 4) + # other
    (4 and 5) # more
): pass


if (
    (1 + 2) < # test
    (3 + 4) > # other
    (4 + 5) # more
): pass

# Output
if (
    (
        1 + 2  # test
    )
    or (
        3 + 4  # other
    )
    or (4 + 5)  # more
):
    pass


if (
    (
        1 and 2  # test
    )
    + (3 and 4)  # other
    + (4 and 5)  # more
):
    pass


if (
    (
        1 + 2  # test
    )
    < (
        3 + 4  # other
    )
    > (4 + 5)  # more
):
    pass
```


**Expected**
Ruff not to expand the parenthesized right sides. 

> *Note*: Prettier handles [this](https://prettier.io/playground/#N4Igxg9gdgLgprEAuEBLAZgAgBQB0qaE4CMmAZGZgEwCUmA1JgPRObwDOM+ROAzOZQAsdRi0wQYACzgAnbkWyCBmAKx0xAWwgy4+GsAC++EABoQEAA4xU0dslABDGTIgB3AApOEdlA4A2rg4AnnZmAEYyDmAA1nAwAMoOGnAAMqhQcMjo-uxw4ZExcfEWUekA5sgwMgCueSC5GqiVNXXs5X5wAIrVEplI2X65ZgBW7AAe8e1dPfBZOXUAjjNw7i4WPiAO7AC0GXAAJgemIFUOqH7lAMIQGhoOyJt+fsdtUGUdAIIwVahh1fDuWRpDJzQZ1SQwDR+ADqklQHBKYDg8W88NQADd4UEHmB2KEQOjagBJKCHWDxMAyVBWD6k+IwIIdUFDEAWFy5aGRCwPNlwXIydGZMzpfkwVYOMp3Zl1EoyfkPMIOMJwZ5mNnpGDQ1D7KTIAAcAAYzDolqgdOLJfd+vMzDAlVqdZJkFQzNVcgAVJU+AYsuAaZX7Q77FION7VCVwABi2ju33KDwc-wgIAMBiAA) correctly

---

_Label `formatter` added by @MichaReiser on 2023-08-30 08:48_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-30 08:48_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-08 07:14_

---

_Comment by @MichaReiser on 2023-09-08 13:59_

Uff this is harder than I expected. The issue is that our comment logic doesn't distinguish between

```python
(
  a # comment
)

(a) # comment
```

It always formats the comment inside of the parentheses instead of only doing so if the comment is inside the parentheses  in the source. I thought I have a clever quick fix that manually moves the trailing comments out inside of `FormatExpr` if it is passed a `)` but I now run into stability issues with function return type annotations 🙅 

Another example:

```python
(
    a + 
    # comment 
    (a + b) 
     > c
)
```

Gets formatted as 

```python
(
    a + 
    (
	    # comment 
		a + b
	) 
     > c
)
```


---

_Closed by @MichaReiser on 2023-09-12 06:39_

---

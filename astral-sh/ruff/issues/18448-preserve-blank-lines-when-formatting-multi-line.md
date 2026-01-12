```yaml
number: 18448
title: Preserve blank lines when formatting multi-line lists, function arguments, etc.
type: issue
state: open
author: mabeyj
labels:
  - formatter
  - style
assignees: []
created_at: 2025-06-03T20:25:41Z
updated_at: 2025-06-04T12:50:31Z
url: https://github.com/astral-sh/ruff/issues/18448
synced_at: 2026-01-12T15:54:56Z
```

# Preserve blank lines when formatting multi-line lists, function arguments, etc.

---

_@mabeyj_

Requesting the ability for `ruff format` to preserve blank lines within multi-line lists, dictionaries, sets, tuples, function arguments, and any other list-like syntax. Collapsing multiple blank lines into a single blank line would make sense here and would be consistent with blank lines being collapsed elsewhere.

Currently, it strips out all blank lines in these cases:

```python
### INPUT

func(
    "first",

    # This could be a long comment
    # documenting something important
    "arg",
    "arg",

    # This could be a long comment
    # for which the blank line implies it's not talking about the last argument
    "arg",
    "arg",

    "last",
)

items = [
    "first",

    # Apples
    "item",
    "item",

    # Oranges
    "item",

    "last",
]

@params(
    ('case'),

    # Really long list of test cases that might be documented
    ('case'),

    # Comment
    ('case'),
)
def test_func(value):
    pass

### OUTPUT

func(
    "first",
    # This could be a long comment
    # documenting something important
    "arg",
    "arg",
    # This could be a long comment
    # for which the blank line implies it's not talking about the last argument
    "arg",
    "arg",
    "last",
)

items = [
    "first",
    # Apples
    "item",
    "item",
    # Oranges
    "item",
    "last",
]

@params(
    ("case"),
    # Really long list of test cases that might be documented
    ("case"),
    # Comment
    ("case"),
)
def test_func(value):
    pass
```

Similar to why we would add blank lines anywhere else, blank lines can improve the readability of long lists. Especially if there are comments within the list; a blank line should be allowed around comment lines for readability.

For comparison, YAPF and Prettier do not strip blank lines here.

---

_Label `formatter` added by @ntBre on 2025-06-03 20:31_

---

_Label `style` added by @MichaReiser on 2025-06-04 05:11_

---

_Comment by @MichaReiser on 2025-06-04 06:24_

I'm supportive of this change. It's something that I found very surprising coming from Prettier but I've to do some digging if there's a specific reason that Black disallows it. 

IMO, it's an instance where the formatter is too opinionated today, to the extend where it is harmful. 

---

_Comment by @MichaReiser on 2025-06-04 06:26_

Related issues in the black repository: 

* https://github.com/psf/black/issues/2805
* https://github.com/psf/black/issues/2723



---

_Comment by @mabeyj on 2025-06-04 12:50_

> IMO, it's an instance where the formatter is too opinionated today, to the extend where it is harmful.

100% agreed! So much so, that I had forked Black to make this change because it was very disruptive for a project and just inconsistent when used alongside Prettier:

* https://github.com/mabeyj/black-ex/pull/3
* https://github.com/jsh9/cercis/pull/26

---

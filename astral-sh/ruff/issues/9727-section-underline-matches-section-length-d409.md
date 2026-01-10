---
number: 9727
title: "`section-underline-matches-section-length` (`D409`) should warn on all titles instead of a list of known ones"
type: issue
state: open
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2024-01-31T00:44:35Z
updated_at: 2024-02-01T12:39:11Z
url: https://github.com/astral-sh/ruff/issues/9727
synced_at: 2026-01-10T01:22:49Z
---

# `section-underline-matches-section-length` (`D409`) should warn on all titles instead of a list of known ones

---

_Issue opened by @DetachHead on 2024-01-31 00:44_

it seems that this rule only cares about specific titles ("Example", "Parameters", "Raises", "Returns") but i don't see why it can't just check every title

```py
def foo(): # error
    """
    example:
    -----------
    asdf
    """

def bar(): # no error
    """
    asdf:
    --------
    asdf
    """
```


https://play.ruff.rs/a10dd179-6047-43c4-97b1-5083e68008a1

---

_Label `rule` added by @AlexWaygood on 2024-02-01 12:39_

---

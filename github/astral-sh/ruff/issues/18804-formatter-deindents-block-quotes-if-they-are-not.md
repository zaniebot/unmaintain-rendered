---
number: 18804
title: Formatter deindents block quotes if they are not followed by a second paragraph.
type: issue
state: open
author: randolf-scholz
labels:
  - formatter
  - style
assignees: []
created_at: 2025-06-19T20:26:23Z
updated_at: 2025-06-28T06:13:22Z
url: https://github.com/astral-sh/ruff/issues/18804
synced_at: 2026-01-07T13:12:16-06:00
---

# Formatter deindents block quotes if they are not followed by a second paragraph.

---

_Issue opened by @randolf-scholz on 2025-06-19 20:26_

### Summary

```python
def foo():
    """This is an ordinary paragraph, introducing a block quote.

        "It is my business to know things.  That is my trade."

        -- Sherlock Holmes
    """

def bar():
    """This is an ordinary paragraph, introducing a block quote.

        "It is my business to know things.  That is my trade."

        -- Sherlock Holmes

    Another ordinary paragrph.
    """
```

Gets formatted to

```python
def foo():
    """This is an ordinary paragraph, introducing a block quote.

    "It is my business to know things.  That is my trade."

    -- Sherlock Holmes
    """


def bar():
    """This is an ordinary paragraph, introducing a block quote.

        "It is my business to know things.  That is my trade."

        -- Sherlock Holmes

    Another ordinary paragrph.
    """
```

See: https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#block-quotes






### Version

https://play.ruff.rs/5b2fedac-691c-4adf-9683-68f33e0e90df

---

_Renamed from "[format] Formatter deindents block quotes if they are not followed by a second paragraph." to "[BUG] Formatter deindents block quotes if they are not followed by a second paragraph." by @randolf-scholz on 2025-06-19 20:28_

---

_Comment by @MichaReiser on 2025-06-19 20:51_

I think the request here is to support restructured text in docstrings. There are similar requests around preserving other whitespace in docstrings but they all require understanding the docstring semantics

---

_Label `formatter` added by @MichaReiser on 2025-06-19 20:51_

---

_Label `style` added by @MichaReiser on 2025-06-19 20:51_

---

_Comment by @randolf-scholz on 2025-06-19 20:57_

A start would be to at least have consistent formatting. The fact that it doesn't deindent the block-quote inside `bar` but does inside `foo` seems like an internal inconsistency to me.

---

_Comment by @MichaReiser on 2025-06-19 21:15_

> A start would be to at least have consistent formatting. The fact that it doesn't deindent the block-quote inside bar but does inside foo seems like an internal inconsistency to me.

The formatter tries to normalize the indent. The way this is done is by detecting the minimal indent shared by all lines and using that as the base. This currently ignores the indent of the trailing quotes (or any line that's all whitespace without content). Whether this should be changed is something we can consider but I'm sure there are other cases where it would be desired to keep the current behavior.

---

_Renamed from "[BUG] Formatter deindents block quotes if they are not followed by a second paragraph." to "Formatter deindents block quotes if they are not followed by a second paragraph." by @MichaReiser on 2025-06-28 06:13_

---

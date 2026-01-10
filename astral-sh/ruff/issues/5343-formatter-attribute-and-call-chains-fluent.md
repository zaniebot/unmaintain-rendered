```yaml
number: 5343
title: "Formatter: Attribute and call chains \"fluent interface\""
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-06-23T16:52:04Z
updated_at: 2024-03-08T11:42:13Z
url: https://github.com/astral-sh/ruff/issues/5343
synced_at: 2026-01-10T11:09:47Z
```

# Formatter: Attribute and call chains "fluent interface"

---

_Issue opened by @konstin on 2023-06-23 16:52_

Black supports attribute access and call chaining to provide good formatting for [fluent interface](https://en.wikipedia.org/wiki/Fluent_interface) style programming with extensive method chaining which is common e.g. with django (https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#call-chains):

```python
def example(session):
    result = (
        session.query(models.Customer.id)
        .filter(
            models.Customer.account_id == account_id,
            models.Customer.email == email_address,
        )
        .order_by(models.Customer.id.asc())
        .all()
    )
```

https://github.com/astral-sh/ruff/pull/5340 implements a part of this badly to fix a bug and https://github.com/astral-sh/ruff/pull/5341 adds basic function call formatting.

We need to implement a formatting where the outermost call adds the optional parentheses and formats all inner accesses and calls to not have them add parentheses. A complication is that unlike for if-statements the tree is left recursive, meaning that for `a.b().c.d.e` the outermost call see an access of `e` on `a.b().c.d`, while we must format first `a`, then `.b()`, `.d` and finally `.e`. We can implement by either collecting into a stack (needs allocation) or using recursing (we most not overrun the recursion and stack limits).

---

_Assigned to @MichaReiser by @konstin on 2023-06-23 16:52_

---

_Assigned to @konstin by @konstin on 2023-06-23 16:52_

---

_Label `formatter` added by @konstin on 2023-06-23 16:52_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 16:10_

---

_Closed by @konstin on 2023-08-04 13:58_

---

_Comment by @olejorgenb on 2024-03-08 10:54_

Probably wrong place to comment on this, but I don't think black's approach here is sufficient. It only work when the chain is very long, and for fluent definitions you usually want one call per line regardless of the number of calls...

Personally I think a OK compromise would be to use explicit parenthesis around a call-chain as a marker that it should be formatted with one line per call?

---

_Comment by @MichaReiser on 2024-03-08 11:42_

Hi @olejorgenb There's an ongoing discussion about this in https://github.com/astral-sh/ruff/issues/8598

---

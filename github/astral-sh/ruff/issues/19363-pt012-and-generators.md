---
number: 19363
title: "PT012: and generators"
type: issue
state: open
author: spaceone
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-15T16:39:50Z
updated_at: 2025-07-15T16:53:34Z
url: https://github.com/astral-sh/ruff/issues/19363
synced_at: 2026-01-07T13:12:16-06:00
---

# PT012: and generators

---

_Issue opened by @spaceone on 2025-07-15 16:39_

I wonder whether PT012 is valid for generators, as one need to resolve them.
in a regular generator i can put `list()` or `next()` around it and for async, `anext` works or putting it into a list comprehension.

```python
@pytest.mark.asyncio
async def test_foo():
    with pytest.raises(Bar):  # W: `pytest.raises()` block should contain a single simple statement
        async for entry in foo():
            assert not entry

```

So you might consider it invalid, but please consider if a exception makes sense here.

---

_Label `rule` added by @ntBre on 2025-07-15 16:53_

---

_Label `needs-decision` added by @ntBre on 2025-07-15 16:53_

---

---
number: 16528
title: Detect not executed iterators
type: issue
state: open
author: prauscher
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-03-06T07:19:27Z
updated_at: 2025-03-06T07:23:31Z
url: https://github.com/astral-sh/ruff/issues/16528
synced_at: 2026-01-10T01:22:57Z
---

# Detect not executed iterators

---

_Issue opened by @prauscher on 2025-03-06 07:19_

### Summary

Imagine a code like this:
```python

values: dict[tuple[int, str]] = {"thirteen": (13, "Lorem"), "twentytwo": (22, "Ipsum"), "two": (2, "Dolor")}

def cleanup_dict(timeout: int) -> Iterator[str]:
    items = list((key, value) for key, (expiration, value) in values.item() if expiration < timeout)
    for key, value in items:
        yield key, value
        del values[key]

# does nothing
cleanup_dict(20)

# only runs partial
next(cleanup_dict(20))

# runs the iterator
deque(cleanup_dict(20), 0)
```

It might be a bad design to mix action and iterator, but what about a rule detecting iterators not executed? A Bonus point would be to detect a not fully executed iterator too. While it might be obvious in this example, given a few bad names and splitting the code over separate files would probably already obfuscate the problem quite well.

So my suggestion would be a rule which triggers on the first call ("does nothing"), but not on the third call (with deque). Whether the rule should also trigger on the second call is up to debate, maybe this could be split in a separate rule.

---

_Comment by @MichaReiser on 2025-03-06 07:23_

Detecting iterators that aren't used at all seems reasonable to me. I don't think we can warn about only partially used iterators because that's a very valid use case. 

Either way, I think we should wait for better type inference to make this rule useful.

---

_Label `rule` added by @MichaReiser on 2025-03-06 07:23_

---

_Label `type-inference` added by @MichaReiser on 2025-03-06 07:23_

---

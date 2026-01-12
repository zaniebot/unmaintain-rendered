```yaml
number: 10505
title: Add new rule to prevent nested list footgun
type: issue
state: open
author: qexat
labels:
  - rule
assignees: []
created_at: 2024-03-21T09:03:19Z
updated_at: 2024-11-10T23:23:25Z
url: https://github.com/astral-sh/ruff/issues/10505
synced_at: 2026-01-12T15:54:50Z
```

# Add new rule to prevent nested list footgun

---

_@qexat_

Hello.

I don't really work with nested lists but I know that it's quite common among the sci community (in fact I'm opening this issue after seeing [this tweet](https://twitter.com/francoisfleuret/status/1770528106513600636)).

I dug into the tracker to see if it was suggested before, but I could not find anything. I forgot to record what I tried, though.

Anyway, here is an excerpt of the issue:

```py
# It might not be intuitive that the lists inside are actually the same
# It is very likely not to be intended, and a footgun all the time
foo: list[list[int]] = [[]] * 4

foo[0].append(42)

print(foo)  # [[42], [42], [42], [42]]
```

It's quite silly and experienced Python programmers *usually* don't fall for it, but people for whom coding is not their main activity and/or beginners can and do.

This rule could also be extended to passing a list literal as the second argument of `dict.fromkeys`, although this might be considered out of this issue's scope.
(+ I don't know by heart all the places where this "lists that actually share the same memory" footgun appears, but feel free to list some more.)

---

_Label `rule` added by @MichaReiser on 2024-03-21 09:52_

---

_Comment by @hikaru-kajita on 2024-03-21 10:50_

Happens for all kinds of mutable, so we could add rules for those included:
```python
foo = [set()] * 4
foo[0].add(42)
print(foo)  # [{42}, {42}, {42}, {42}]

bar = [{}] * 4
bar[0]["a"] = 42
print(bar)  # [{'a': 42}, {'a': 42}, {'a': 42}, {'a': 42}]

---

_Comment by @qexat on 2024-03-21 16:38_

Dang it, I missed this discussion. I really am terrible at searching up.
But yeah, of course, any mutable non-scalar object should be included.

---

_Comment by @pankdm on 2024-11-10 23:23_

I think it might be pretty hard to detect really dangerous examples of that with high precision. Looking at https://grep.app/search?q=%5B%5B%5D%5D%20%2A&case=true&filter[lang][0]=Python there are plenty of usages, but they all look fine.
Someone needs to do
```
foo[0].append(bar)
```
to make it a real footgun.

---

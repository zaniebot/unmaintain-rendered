```yaml
number: 2526
title: "Literal[True] overload resolution fails in argument position"
type: issue
state: open
author: PeterKhoudary
labels: []
assignees: []
created_at: 2026-01-16T01:02:02Z
updated_at: 2026-01-16T01:18:46Z
url: https://github.com/astral-sh/ty/issues/2526
synced_at: 2026-01-16T02:03:52Z
```

# Literal[True] overload resolution fails in argument position

---

_@PeterKhoudary_

### Summary

While attempting to integrate with SQLAlchemy, ran into an issue with matched overloads that I've been able to minimally replicate [here](https://play.ty.dev/e1219b80-9877-4c8f-a8df-7ce8bd843108). TLDR ty is failing to resolve an overload on __init__ when the class is being constructed as an argument to some consumer, but the overload works when constructing standalone. Here's the raw file if playground isn't working.
[ty-literal-true-overload.py](https://github.com/user-attachments/files/24657002/ty-literal-true-overload.py)

I have zero custom configurations on ty, SQLAlchemy version is 2.0.45. Can confirms this works on Pyright, and unsure if this related to existing issues on bidrectional inference. Apologies if so!

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---

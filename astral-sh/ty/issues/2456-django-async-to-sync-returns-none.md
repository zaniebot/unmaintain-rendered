```yaml
number: 2456
title: Django async_to_sync returns None
type: issue
state: open
author: andre-dasilva
labels: []
assignees: []
created_at: 2026-01-12T08:29:25Z
updated_at: 2026-01-12T08:29:25Z
url: https://github.com/astral-sh/ty/issues/2456
synced_at: 2026-01-12T15:54:26Z
```

# Django async_to_sync returns None

---

_@andre-dasilva_

### Summary

First of all thank you for making ty. i would love to use it as a replacement or addition for mypy in the future 😄.

I tried to use ty in my django app. 

Since django is not yet fully async compatiable i am using the asgiref [async_to_sync](https://docs.djangoproject.com/en/6.0/topics/async/#asgiref.sync.async_to_sync) helper function, for parts of my application.

ty says, that every function with async_to_sync could return `None`:

<img width="511" height="210" alt="Image" src="https://github.com/user-attachments/assets/911d3cbb-4c53-4189-882a-c0e913e35caa" />

if i look at the implementation:

<img width="721" height="545" alt="Image" src="https://github.com/user-attachments/assets/c3825cf5-a308-4c59-ba7d-cd7a47f42dfb" />

i dont see that `None` is returned anywhere? is this a bug in uv? or does this come from asgiref?


### Version

0.0.11

---

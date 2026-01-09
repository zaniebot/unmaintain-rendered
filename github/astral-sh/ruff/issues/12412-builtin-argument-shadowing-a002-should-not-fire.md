---
number: 12412
title: "`builtin-argument-shadowing` (`A002`) should not fire when overriding"
type: issue
state: closed
author: tylerlaprade
labels:
  - bug
assignees: []
created_at: 2024-07-19T20:37:40Z
updated_at: 2024-07-20T01:32:34Z
url: https://github.com/astral-sh/ruff/issues/12412
synced_at: 2026-01-07T13:12:15-06:00
---

# `builtin-argument-shadowing` (`A002`) should not fire when overriding

---

_Issue opened by @tylerlaprade on 2024-07-19 20:37_

I have a custom API request factory that inherits from Django Rest Framework's `APIRequestFactory`. I am overriding the `post()` and `patch()` methods with some custom behavior. I need to use the same parameters as the overridden method, including one named `format`. Since `format` is a Python built-in, `A002` is triggered, but I have no recourse to rename it. For now, I'm excluding `"format"` in `builtin-ignores`, but I'd prefer to keep triggering on `format` in any non-overridden method.

By the way, I'm on Python 3.12 and using `@override` on all my override methods, so it should be easy to determine if a method is an override that way.

---

_Label `bug` added by @charliermarsh on 2024-07-20 00:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-20 01:01_

---

_Referenced in [astral-sh/ruff#12415](../../astral-sh/ruff/pulls/12415.md) on 2024-07-20 01:19_

---

_Closed by @charliermarsh on 2024-07-20 01:32_

---

_Closed by @charliermarsh on 2024-07-20 01:32_

---

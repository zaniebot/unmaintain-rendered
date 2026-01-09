---
number: 16436
title: cached-instance-method (B019) for async_lru and aiocache
type: issue
state: open
author: LuckySting
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-02-28T10:58:34Z
updated_at: 2025-03-01T16:11:27Z
url: https://github.com/astral-sh/ruff/issues/16436
synced_at: 2026-01-07T13:12:16-06:00
---

# cached-instance-method (B019) for async_lru and aiocache

---

_Issue opened by @LuckySting on 2025-02-28 10:58_

### Summary

Hello! Firstly, I want to share my appreciations for the Ruff linter; it is amazing and incredibly fast.

There is a rule (B019) that helps to avoid mistakes when using `functools.lru_cache` in the synchronous world. Meanwhile, in asynchronous Python, there are two widely used libraries: `async_lru` and `aiocache`, which provide decorators that work with async functions. And there are the same problems with caching of `self`.

It would be great if Ruff could check for possible problems in these two libraries.

---

_Label `rule` added by @ntBre on 2025-02-28 19:14_

---

_Label `needs-decision` added by @ntBre on 2025-02-28 19:14_

---

_Label `needs-decision` removed by @ntBre on 2025-02-28 19:15_

---

_Comment by @ntBre on 2025-02-28 19:25_

Thanks for the kind words!

I think that's a reasonable rule extension, and should be quite easy to add here if anyone is interested in taking this on.

https://github.com/astral-sh/ruff/blob/fdf091528346d7b7b4cffe587a989e1d11d41e4c/crates/ruff_linter/src/rules/flake8_bugbear/rules/cached_instance_method.rs#L110-L120

In addition to extending the `matches!` call, we'd just want some new tests and probably to update the docs too.

---

_Label `help wanted` added by @ntBre on 2025-02-28 19:26_

---

_Referenced in [astral-sh/ruff#16450](../../astral-sh/ruff/pulls/16450.md) on 2025-03-01 16:03_

---

_Comment by @LuckySting on 2025-03-01 16:11_

Although I'm not an expert in Rust, I have opened a pull request.

---

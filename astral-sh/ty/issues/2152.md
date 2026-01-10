```yaml
number: 2152
title: Wrong type inference in some cases with Awaitable
type: issue
state: closed
author: tkukushkin
labels: []
assignees: []
created_at: 2025-12-22T09:02:29Z
updated_at: 2025-12-22T09:05:07Z
url: https://github.com/astral-sh/ty/issues/2152
synced_at: 2026-01-10T01:56:41Z
```

# Wrong type inference in some cases with Awaitable

---

_Issue opened by @tkukushkin on 2025-12-22 09:02_

### Summary

Hi! In some cases with `collections.abc.Awaitable`, ty infers wrong type of function call result, `T` becomes `T | None`, [playground link](https://play.ty.dev/7c7bb916-2fb9-4e59-bd12-7681d51057cf).

If I replace `Awaitable` with `collections.abc.Coroutine`, it works fine, [playground link](https://play.ty.dev/7a4830be-c0ef-48de-ab83-532408722bd5).

I noticed this behavior originally with [`anyio.from_thread.run`](https://github.com/agronholm/anyio/blob/master/src/anyio/from_thread.py#L67-L71).

### Version

0.0.5

---

_Closed by @tkukushkin on 2025-12-22 09:04_

---

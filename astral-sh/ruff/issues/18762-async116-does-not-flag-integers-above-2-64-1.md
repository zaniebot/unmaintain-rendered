---
number: 18762
title: "ASYNC116 does not flag integers above `2^64 - 1`"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - rule
assignees: []
created_at: 2025-06-18T17:28:04Z
updated_at: 2025-06-19T09:37:21Z
url: https://github.com/astral-sh/ruff/issues/18762
synced_at: 2026-01-10T01:23:00Z
---

# ASYNC116 does not flag integers above `2^64 - 1`

---

_Issue opened by @MeGaGiGaGon on 2025-06-18 17:28_

### Summary

If the sleep time is an integer above `2^64 - 1` (`18446744073709551615`) it will not be caught by ASYNC1116 [playground](https://play.ruff.rs/ef43cd10-fc56-4df1-badb-3aaeceefec77)

```py
import trio


async def func():
    await trio.sleep(9999999999999999999) # ASYNC116

async def func():
    await trio.sleep(10000000000000000000000000000000000e100000000000000000000000000000000) # ASYNC116

async def func():
    await trio.sleep(99999999999999999999999999999999.0) # ASYNC116

async def func():
    await trio.sleep(18446744073709551615) # ASYNC116

async def func():
    await trio.sleep(18446744073709551616) # No error

async def func():
    await trio.sleep(99999999999999999999) # No error
```

I'm not an async user, but this may be worth fixing since my natural amount of `9`s for a really big number is just above the limit this rule will detect.

I think the bug comes from here https://github.com/astral-sh/ruff/blob/cb512ba80b8594efa3543365a7a3374c4415adba/crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs#L103-L110

If the `as_u64` fails, that should mean the number is bigger than a `u64`, and thus the error should be reported, instead of skipped.

I think floats bypass this issue since they give `inf` if they are too big.

### Version

playground

---

_Label `bug` added by @ntBre on 2025-06-18 17:37_

---

_Label `rule` added by @ntBre on 2025-06-18 17:37_

---

_Referenced in [astral-sh/ruff#18767](../../astral-sh/ruff/pulls/18767.md) on 2025-06-18 20:03_

---

_Closed by @MichaReiser on 2025-06-19 09:37_

---

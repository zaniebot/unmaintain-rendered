---
number: 8606
title: "Omit `asyncio.timeout` from `SIM117`"
type: issue
state: closed
author: maltevesper
labels:
  - bug
assignees: []
created_at: 2023-11-10T18:08:37Z
updated_at: 2023-11-21T11:53:43Z
url: https://github.com/astral-sh/ruff/issues/8606
synced_at: 2026-01-07T13:12:15-06:00
---

# Omit `asyncio.timeout` from `SIM117`

---

_Issue opened by @maltevesper on 2023-11-10 18:08_

While I generally agree with SIM117 (combine with statements where possible), I am wondering if there are cases where there should be exceptions to the rule, in particular `asyncio.timeout` comes to mind.

I.e. I find the following clearer
```
async with asyncio.timeout(0.2):
    async with line_processor:
        await line_processor.stop(0)
```

Less clear in my opinion:
```
async with asyncio.timeout(0.2), line_processor:
    await line_processor.stop(0)
```

While this is a minor cosmetic issue, I am wondering if this warrants a configuration option (a list of contextmanagers for which to ignore SIM117), or is to much of an edge case to consider complicating the configuration.

For now I ignore SIM117 for tests, which is the main case in which I trigger these.

---

_Label `configuration` added by @charliermarsh on 2023-11-11 15:12_

---

_Comment by @charliermarsh on 2023-11-11 15:13_

This seems like a reasonable suggestion -- I agree that collapsing the timeout here is awkward, since semantically it's meant to be wrapping the inner `async with` rather than peer to it. We could start just by omitting `asyncio.timeout`, and parameterize it later on if there's more need.

---

_Renamed from "SIM117 ignore for asyncio.timeout?" to "Omit `asyncio.timeout` from `SIM117`" by @charliermarsh on 2023-11-11 15:13_

---

_Label `bug` added by @charliermarsh on 2023-11-11 15:13_

---

_Comment by @charliermarsh on 2023-11-11 15:14_

PR welcome if you're interested :) The changes would be in [`multiple_with_statements`](https://github.com/astral-sh/ruff/blob/d7144d6d8eb06177be977fdbdf3ec38ea1c8f721/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs#L77C10-L77C10), we'd want to check if the `with_parent` has a single item that matches `asyncio.timeout`.

---

_Label `configuration` removed by @charliermarsh on 2023-11-11 15:15_

---

_Comment by @zanieb on 2023-11-11 16:30_

If we're going with the spirit of this change, we should probably also include [AnyIO](https://anyio.readthedocs.io/en/stable/api.html#timeouts-and-cancellation)/[Trio](https://trio.readthedocs.io/en/stable/reference-core.html#trio.move_on_after)'s `move_on_after`, `fail_after`, `move_on_at`, `fail_at`, and maybe`CancelScope`.

---

_Comment by @maltevesper on 2023-11-12 17:59_

I will happily have a go at a PR this week (am a little busy)

Locks crossed my mind as a possible further candidate for "more explicit scoping", but I will stick with timing for starters.

---

_Comment by @charliermarsh on 2023-11-12 18:01_

Awesome, just ask here or in Discord if you have any questions.

---

_Referenced in [astral-sh/ruff#8801](../../astral-sh/ruff/pulls/8801.md) on 2023-11-21 06:05_

---

_Closed by @charliermarsh on 2023-11-21 11:53_

---

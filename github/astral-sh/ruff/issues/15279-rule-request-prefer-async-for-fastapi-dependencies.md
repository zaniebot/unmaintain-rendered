---
number: 15279
title: "Rule request: prefer async for FastAPI dependencies"
type: issue
state: open
author: spladug
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-05T22:02:11Z
updated_at: 2025-01-06T20:25:46Z
url: https://github.com/astral-sh/ruff/issues/15279
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule request: prefer async for FastAPI dependencies

---

_Issue opened by @spladug on 2025-01-05 22:02_

When a synchronous function is used as a dependency factory for FastAPI, the framework runs the function on a threadpool. This is done even if the factory never does IO, which can be quite wasteful. Since it's super tempting to not async-color the function when it's doing something super simple, it seems like it'd be useful to lint for this to avoid potential performance pitfalls.

See https://github.com/zhanymkanov/fastapi-best-practices?tab=readme-ov-file#prefer-async-dependencies for more info.

---

_Comment by @MichaReiser on 2025-01-06 08:40_

Thanks. 

I think the challenge with a rule such as this is how Ruff should quantify "a small non-IO operation"

> And threads here also come with a price and limitations, that are redundant, if you just make a small non-I/O operation.

I worry that suggest a rule would have a high false positive rate. It might be a better fit for Ruff once we have a "suggestion" severity that only applies to the IDE context.

---

_Label `rule` added by @MichaReiser on 2025-01-06 08:40_

---

_Label `needs-decision` added by @MichaReiser on 2025-01-06 08:40_

---

_Comment by @spladug on 2025-01-06 20:25_

Yeah, that's totally fair.  Difficult to tell without some way of identifying a function as pure.

I could maybe see it being an opt-in rule as in an asyncio-primary codebase it's definitely the exception to want the synchronous style for IO anyway. 

---

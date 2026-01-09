---
number: 16811
title: inconsistent behavior regarding asyncio-dangling-task (RUF006)
type: issue
state: open
author: amirsoroush
labels:
  - question
  - rule
assignees: []
created_at: 2025-03-17T15:09:37Z
updated_at: 2025-03-17T21:33:34Z
url: https://github.com/astral-sh/ruff/issues/16811
synced_at: 2026-01-07T13:12:16-06:00
---

# inconsistent behavior regarding asyncio-dangling-task (RUF006)

---

_Issue opened by @amirsoroush on 2025-03-17 15:09_

### Summary

I see an inconsistent behavior in detecting `RUF006` rule.

```python
import asyncio


async def coro1() -> None:
    while True:
        print("inside coro1...")
        await asyncio.sleep(1)

async def main() -> None:
    global t1
    s = set()

    asyncio.create_task(coro1())        # 1  (detects correctly)
    t1 = asyncio.create_task(coro1())   # 2  (discards correctly)
    t2 = asyncio.create_task(coro1())   # 3  (?)
    s.add(asyncio.create_task(coro1())) # 4  (?)

    await asyncio.sleep(5)
    print("done")

asyncio.run(main())
```
Output for `# 1` and `# 3` is:
```none
Store a reference to the return value of `asyncio.create_task`
```

Ruff correctly detects `# 1` and correctly discards `# 2`(it has a reference in global). Now if Ruff decides to detect `# 3` as error then `# 4` should also be detected as error. There is no difference in how the returned references are stored. 

If Ruff detects `# 3` due to the fact that `t2` is only referenced inside the contained coroutine(and creates a circular reference) then it's the same as `# 4`. One of them inside a local variable and one of them inside a `set` object. In this case it should only discards `# 4` if the `set` object is defined outside in a global namespace.

I don't know what the correct decision is but I guess those should be treated equally.

I might have missed something here, I would be grateful if you correct me. Thanks.

### Version

ruff==0.10.0 (shipped with VSCode Ruff extension version 2025.18.0)

---

_Comment by @ntBre on 2025-03-17 19:49_

Thanks for the report! I think this corresponds to one of the known false negative test cases added in https://github.com/astral-sh/ruff/pull/9060, but I don't see a tracking issue for it.

I'm not that familiar with these `async` semantics, but I think I agree with you that ruff is correctly flagging (3), which means it should also flag (4) since these are both local variables. ([playground link](https://play.ruff.rs/97411537-7fba-48a6-9777-70c1b6d6c0c2))

---

_Label `rule` added by @ntBre on 2025-03-17 19:49_

---

_Comment by @MichaReiser on 2025-03-17 20:12_

I think the reason ruff doesn't dedect 4 is because s.add could await its parameter in some form or another. This seems a case where type checking can find this isse easily but we'd have to encode a long list of known methods that don't accept tasks

---

_Label `question` added by @MichaReiser on 2025-03-17 20:12_

---

_Comment by @amirsoroush on 2025-03-17 20:28_

> I think the reason ruff doesn't dedect 4 is because s.add could await its parameter in some form or another.

I'm pretty sure `s.add` can't await its arguments. It's just a builtin method of set object and behave semantically like `list.append`. Whether the task is get executed immediately or not depends on the eagerness of the [Task object](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task) itself which is another story.


---

_Comment by @ntBre on 2025-03-17 21:27_

I think @MichaReiser was referring to a more general case, where it's hard to tell the type of `s`. As a (likely silly) example,

```python
class Awaiter:
    async def add(self, task):
        await task

s = Awaiter()
...
s.add(task)
```

ruff could currently handle specific cases like `set` if we can resolve its type within a single file, but a more general approach requires type checking (and likely multi-file analysis) to really know what `s.add` means in a given context. Otherwise we end up with the long list Micha mentioned of methods like `set.add` and `list.append`. So we'll need better type inference to handle this robustly.

---

_Comment by @amirsoroush on 2025-03-17 21:33_

@ntBre oops my bad. Yes I got you.

---

_Referenced in [astral-sh/ruff#20421](../../astral-sh/ruff/issues/20421.md) on 2025-09-16 08:58_

---

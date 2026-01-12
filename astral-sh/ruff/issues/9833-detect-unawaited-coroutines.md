```yaml
number: 9833
title: Detect unawaited coroutines
type: issue
state: open
author: alekssamos
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-02-05T15:24:22Z
updated_at: 2025-07-12T16:30:47Z
url: https://github.com/astral-sh/ruff/issues/9833
synced_at: 2026-01-12T15:54:49Z
```

# Detect unawaited coroutines

---

_@alekssamos_

Hello. I fixed my code via ruff.
Applied fix and  unsafe fixes.
But ruff did not find one error.
I forgot to write await when calling an asynchronous function.
Accordingly, I received RuntimeWarning never awaited...
e.g.
```python
async def another():
    ...

async def main():
    result = another()
```
Is there any way to do this, and if not, will it be taken into account in the new versions of ruff?

---

_Label `rule` added by @charliermarsh on 2024-02-05 15:55_

---

_Comment by @charliermarsh on 2024-02-05 15:55_

I don't believe we check this now, but it looks like a reasonable rule to me.

---

_Renamed from "Question: how do I detect a forgotten await?" to "Detect `async` function without `await`" by @charliermarsh on 2024-02-05 15:56_

---

_Comment by @mikeleppane on 2024-02-08 14:11_

WIP

---

_Comment by @AlexWaygood on 2024-02-09 13:36_

Note that mypy already implements this rule with its `unused-awaitable` error code: https://mypy.readthedocs.io/en/stable/error_code_list2.html#check-that-awaitable-return-value-is-used-unused-awaitable.

That doesn't stop ruff implementing it as well -- but I feel like it might be a better fit for a type checker, as a type checker has the power to analyse multiple files at a time and track which calls return awaitable objects. Ruff doesn't (yet) have the power to do that (though it's obviously something we'd like to have).

Still, it can be hard to configure mypy to enable just that warning without also enabling other warnings that might be unwelcome -- and maybe we can provide a valuable lint here even while only looking at one file at a time.

---

_Comment by @charliermarsh on 2024-02-09 14:01_

@AlexWaygood - I guess the rule as implemented here is actually a bit different (unclear if better or worse) than what's described in the Mypy docs, since this raises whenever you have an `async` method that doesn't await anything (like Clippy's unnecessary async rule), whereas the Mypy rule seems to raise on the _awaitable_ rather than the async function. Is that correct?

---

_Comment by @AlexWaygood on 2024-02-09 14:05_

> @AlexWaygood - I guess the rule as implemented here is actually a bit different (unclear if better or worse) than what's described in the Mypy docs, since this raises whenever you have an `async` method that doesn't await anything (like Clippy's unnecessary async rule), whereas the Mypy rule seems to raise on the _awaitable_ rather than the async function. Is that correct?

Ah interesting -- I think what you describe is possibly what's originally being asked for in this issue, but the implementation that's been put forward for consideration in https://github.com/astral-sh/ruff/pull/9911 is more similar to mypy's `unused-awaitable` check than what you're describing

---

_Comment by @charliermarsh on 2024-02-09 14:07_

Oh sorry, I hadn't looked at the code yet. I was imagining we'd look for `async` function definitions that lack any `await` or `async` iterator (like `async for`, etc.).

---

_Comment by @zanieb on 2024-02-09 16:02_

Yeah it sounds like #9911 addresses a separate (but also valid) use case

---

_Comment by @plredmond on 2024-02-09 18:36_

~~Hi, I've blocked out time to do this on Monday.~~

[Edit: I'll work on #9951, not this.]

---

_Assigned to @plredmond by @zanieb on 2024-02-09 23:44_

---

_Renamed from "Detect `async` function without `await`" to "Detect unawaited coroutines" by @zanieb on 2024-02-12 19:22_

---

_Unassigned @plredmond by @zanieb on 2024-02-12 19:24_

---

_Assigned to @mikeleppane by @zanieb on 2024-02-12 19:24_

---

_Comment by @Gu1nness on 2024-08-19 12:00_

Do you know when this will be implemented?
This would be a great addition to ruff!

---

_Label `multifile-analysis` added by @MichaReiser on 2024-08-19 12:01_

---

_Comment by @MichaReiser on 2024-08-19 12:02_

This will take a while. Ruff cannot currently resolve functions across files, which is a requirement for this rule to work reliable.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @mikeckennedy on 2024-11-19 17:39_

I want to just add a strong +1 here for detecting non-awaited coroutines. Check out the discussion here:

https://www.reddit.com/r/Python/comments/1gulzjt/rewriting_4000_lines_of_python_to_migrate_to/

One of the commenters says they have a huge code base but can not (never?) migrate to async because it's just too hard to make sure all the code works together (like no missed awaits). This feature would make Ruff a must have tool for such upgrades.

However, I realize this is super hard. For example, this should be fine even though the coroutines are not directly awaited.

```python
t1 = async_thing_1()
t2 = async_thing_2()

await task.gather(t1, t2)
```

---

_Comment by @Zac-HD on 2025-02-05 01:23_

Note that the `gather()` example is only a difficulty for `asyncio`; neither `anyio` nor `trio` permit calling async functions without immediately awaiting them, in large part to avoid this kind of confusion.  Ruff could be very valuable for these use-cases, even if the rule had to be disabled for asyncio codebases, or at least disable-if-retval-used.

(idiomatically, you'd call `my_gather(partial(fn1, *args), partial(fn2, **kwargs), ...)` - a list of functions)

---

_Comment by @mikeckennedy on 2025-02-08 16:07_

Hey @Zac-HD nice to see you here. I'm somewhat familiar with Trio and AnyIO. But how do you actually run two or more things concurrently without starting them before awaiting them?

```python
for x in xes:
   await async_thing(x)
```

still only runs one thing at a time. And await gather(f(x), g(x), ...) is fine but can be tricky if you have to build up the args for each one in a loop.

---

_Comment by @zanieb on 2025-02-08 18:15_

@mikeckennedy I believe the canonical example would be using `TaskGroup.start_soon` (https://anyio.readthedocs.io/en/stable/tasks.html#creating-and-managing-tasks)

---

_Comment by @pirate on 2025-05-28 17:49_

Would this be a feature better suited to https://github.com/astral-sh/ty ?

---

_Comment by @MichaReiser on 2025-05-28 20:06_

Maybe in the future. Our focus right now is exclusively on typing features (things that are simply incorrect and not just suspicious)

---

_Comment by @JoshPaulie on 2025-07-12 15:09_

Big fan of this rule proposal!

---

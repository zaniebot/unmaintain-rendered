---
number: 8451
title: Implement flake8-async rules
type: issue
state: open
author: charliermarsh
labels:
  - plugin
assignees: []
created_at: 2023-11-03T00:55:27Z
updated_at: 2025-09-05T11:10:56Z
url: https://github.com/astral-sh/ruff/issues/8451
synced_at: 2026-01-07T13:12:15-06:00
---

# Implement flake8-async rules

---

_Issue opened by @charliermarsh on 2023-11-03 00:55_

## General Rules

- [x] `ASYNC100`: `A with trio.fail_after(...): or with trio.move_on_after(...): context does not contain any await statements. This makes it pointless, as the timeout can only be triggered by a checkpoint.`
- [ ] `ASYNC101`: `yield inside a nursery or cancel scope is only safe when implementing a context manager - otherwise, it breaks exception handling.`
- [ ] `ASYNC102`: `It's unsafe to await inside finally: or except BaseException/trio.Cancelled unless you use a shielded cancel scope with a timeout.`
- [ ] `ASYNC103`: `except BaseException, except trio.Cancelled or a bare except: with a code path that doesn't re-raise. If you don't want to re-raise BaseException, add a separate handler for trio.Cancelled before.`
- [ ] `ASYNC104`: `Cancelled and BaseException must be re-raised - when a user tries to return or raise a different exception.`
- [x] `ASYNC105`: `Calling a trio async function without immediately awaiting it.`
- [ ] `ASYNC106`: `trio must be imported with import trio for the linter to work.`
- [x] `ASYNC109`: `Async function definition with a timeout parameter - use trio.[fail/move_on]_[after/at] instead`
- [x] `ASYNC110`: `while <condition>: await trio.sleep() should be replaced by a trio.Event.`
- [ ] `ASYNC111`: `Variable, from context manager opened inside nursery, passed to start[_soon] might be invalidly accessed while in use, due to context manager closing before the nursery. This is usually a bug, and nurseries should generally be the inner-most context manager.`
- [ ] `ASYNC112`: `Nursery body with only a call to nursery.start[_soon] and not passing itself as a parameter can be replaced with a regular function call.`
- [ ] `ASYNC113`: `Using nursery.start_soon in __aenter__ doesn't wait for the task to begin. Consider replacing with nursery.start.`
- [ ] `ASYNC114`: `Startable function (i.e. has a task_status keyword parameter) not in --startable-in-context-manager parameter list, please add it so TRIO113 can catch errors when using it.`
- [x] `ASYNC115`: `Replace trio.sleep(0) with the more suggestive trio.lowlevel.checkpoint().`
- [x] `ASYNC116`: `trio.sleep() with >24 hour interval should usually be trio.sleep_forever().`
- [ ] `ASYNC118`: `Don't assign the value of anyio.get_cancelled_exc_class() to a variable, since that breaks linter checks and multi-backend programs.`
- [ ] `ASYNC119`: yield in context manager in async generator is unsafe, the cleanup may be delayed until await is no longer allowed. We strongly encourage you to read [PEP 533](https://peps.python.org/pep-0533/) and use [async with aclosing(…)](https://docs.python.org/3/library/contextlib.html#contextlib.aclosing), or better yet avoid async generators entirely (see [ASYNC900](https://flake8-async.readthedocs.io/en/latest/rules.html#async900) ) in favor of context managers which return an iterable [channel/stream/queue](https://flake8-async.readthedocs.io/en/latest/glossary.html#channel-stream-queue).
- [ ] `ASYNC120`: Dangerous [Checkpoint](https://flake8-async.readthedocs.io/en/latest/glossary.html#checkpoint) inside an except block. If this checkpoint is cancelled, the current active exception will be replaced by the Cancelled exception, and cannot be reraised later. This will not trigger when [ASYNC102](https://flake8-async.readthedocs.io/en/latest/rules.html#async102) does, and if you don’t care about losing non-cancelled exceptions you could disable this rule. This is currently not able to detect asyncio shields.

## Blocking sync calls in async functions
- [ ] `ASYNC200`: `User-configured error for blocking sync calls in async functions. Does nothing by default, see trio200-blocking-calls for how to configure it.`
- [x] `ASYNC210`: `Sync HTTP call in async function, use httpx.AsyncClient.`
- [ ] `ASYNC211`: `Likely sync HTTP call in async function, use httpx.AsyncClient. Looks for urllib3 method calls on pool objects, but only matching on the method signature and not the object.`
- [x] `ASYNC212`: `Blocking sync HTTP call on httpx object, use httpx.AsyncClient.`
- [x] `ASYNC220`: `Sync process call in async function, use await nursery.start(trio.run_process, ...).`
- [x] `ASYNC221`: `Sync process call in async function, use await trio.run_process(...).`
- [x] `ASYNC222`: `Sync os.* call in async function, wrap in await trio.to_thread.run_sync().`
- [x] `ASYNC230`: `Sync IO call in async function, use trio.open_file(...).`
- [ ] `ASYNC231`: `Sync IO call in async function, use trio.wrap_file(...).`
- [ ] `ASYNC232`: `Blocking sync call on file object, wrap the file object in trio.wrap_file() to get an async file object.`
- [ ] `ASYNC240`: `Avoid using os.path in async functions, prefer using trio.Path objects.`
- [ ] `ASYNC250`: Builtin [input()](https://docs.python.org/3/library/functions.html#input) should not be called from async function. Wrap in [trio.to_thread.run_sync()](https://trio.readthedocs.io/en/latest/reference-core.html#trio.to_thread.run_sync)/[anyio.to_thread.run_sync()](https://anyio.readthedocs.io/en/latest/api.html#anyio.to_thread.run_sync) or [asyncio.loop.run_in_executor()](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.run_in_executor).
- [x] `ASYNC251`: [time.sleep()](https://docs.python.org/3/library/time.html#time.sleep) should not be called from async function. Use [trio.sleep()](https://trio.readthedocs.io/en/latest/reference-core.html#trio.sleep)/[anyio.sleep()](https://anyio.readthedocs.io/en/latest/api.html#anyio.sleep)/[asyncio.sleep()](https://docs.python.org/3/library/asyncio-task.html#asyncio.sleep).

## Asyncio-specific rules
- [ ] `ASYNC300`: Calling [asyncio.create_task()](https://docs.python.org/3/library/asyncio-task.html#asyncio.create_task) without saving the result. A task that isn’t referenced elsewhere may get garbage collected at any time, even before it’s done. Note that this rule won’t check whether the variable the result is saved in is susceptible to being garbage-collected itself. See the asyncio documentation for best practices. You might consider instead using a [TaskGroup](https://flake8-async.readthedocs.io/en/latest/glossary.html#taskgroup-nursery) and calling [asyncio.TaskGroup.create_task()](https://docs.python.org/3/library/asyncio-task.html#asyncio.TaskGroup.create_task) to avoid this problem, and gain the advantages of structured concurrency with e.g. better cancellation semantics.

## Optional rules disabled by default

- [ ] `ASYNC900`: `Async generator without @asynccontextmanager not allowed.`
- [ ] `ASYNC910`: `Exit or return from async function with no guaranteed checkpoint or exception since function definition.`
- [ ] `ASYNC911`: `Exit, yield or return from async iterable with no guaranteed checkpoint since possible function entry (yield or function definition) Checkpoints are await, async for, and async with (on one of enter/exit).`
- [ ] `ASYNC912`: A timeout/cancelscope has [checkpoints](https://flake8-async.readthedocs.io/en/latest/glossary.html#checkpoint), but they’re not guaranteed to run. Similar to [ASYNC100](https://flake8-async.readthedocs.io/en/latest/rules.html#async100), but it does not warn on trivial cases where there is no checkpoint at all. It instead shares logic with [ASYNC910](https://flake8-async.readthedocs.io/en/latest/rules.html#async910) and [ASYNC911](https://flake8-async.readthedocs.io/en/latest/rules.html#async911) for parsing conditionals and branches.
- [ ] `ASYNC913`: An indefinite loop (e.g. while True) has no guaranteed [checkpoint](https://flake8-async.readthedocs.io/en/latest/glossary.html#checkpoint). This could potentially cause a deadlock.


## Resources

* [flake8-async Rules](https://flake8-async.readthedocs.io/en/latest/rules.html)

---

_Referenced in [astral-sh/ruff#8439](../../astral-sh/ruff/pulls/8439.md) on 2023-11-03 00:57_

---

_Label `plugin` added by @charliermarsh on 2023-11-03 01:19_

---

_Referenced in [astral-sh/ruff#8468](../../astral-sh/ruff/pulls/8468.md) on 2023-11-03 12:49_

---

_Referenced in [astral-sh/ruff#8486](../../astral-sh/ruff/pulls/8486.md) on 2023-11-04 12:32_

---

_Comment by @qdegraaf on 2023-11-04 14:17_

`TRIO118` sounds like it is not needed in Ruff, similar to `TRIO106`

The `TRIO2XX` rules are partially covered by the port of [flake8_async](https://github.com/astral-sh/ruff/tree/75c9be099f878c381c8734ecd7c1217c44d2b612/crates/ruff_linter/src/rules/flake8_async/rules). 

The recommended fixes in `flake8-trio` are TRIO specific though. How do we want to combine those two? 


---

_Referenced in [astral-sh/ruff#8490](../../astral-sh/ruff/pulls/8490.md) on 2023-11-04 16:33_

---

_Referenced in [astral-sh/ruff#8534](../../astral-sh/ruff/pulls/8534.md) on 2023-11-07 06:59_

---

_Referenced in [astral-sh/ruff#8537](../../astral-sh/ruff/pulls/8537.md) on 2023-11-07 12:23_

---

_Referenced in [astral-sh/ruff#8578](../../astral-sh/ruff/pulls/8578.md) on 2023-11-09 09:23_

---

_Comment by @jakkdl on 2023-11-10 16:00_

whoa, this is super cool!! As the primary author of flake8-trio I'm super honored to be included in ruff :heart: 
Feel free to ping me about any questions of functionality or implementation details
@Zac-HD might also have opinions or suggestions about stuff

---

_Comment by @jakkdl on 2023-11-10 16:05_

> The recommended fixes in `flake8-trio` are TRIO specific though. How do we want to combine those two?

There's some `anyio` support as well in flake8-trio and for a while we considered renaming the library. If `flake8-trio` sees `import anyio` and no `import trio` in the code, or [--anyio](https://github.com/Zac-HD/flake8-trio#--anyio) is passed it will suggest autofixes from the anyio library instead of trio.

---

_Comment by @jakkdl on 2023-11-10 16:37_

> TRIO118 sounds like it is not needed in Ruff, similar to TRIO106

TRIO106 definitely isn't needed by itself, and solely a consequence of implementation at the time. TRIO118 is only partially a weakness in implementation, the part about multi-backend programs still holds though.

> The TRIO2XX rules are partially covered by the port of [flake8_async](https://github.com/astral-sh/ruff/tree/75c9be099f878c381c8734ecd7c1217c44d2b612/crates/ruff_linter/src/rules/flake8_async/rules).

Yeah there's overlap there, though I gave the implementation in flake8-trio quite a bit more love and the suggestions/error messages are more detailed/specific since it can know what the backend is.



TRIO105 is mostly redundant as of the recently released trio 0.23 which included types into the main package, and type checkers will complain about async functions not being awaited. I suppose it could be useful for somebody not using type-checking, but enabling this check, but I think that's a pretty slim proportion and the rule requires constantly keeping up with the API of anyio/trio.

---

_Comment by @Zac-HD on 2023-11-12 23:05_

🥳 I'm delighted to see this going into `ruff`!

`TRIO105` doesn't seem redundant to me - many people aren't using type annotations (everywhere), and typecheckers also don't offer autofixers the way that `ruff` can.  Those fixers would mostly be unsafe, but still remarkably useful IMO!

---

_Comment by @charliermarsh on 2023-11-12 23:08_

Thank you both so much for chiming in here, for all the helpful commentary, and for all your work on flake8-trio (among so much else)!

I already gated one rule behind "require an `import trio` to be present before firing" so we now have some precedent and infrastructure for that, which will likely be helpful with a few of the other rules and their associated fixes.

---

_Comment by @alex007sirois on 2023-11-21 21:34_

> I already gated one rule behind "require an import trio to be present before firing"

Most of the rules for trio would apply for anyio too:

> > The recommended fixes in `flake8-trio` are TRIO specific though. How do we want to combine those two?
> 
> There's some `anyio` support as well in flake8-trio and for a while we considered renaming the library. If `flake8-trio` sees `import anyio` and no `import trio` in the code, or [--anyio](https://github.com/Zac-HD/flake8-trio#--anyio) is passed it will suggest autofixes from the anyio library instead of trio.



---

_Comment by @jakkdl on 2024-04-12 13:50_

This issue should be renamed and all the rules in the checklist renamed to match upstream merging with flake8-async. See #10303

Also TRIO117 is removed (because `trio.MultiError` is removed), and there's two new rules ASYNC250 and ASYNC251

---

_Referenced in [astral-sh/ruff#11498](../../astral-sh/ruff/pulls/11498.md) on 2024-05-22 20:49_

---

_Referenced in [astral-sh/ruff#10416](../../astral-sh/ruff/pulls/10416.md) on 2024-06-25 21:22_

---

_Renamed from "Implement flake8-trio rules" to "Implement flake8-async rules" by @MichaReiser on 2024-06-26 07:51_

---

_Comment by @MichaReiser on 2024-06-26 07:59_

I updated the issue to reflect the `flake8-trio` to `flake8-async` rename, crossed of the new rules implemented in #10416, and added new rules to the list to match `flake8-async`'s documentation

---

_Referenced in [astral-sh/ruff#12873](../../astral-sh/ruff/issues/12873.md) on 2024-08-14 01:25_

---

_Comment by @Zac-HD on 2024-08-15 03:02_

Hey all - I just tried moving some code from `flake8-async` to `ruff check` for that delicious massive speedup, and ended up back on this issue 🙂 

The good news: https://github.com/astral-sh/ruff/pull/10416 also implemented `ASYNC251`!

The sad-for-me news: lots of other rules still to implement.  If you feel inspired but not _so_ inspired as to finish closing out this issue, `ASYNC900` and `ASYNC200` seem pretty simple to implement, and would be _really_ nice to have...

---

_Referenced in [astral-sh/ruff#13341](../../astral-sh/ruff/issues/13341.md) on 2024-09-12 23:38_

---

_Comment by @Zac-HD on 2024-09-18 22:42_

Prompted by a conversation elsewhere: if `ruff` starts to handle cross-file analyses, you could avoid `ASYNC114` entirely (ie check whether there's a `task_status=` argument instead of configuring a list) which would be quite nice 🙂 

---

_Comment by @charliermarsh on 2024-09-19 18:49_

Also prompted by a conversation elsewhere: I'd love to see implementations for `ASYNC900`, `ASYNC910`, and `ASYNC911` in particular.

---

_Comment by @jakkdl on 2024-10-22 14:05_

New rules in upstream https://flake8-async.readthedocs.io/en/latest/rules.html#async121
* `ASYNC121` control-flow-in-taskgroup
* `ASYNC122` delayed-entry-of-relative-cancelscope
* `ASYNC123` bad-exception-group-flattening
  * this one isn't specific to async, and affects anybody trying to extract and reraise an exception contained inside an exception group. Upstream implementation is fairly crude

---

_Referenced in [astral-sh/ruff#19619](../../astral-sh/ruff/pulls/19619.md) on 2025-08-04 20:19_

---

_Referenced in [astral-sh/ruff#20091](../../astral-sh/ruff/pulls/20091.md) on 2025-08-26 00:18_

---

_Referenced in [astral-sh/ruff#20122](../../astral-sh/ruff/pulls/20122.md) on 2025-08-28 00:20_

---

_Referenced in [astral-sh/ruff#20264](../../astral-sh/ruff/pulls/20264.md) on 2025-09-05 10:08_

---

_Comment by @PieterCK on 2025-09-05 10:33_

Hello folks! Not, sure if this is the best place to ask for reviews, but I've opened #20264 for `ASYNC240`. I'd appreciate it if someone could drop a quick review 🙂 


---

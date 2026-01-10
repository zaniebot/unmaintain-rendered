---
number: 2081
title: wrong invalid-argument-type errors for prefect task submit function
type: issue
state: closed
author: maleasy
labels: []
assignees: []
created_at: 2025-12-18T17:27:35Z
updated_at: 2026-01-07T08:00:36Z
url: https://github.com/astral-sh/ty/issues/2081
synced_at: 2026-01-10T01:51:14Z
---

# wrong invalid-argument-type errors for prefect task submit function

---

_Issue opened by @maleasy on 2025-12-18 17:27_

### Summary

Example code (sorry, I did not manage to reproduce this without importing prefect):

```py
from prefect.futures import wait
from prefect import flow, task

@task
def task_get() -> int:
    """Task get integer."""
    return 42

@task
def task_add(x: int, y: int) -> int:
    """Task add two integers."""
    print(f"Adding {x} and {y}")
    return x + y

@flow
def my_flow():
    """My flow."""
    x = 23
    future_y = task_get.submit()

    future_xy = task_add.submit(x, future_y)
    future_yy = task_add.submit(future_y, future_y)
    future_xx = task_add.submit(x, x)

    xx = task_add(x, x)
    yy = task_add(future_y, future_y)
    xy = task_add(x, future_y)

    wait([future_xy, future_yy, future_xx])


if __name__ == "__main__":
    my_flow()
```

`ty check` output:

```
error[invalid-argument-type]: Argument to bound method `submit` is incorrect
  --> tmp/tmp.py:21:33
   |
19 |     future_y = task_get.submit()
20 |
21 |     future_xy = task_add.submit(x, future_y)
   |                                 ^ Expected `(x: int, y: int)`, found `Literal[23]`
22 |     future_yy = task_add.submit(future_y, future_y)
23 |     future_xx = task_add.submit(x, x)
   |
info: Matching overload defined here
    --> .venv/lib/python3.12/site-packages/prefect/tasks.py:1066:9
     |
1065 |     @overload
1066 |     def submit(
     |         ^^^^^^
1067 |         self: "Task[P, R]",
1068 |         *args: P.args,
     |         ------------- Parameter declared here
1069 |         **kwargs: P.kwargs,
1070 |     ) -> PrefectFuture[R]: ...
     |
info: Non-matching overloads for bound method `submit`:
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to bound method `submit` is incorrect
  --> tmp/tmp.py:21:36
   |
19 |     future_y = task_get.submit()
20 |
21 |     future_xy = task_add.submit(x, future_y)
   |                                    ^^^^^^^^ Expected `(x: int, y: int)`, found `PrefectFuture[int]`
22 |     future_yy = task_add.submit(future_y, future_y)
23 |     future_xx = task_add.submit(x, x)
   |
info: Matching overload defined here
    --> .venv/lib/python3.12/site-packages/prefect/tasks.py:1066:9
     |
1065 |     @overload
1066 |     def submit(
     |         ^^^^^^
1067 |         self: "Task[P, R]",
1068 |         *args: P.args,
     |         ------------- Parameter declared here
1069 |         **kwargs: P.kwargs,
1070 |     ) -> PrefectFuture[R]: ...
     |
info: Non-matching overloads for bound method `submit`:
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to bound method `submit` is incorrect
  --> tmp/tmp.py:22:33
   |
21 |     future_xy = task_add.submit(x, future_y)
22 |     future_yy = task_add.submit(future_y, future_y)
   |                                 ^^^^^^^^ Expected `(x: int, y: int)`, found `PrefectFuture[int]`
23 |     future_xx = task_add.submit(x, x)
   |
info: Matching overload defined here
    --> .venv/lib/python3.12/site-packages/prefect/tasks.py:1066:9
     |
1065 |     @overload
1066 |     def submit(
     |         ^^^^^^
1067 |         self: "Task[P, R]",
1068 |         *args: P.args,
     |         ------------- Parameter declared here
1069 |         **kwargs: P.kwargs,
1070 |     ) -> PrefectFuture[R]: ...
     |
info: Non-matching overloads for bound method `submit`:
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to bound method `submit` is incorrect
  --> tmp/tmp.py:22:43
   |
21 |     future_xy = task_add.submit(x, future_y)
22 |     future_yy = task_add.submit(future_y, future_y)
   |                                           ^^^^^^^^ Expected `(x: int, y: int)`, found `PrefectFuture[int]`
23 |     future_xx = task_add.submit(x, x)
   |
info: Matching overload defined here
    --> .venv/lib/python3.12/site-packages/prefect/tasks.py:1066:9
     |
1065 |     @overload
1066 |     def submit(
     |         ^^^^^^
1067 |         self: "Task[P, R]",
1068 |         *args: P.args,
     |         ------------- Parameter declared here
1069 |         **kwargs: P.kwargs,
1070 |     ) -> PrefectFuture[R]: ...
     |
info: Non-matching overloads for bound method `submit`:
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to bound method `submit` is incorrect
  --> tmp/tmp.py:23:33
   |
21 |     future_xy = task_add.submit(x, future_y)
22 |     future_yy = task_add.submit(future_y, future_y)
23 |     future_xx = task_add.submit(x, x)
   |                                 ^ Expected `(x: int, y: int)`, found `Literal[23]`
24 |
25 |     xx = task_add(x, x)
   |
info: Matching overload defined here
    --> .venv/lib/python3.12/site-packages/prefect/tasks.py:1066:9
     |
1065 |     @overload
1066 |     def submit(
     |         ^^^^^^
1067 |         self: "Task[P, R]",
1068 |         *args: P.args,
     |         ------------- Parameter declared here
1069 |         **kwargs: P.kwargs,
1070 |     ) -> PrefectFuture[R]: ...
     |
info: Non-matching overloads for bound method `submit`:
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to bound method `submit` is incorrect
  --> tmp/tmp.py:23:36
   |
21 |     future_xy = task_add.submit(x, future_y)
22 |     future_yy = task_add.submit(future_y, future_y)
23 |     future_xx = task_add.submit(x, x)
   |                                    ^ Expected `(x: int, y: int)`, found `Literal[23]`
24 |
25 |     xx = task_add(x, x)
   |
info: Matching overload defined here
    --> .venv/lib/python3.12/site-packages/prefect/tasks.py:1066:9
     |
1065 |     @overload
1066 |     def submit(
     |         ^^^^^^
1067 |         self: "Task[P, R]",
1068 |         *args: P.args,
     |         ------------- Parameter declared here
1069 |         **kwargs: P.kwargs,
1070 |     ) -> PrefectFuture[R]: ...
     |
info: Non-matching overloads for bound method `submit`:
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[False], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> PrefectFuture[R@Task]
info:   (self: Task[P@Task, Coroutine[Any, Any, R@Task]], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info:   (self: Task[P@Task, R@Task], *args: P@Task.args, *, return_state: Literal[True], wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, **kwargs: P@Task.kwargs) -> State[R@Task]
info: rule `invalid-argument-type` is enabled by default

Found 6 diagnostics
```

maybe related to #2027 ?

### Version

0.0.3

---

_Comment by @carljm on 2025-12-18 17:52_

Thanks! Yeah these other methods of prefect's `Task` are all overloaded in a similar way to `__call__`, so I strongly suspect this has the same root cause and same fix as #2027. Will leave it open as a sub-issue to make sure we verify the fix covers both cases, though.

---

_Added to milestone `Stable` by @carljm on 2025-12-18 17:52_

---

_Closed by @dhruvmanila on 2026-01-07 08:00_

---

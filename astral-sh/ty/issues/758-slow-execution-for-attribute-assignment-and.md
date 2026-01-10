```yaml
number: 758
title: Slow execution for attribute assignment and access with reachability constraints
type: issue
state: closed
author: Glyphack
labels:
  - performance
  - attribute access
assignees: []
created_at: 2025-07-02T20:53:59Z
updated_at: 2025-08-28T14:25:08Z
url: https://github.com/astral-sh/ty/issues/758
synced_at: 2026-01-10T02:06:24Z
```

# Slow execution for attribute assignment and access with reachability constraints

---

_Issue opened by @Glyphack on 2025-07-02 20:53_

### Summary

This was found by mypy primer in my [PR](https://github.com/astral-sh/ruff/pull/18473/).

The following program has a high execution time:

```py
from typing import Self

EMPTY = b""


class GridOut:
    def __init__(self: Self) -> None:
        self._buffer_pos = 0
        self._buffer = b""

    def readchunk(self: Self) -> bytes:
        if not len(self._buffer) - self._buffer_pos:
            raise Exception("truncated chunk")
        self._buffer_pos = 0
        return EMPTY

    def _read_size_or_line(self: Self, size: int = -1) -> bytes:
        if size > self._position:
            size = self._position
        if size == 0:
            return bytes()

        received = 0
        needed = size - received
        while received < size:
            if self._buffer:
                buf = self._buffer
                chunk_start = self._buffer_pos
                chunk_data = buf[self._buffer_pos :]
                self._buffer = EMPTY
            else:
                buf = self.readchunk()
                chunk_start = 0
                chunk_data = buf

            needed = buf.find(EMPTY, chunk_start, chunk_start + needed)
            if len(chunk_data) > needed:
                self._buffer = buf
                self._buffer_pos = chunk_start + needed
                self._position -= len(self._buffer) - self._buffer_pos

        return b""
```

https://play.ty.dev/80ab85b0-a6d7-4aa2-8de4-b4404d6a5b4e

Check time with release is around 1 sec and in debug  it's 16 seconds for me(if you start editing the code the delay visible.)

Any line I remove reduces the check time significantly. So I left them in there.
This is the original code that took 2 seconds in release and 40 seconds in debug:
https://play.ty.dev/82349c52-7963-4388-9e45-a65a90032951

From the traces I found that there is a cycle event on `member_lookup_with_policy`.
TRACE ty_project::db: Salsa event: Event { thread_id: ThreadId(2), kind: WillIterateCycle { database_key: member_lookup_with_policy_(Id(f4e2)), iteration_count: IterationCount(1), fell_back: false } }

I haven't done a good investigation yet, I'm creating this issue so it's not lost. I'll do more investigation in a few days.

---

The original files that uncovered this issue:
https://github.com/mongodb/mongo-python-driver/blob/65f7c542088356bba78bd70d68b7a4881cab7f8b/gridfs/synchronous/grid_file.py#L1622
https://github.com/mongodb/mongo-python-driver/blob/65f7c542088356bba78bd70d68b7a4881cab7f8b/gridfs/asynchronous/grid_file.py#L1634

### Version

f76d3f87c 2025-07-02

---

_Label `performance` added by @carljm on 2025-07-02 20:57_

---

_Label `attribute access` added by @carljm on 2025-07-02 20:58_

---

_Comment by @sharkdp on 2025-08-20 12:13_

Thank you for reporting this! I am looking into this today. Just writing down what I have so far.

I attempted to further minimize the example, but it's hard (see above: *"Any line I remove reduces the check time significantly"*). Here is what I have. I added a few more `if size == …` checks to make the execution times even longer:

```py
class GridOut:
    def __init__(self: "GridOut") -> None:
        self._buffer = b""

    def _read_size_or_line(self: "GridOut", size: int = -1):
        if size > self._position:
            size = self._position
            pass
        if size == 0:
            return bytes()
        if size == 1:
            return bytes()
        if size == 2:
            return bytes()
        if size == 3:
            return bytes()

        while size > 0:
            if self._buffer:
                buf = self._buffer
                self._buffer = b""
            else:
                buf = b""

            if len(buf) > size:
                self._buffer = buf
                self._position -= len(self._buffer)


reveal_type(GridOut()._buffer)
reveal_type(GridOut()._position)
```

What's very weird: if the order of the two `reveal_type` statements is changed, i.e. if we query the type of the `_position` member first, ty finishes instantly. However, if `_buffer` is accessed first (which depends on `_position` in complicated ways), we have this long execution time.

---

_Comment by @sharkdp on 2025-08-20 12:28_

My current assumption is that this is very similar to the case that was patched in https://github.com/astral-sh/ruff/pull/18669; only that there are now actual bindings. Back then, I wrote …

> This is not a fix for cases where there are actual bindings in the method. When we add self.a = 1; self.b = 1 to that example above, we still see that combinatorial explosion of runtime. […] I will open a ticket to track that separately.

… but then failed to create that ticket. I assume this ticket here is basically the same (just much more convoluted): implicit instance attributes whose definitions are guarded by complex reachability constraints involving those same instance attributes.

So I guess the easier example to reproduce this is:
```py
class C:
    def f(self: "C"):
        if isinstance(self.a, str):
            return

        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return

        self.a = 1
        self.b = 1
```

---

_Renamed from "Slow execution for attribute assignment and access with narrowing constraints" to "Slow execution for attribute assignment and access with reachability constraints" by @sharkdp on 2025-08-20 16:17_

---

_Assigned to @sharkdp by @sharkdp on 2025-08-22 13:58_

---

_Closed by @sharkdp on 2025-08-28 14:25_

---

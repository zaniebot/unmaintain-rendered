---
number: 17699
title: "`PTH*`: Incorrect suggestion to use `Pathlib` when using file descriptors"
type: issue
state: open
author: MichaReiser
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-04-29T06:10:40Z
updated_at: 2025-04-30T19:38:06Z
url: https://github.com/astral-sh/ruff/issues/17699
synced_at: 2026-01-07T13:12:16-06:00
---

# `PTH*`: Incorrect suggestion to use `Pathlib` when using file descriptors

---

_Issue opened by @MichaReiser on 2025-04-29 06:10_

Multiple pathlib rules recommend the use of `pathlib.Path` over their `os` equivalent even when they're used with a bytes string or directory descriptor which aren't supported by pathlib. 

One such example is

```py
import os
os.listdir(1)
```


```
$ ruff --version
ruff 0.11.7
$ ruff check --isolated --select PTH208 t.py
t.py:2:1: PTH208 Use `pathlib.Path.iterdir()` instead.
  |
1 | import os
2 | os.listdir(1)
  | ^^^^^^^^^^ PTH208
  |

Found 1 error.
```


Past instances of the same or similar error:
* https://github.com/astral-sh/ruff/issues/7620
* https://github.com/astral-sh/ruff/issues/12871


---

_Label `bug` added by @MichaReiser on 2025-04-29 06:10_

---

_Label `rule` added by @MichaReiser on 2025-04-29 06:10_

---

_Label `help wanted` added by @MichaReiser on 2025-04-29 06:10_

---

_Comment by @LaBatata101 on 2025-04-29 13:08_

I can take all of those, but if anyone wants to take any of the others, feel free to.  I'll start with #17691.

---

_Comment by @sbrudenell on 2025-04-29 21:11_

I wrote the linked sub-issues.

IMO file descriptor usage shouldn't be conflated with `bytes` usage.

It's still valid to suggest `pathlib.Path` in place of a non-`pathlib` function used with a `bytes` path.

It's not the case that `pathlib` somehow supports `str` and doesn't support `bytes`. It actually just expects its arguments to be `fsdecode()`'ed strings.

That's to say, IMO the following *should* error:
```python
open(b"foo")  # *should* error and suggest Path(fsdecode(b"foo")).open()
```

In #17705 I see that `open(b"foo")` is added as a test.

This is a philosophical stance. In my experience, explicitly calling `fsdecode()` makes it clear where possible platform path encoding issues might arise.

File descriptors are a different access pattern that just doesn't have an analog in `pathlib`.

---

_Comment by @ntBre on 2025-04-29 21:31_

Ah I see, I just tested `Path(b"foo")` and concluded that it should be included here because it also causes an error.

It does seem reasonable to me still to flag the bytes case if we update the rule docs with an example using `fsdecode`, but I'm curious what @MichaReiser and @LaBatata101 think too.

Thanks for opening the issues and following up here!

---

_Comment by @LaBatata101 on 2025-04-29 22:17_

I think that's reasonable, we could even provide a fix for that.

---

_Comment by @MichaReiser on 2025-04-30 06:34_

Thanks @sbrudenell for the added context. I only read the pathlib documentation and it stated that byte strings aren't supported. 

Let's only change the file descriptor case for now. We need to be more careful about byte strings if we start providing fixes (although they have to be unsafe in most cases anyway because we can't tell in many instances if a path is a file descriptor, string or byte string)

---

_Renamed from "`PTH*`: Incorrect suggestion to use `Pathlib` when using file descriptors or byte strings" to "`PTH*`: Incorrect suggestion to use `Pathlib` when using file descriptors" by @MichaReiser on 2025-04-30 06:34_

---

_Comment by @sbrudenell on 2025-04-30 19:38_

+1 for `PTH*` to *not* apply fixes.

I'll mention that part of the type safety of `Path` over `str` is that it's effectively a strong type for the filesystem encoding. If you see a `Path`, it must (or should!) have been derived from `fsdecode()`, *not* `bytes.decode()`.

I often find that using `Path` reminds me to check *other, upstream parts of my codebase* for encoding issues.

Since converting `os` calls to `pathlib` is so instructive for humans, I would find it counterproductive for the machine to do it.

---

_Referenced in [astral-sh/ruff#17767](../../astral-sh/ruff/pulls/17767.md) on 2025-05-01 13:37_

---

_Referenced in [astral-sh/ruff#17712](../../astral-sh/ruff/pulls/17712.md) on 2025-05-01 13:57_

---

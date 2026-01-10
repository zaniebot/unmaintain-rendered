---
number: 2250
title: Incomplete function argument inlay hint for unpacked iterable
type: issue
state: closed
author: phistep
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-28T23:17:00Z
updated_at: 2025-12-30T22:52:48Z
url: https://github.com/astral-sh/ty/issues/2250
synced_at: 2026-01-10T01:51:14Z
---

# Incomplete function argument inlay hint for unpacked iterable

---

_Issue opened by @phistep on 2025-12-28 23:17_

### Summary

```py
def foo(a: str, b: int, c: int, d: str): ...

t: tuple[int, int] = (23, 42)
foo("foo", *t, d="bar")
```
shows 
```py
foo(a="foo", b=*t, d="bar")
```
when it would have to be something like
```py
foo(a="foo", a,b=*t, d="bar)
```

<img width="414" height="135" alt="Image" src="https://github.com/user-attachments/assets/4f9a0912-b191-4b29-8f20-1ba60501b167" />

This is similar to #1985 but appears even without any overloads.

### Version

```
~/Library/Application Support/Zed/languages/ty/ty-0.0.7/ty-aarch64-apple-darwin/ty
```

Bundled with Zed `0.217.3+stable.105.80433cb239e868271457ac376673a5f75bc4adb1`

---

_Renamed from "Wrong function argument inlay hint for unpacked iterable" to "Incomplete function argument inlay hint for unpacked iterable" by @phistep on 2025-12-28 23:18_

---

_Comment by @MichaReiser on 2025-12-29 10:40_

Thanks for the report. yes, this looks like a bug

---

_Label `bug` added by @MichaReiser on 2025-12-29 10:40_

---

_Label `server` added by @MichaReiser on 2025-12-29 10:40_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-29 10:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-29 18:03_

---

_Closed by @charliermarsh on 2025-12-29 18:25_

---

_Comment by @phistep on 2025-12-30 22:52_

Thanks for the swift reply and fix. Amazing! 🙏 

In the PR https://github.com/astral-sh/ruff/pull/22286 you write

> We could implement support for showing multiple argument names, though this seems to match PyCharm.

No hint is better than a wrong one, I agree. But I think we (ahem as in _you folks_) could be doing better than Pycharm ;) In particular with many args I think a comprehensive hint might be very valuable in this situation, maybe even more so than with passing the args one-by-one. Imagine a situation where you do

```py
func(some_arg, *unpack_some, *unpack_some_more, some_other_arg, **unpack_kwargs, regular="kwarg")
```

it might be very useful to show which args are passed how, given that the length of `unpack_some` and `unpack_some_more` are known. Same wouold be true for `unpack_kwargs` if it is a `TypedDict`.

I understand this is a much more complicated issue and would be a feature request. Do you want me to file a proper new ticket or re-open this one?

---

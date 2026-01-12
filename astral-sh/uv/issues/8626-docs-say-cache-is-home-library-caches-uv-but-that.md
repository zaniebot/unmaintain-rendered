```yaml
number: 8626
title: Docs say cache is $HOME/Library/Caches/uv but that may not be correct
type: issue
state: closed
author: simonw
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-10-28T01:14:46Z
updated_at: 2024-10-28T03:23:14Z
url: https://github.com/astral-sh/uv/issues/8626
synced_at: 2026-01-12T15:59:30Z
```

# Docs say cache is $HOME/Library/Caches/uv but that may not be correct

---

_@simonw_

The docs here:

https://github.com/astral-sh/uv/blob/be92a2bbefffa26512f4f5b561edd2fc27c2886f/docs/reference/cli.md#L90-L92

Say that the cache directory...

> Defaults to <code>$HOME/Library/Caches/uv</code> on macOS

But... I had a look on my Mac in `~/Library/Caches` and there was no `uv` folder.

What I **did** find was this:

    /Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv

So either the documentation here needs updating or there's some kind of weird bug going on, or I'm missing something else.

---

_Comment by @simonw on 2024-10-28 01:17_

I found what looks to be the real `uv` cache directory here:

    /Users/simon/.cache/uv

That weird `com.apple.python` one turned out to be mostly empty:

```bash
find /Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv
```
```
/Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv
/Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv/archive-v0
/Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv/archive-v0/0VSKRv7KPsU6OVxKhRKyA
/Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv/archive-v0/0VSKRv7KPsU6OVxKhRKyA/lib
/Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv/archive-v0/0VSKRv7KPsU6OVxKhRKyA/lib/python3.9
/Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv/archive-v0/0VSKRv7KPsU6OVxKhRKyA/lib/python3.9/site-packages
/Users/simon/Library/Caches/com.apple.python/Users/simon/Library/Caches/uv/archive-v0/0VSKRv7KPsU6OVxKhRKyA/lib/python3.9/site-packages/_virtualenv.cpython-39.pyc
```
This one was VERY full:
```
find /Users/simon/.cache/uv | wc -l
```
```
  127275
```
```bash
du -h ~/.cache/uv | tail -n 1
```
```
3.7G	/Users/simon/.cache/uv
```

---

_Comment by @zanieb on 2024-10-28 01:19_

We switched to XDG in https://github.com/astral-sh/uv/pull/5806 — looks like we forgot to update this doc. It _can_ be there for backwards compatibility reasons, but new installs use the XDG standard location.

---

_Label `documentation` added by @zanieb on 2024-10-28 01:19_

---

_Label `good first issue` added by @zanieb on 2024-10-28 01:19_

---

_Comment by @simonw on 2024-10-28 01:24_

Also useful (for other people exploring this):

    uv cache dir

On my machine outputs:

    /Users/simon/.cache/uv


---

_Closed by @zanieb on 2024-10-28 03:23_

---

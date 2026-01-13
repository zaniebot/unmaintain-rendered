```yaml
number: 17437
title: "How to request for the GIL version of Python in `.python-version`?"
type: issue
state: open
author: AngheloAlf
labels:
  - question
assignees: []
created_at: 2026-01-13T11:51:34Z
updated_at: 2026-01-13T21:24:42Z
url: https://github.com/astral-sh/uv/issues/17437
synced_at: 2026-01-13T21:36:34Z
```

# How to request for the GIL version of Python in `.python-version`?

---

_@AngheloAlf_

### Question

I would like to use the GIL version of Python instead of the freethreaded one in my project, but `uv` insists in picking the free threaded version instead and I don't see a way of changing it.

I want to use Python GIL because some of the dependencies of my project do not provide wheels for freethreaded Python but do provide wheels for GIL, which is bad because it introduces a dependency on other compilers for other languages in a project that doesn't use them directly at all.

According to https://github.com/astral-sh/uv/issues/16722 this is a feature, and they recommend adding `+gil` to the version, but `.python-version` doesn't seem to recognize `3.14+gil`:

```bash
$ cat .python-version 
3.14+gil
$ uv sync
error: No interpreter found for Python 3.14+gil in managed installations or search path
```

So this leads to the question, how do I tell `uv` I want `3.14+gil` instead of `3.14t` even if I have `3.14t` installed in my `PATH`, by using the `.python-version` file?


### Platform

Pop!_OS 24.04 (Linux 6.17.9-76061709-generic x86_64 GNU/Linux)

### Version

uv 0.9.24

---

_Label `question` added by @AngheloAlf on 2026-01-13 11:51_

---

_Comment by @zanieb on 2026-01-13 21:24_

Thanks for the report, this is just a bug in our Python download matching logic.

---

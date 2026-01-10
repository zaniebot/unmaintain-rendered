---
number: 11518
title: Provide --with-pydebug builds of Python
type: issue
state: open
author: nickelpro
labels:
  - enhancement
assignees: []
created_at: 2025-02-14T19:40:46Z
updated_at: 2025-02-14T20:28:26Z
url: https://github.com/astral-sh/uv/issues/11518
synced_at: 2026-01-10T01:25:06Z
---

# Provide --with-pydebug builds of Python

---

_Issue opened by @nickelpro on 2025-02-14 19:40_

### Summary

When developing CPython extensions it is very useful to develop and test on non-optimized, `--with-pydebug` builds of Python. The set of asserts enabled by such builds provide early warning of a plethora of otherwise tricky to detect bugs, most notably bad reference counting.

Doing so today requires developers to download and compile all relevant versions, and somehow make those versions available to their virtual environment managers (you typically don't want these in `PATH`). `uv` could simplify this workflow greatly for most use-cases.

`uv` already provides a free-threaded variant via the `t` suffix for freethreaded python, see https://github.com/astral-sh/python-build-standalone/issues/320

The same could be provided for `--with-pydebug` builds via `d` suffixed versions

### Example

```
uv python install 3.13d
```

---

_Label `enhancement` added by @nickelpro on 2025-02-14 19:40_

---

_Comment by @zanieb on 2025-02-14 19:46_

Related https://github.com/astral-sh/uv/issues/11387

Notably this differs from the "I just want debug symbols" use-case cc @geofft  

---

_Comment by @zanieb on 2025-02-14 20:25_

We were about half-way there on this so I pushed some progress https://github.com/astral-sh/uv/pull/11520 — will discuss more the state of it there but if you're interested in contributing it could use some help.

---

_Comment by @nickelpro on 2025-02-14 20:28_

Ah crap, I promise I tried to search for extant issues. Thanks for the rapid response.

I'll give a look through of the existing work and discuss in the MR if there's anything I think is a problem or can help with.

---

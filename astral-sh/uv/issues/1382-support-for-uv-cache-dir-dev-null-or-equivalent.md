```yaml
number: 1382
title: "Support for `UV_CACHE_DIR=/dev/null` or equivalent"
type: issue
state: closed
author: hauntsaninja
labels:
  - enhancement
assignees: []
created_at: 2024-02-15T23:15:00Z
updated_at: 2024-02-15T23:36:54Z
url: https://github.com/astral-sh/uv/issues/1382
synced_at: 2026-01-10T05:40:31Z
```

# Support for `UV_CACHE_DIR=/dev/null` or equivalent

---

_Issue opened by @hauntsaninja on 2024-02-15 23:15_

It would be useful to do the equivalent of `uv pip install --no-cache` via env var

---

_Comment by @zanieb on 2024-02-15 23:15_

Like specifically to another location or to disable it entirely? Is there an explicit use-case where this is important?

---

_Label `enhancement` added by @zanieb on 2024-02-15 23:15_

---

_Comment by @hauntsaninja on 2024-02-15 23:16_

To disable it entirely, I think to another location currently works

---

_Comment by @zanieb on 2024-02-15 23:17_

It'd be nice to be cross-platform so I think `UV_NO_CACHE` makes more sense than changing the value of the directory and trying to guess what value we want?

---

_Comment by @hauntsaninja on 2024-02-15 23:22_

Yeah, that's the better UI

In a world where hard links were commonly used for Python packaging I'm not sure I would have a use case. But my current use case is that I have some code that builds a container where for reasons I currently feel most comfortable using `--link-mode=copy` and I'd prefer to not end up with two copies and various things are wrapped by scripts so chaining command lines is not trivial. More generally, this is a env var pip has, so if I can get similar behaviour when using `uv` it reduces FUD when porting

---

_Comment by @charliermarsh on 2024-02-15 23:23_

>  But my current use case is that I have some code that builds a container where for reasons I currently feel most comfortable using --link-mode=copy and I'd prefer to not end up with two copies

This part is also interesting, maybe worth considering better support for this workflow in `uv`.

---

_Comment by @hauntsaninja on 2024-02-15 23:34_

> maybe worth considering

I'm really glad uv defaults to reflink / hard links. I just have a little FUD about some things specific to my codebase, so not sure how strongly you should consider that specific bit. Definitely consider containers though!

I do generally like that pip exposes a lot of its configuration via env vars. I think that was a good choice for a low-ish level tool

---

_Closed by @zanieb on 2024-02-15 23:34_

---

_Comment by @hauntsaninja on 2024-02-15 23:36_

Thank you! :-)

---

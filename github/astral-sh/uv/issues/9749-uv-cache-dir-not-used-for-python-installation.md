---
number: 9749
title: UV_CACHE_DIR not used for Python installation
type: issue
state: closed
author: jheiselman
labels:
  - needs-decision
assignees: []
created_at: 2024-12-09T20:50:40Z
updated_at: 2024-12-10T19:20:41Z
url: https://github.com/astral-sh/uv/issues/9749
synced_at: 2026-01-07T13:12:18-06:00
---

# UV_CACHE_DIR not used for Python installation

---

_Issue opened by @jheiselman on 2024-12-09 20:50_

On Windows, when running a `uv python install` while having UV_CACHE_DIR set, does not result in uv using that cache location. It instead uses the default location %APPDATA%\uv\python\.cache.

`uv python install` should respect UV_CACHE_DIR. This is especially useful when using a DevDrive and have security tools set to either ignore or be more lenient with DevDrives.

Platform: Windows 10
uv version: 0.5.7

Debug log attached. It doesn't show it, but my UV_CACHE_DIR is set to E:\Cache\uv

[uv-debug.log](https://github.com/user-attachments/files/18067883/uv-debug.log)


---

_Comment by @charliermarsh on 2024-12-09 22:20_

@zanieb -- Should I change this?

---

_Label `needs-decision` added by @charliermarsh on 2024-12-09 22:21_

---

_Comment by @zanieb on 2024-12-09 23:06_

We changed this in https://github.com/astral-sh/uv/pull/6043 because of #6036

We don't really cache installs right now, this is just the location we download to to ensure the install is idempotent. Can you explain a bit more why this matters to you?

I would like to cache downloads anyway (per https://github.com/astral-sh/uv/issues/8025) in which we'd use the `UV_CACHE_DIR` but then we'd also presumably need to switch to hardlinking and a copy fallback rather than renaming.

---

_Comment by @charliermarsh on 2024-12-09 23:46_

Totally forgot about that change, whoops.

---

_Comment by @jheiselman on 2024-12-09 23:56_

It matters for two reasons. 

1. As already stated, with DevDrives being a thing, cache location is becoming important for security tool configuration. 
2. As a matter of the tool acting in an intuitive manner, I assumed, and figure others might as well, that if I set the cache directory it is used for any and all caching. It is counterintuitive, to me at least, that should not be respected and caused me some troubleshooting time tracking down what I may have done wrong. 

---

_Comment by @zanieb on 2024-12-10 00:07_

It's not a cache though, we are just downloading the file there and moving it into the install location. There is no persistence or caching. It's a misnomer.

---

_Comment by @charliermarsh on 2024-12-10 00:08_

Yeah it's more like a scratch directory.

---

_Referenced in [astral-sh/uv#9756](../../astral-sh/uv/pulls/9756.md) on 2024-12-10 00:15_

---

_Comment by @zanieb on 2024-12-10 00:17_

Regardless, can you expand on

>  cache location is becoming important for security tool configuration

I still don't follow what precise issue you're talking about here. Why is it important? What happens?

---

_Comment by @jheiselman on 2024-12-10 01:18_

I’m not sure how familiar you are with the concept of DevDrives has Microsoft had defined them, but they are mountable volume within Windows where you would keep code caches, git repo clones, and other things that may trigger a lot of churn with enterprise security tools (AV, heuristic scanners, IDS, etc…). They are then pitching to desktop security teams to have all developers keep things isolated to these DevDrives so that they can simplify exceptions and whatnot to these locations. 

Since uv downloads a copy of python, it may be a good fit to have it also use the DevDrives system. Maybe not. In either case, the confusion of having it named cache is what threw me off. If it’s not really a cache, then perhaps this issue should become about changing the name of the directory to avoid confusion.  

---

_Comment by @zanieb on 2024-12-10 02:57_

Yep we're familiar with DevDrives (we use them in CI to improve the GitHub Actions perf!)

> Since uv downloads a copy of python, it may be a good fit to have it also use the DevDrives system.

Makes sense, I think you want to change [`UV_PYTHON_INSTALL_DIR`](https://docs.astral.sh/uv/configuration/environment/#uv_python_install_dir) instead.

I've changed the name of the directory in #9756 

---

_Comment by @jheiselman on 2024-12-10 03:23_

Excellent! Thank you. I’ll give it a try tomorrow. 

---

_Comment by @jheiselman on 2024-12-10 19:19_

This works for me. Thank you.

---

_Closed by @zanieb on 2024-12-10 19:20_

---

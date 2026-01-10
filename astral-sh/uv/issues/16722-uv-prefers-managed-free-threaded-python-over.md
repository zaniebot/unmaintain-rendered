---
number: 16722
title: uv prefers managed free-threaded Python over unmanaged non-freethreaded Python
type: issue
state: open
author: lengau
labels:
  - bug
assignees: []
created_at: 2025-11-13T14:30:33Z
updated_at: 2025-11-15T13:37:05Z
url: https://github.com/astral-sh/uv/issues/16722
synced_at: 2026-01-10T01:26:09Z
---

# uv prefers managed free-threaded Python over unmanaged non-freethreaded Python

---

_Issue opened by @lengau on 2025-11-13 14:30_

### Summary

I have a non-freethreaded copy of Python 3.14 installed from my distro and free-threaded Python 3.14 installed managed with uv:

```
lengau@ratel:~$ uv python list
cpython-3.15.0a1-linux-x86_64-gnu                 <download available>
cpython-3.15.0a1+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.14.0-linux-x86_64-gnu                   /usr/bin/python3.14
cpython-3.14.0-linux-x86_64-gnu                   <download available>
cpython-3.14.0+freethreaded-linux-x86_64-gnu      .local/bin/python3.14t -> .local/share/uv/python/cpython-3.14.0+freethreaded-linux-x86_64-gnu/bin/python3.14t
cpython-3.14.0+freethreaded-linux-x86_64-gnu      .local/share/uv/python/cpython-3.14.0+freethreaded-linux-x86_64-gnu/bin/python3.14t
cpython-3.13.9-linux-x86_64-gnu                   /usr/bin/python3.13
cpython-3.13.9-linux-x86_64-gnu                   /usr/bin/python3 -> python3.13
cpython-3.13.9-linux-x86_64-gnu                   /usr/bin/python -> python3
cpython-3.13.9-linux-x86_64-gnu                   <download available>
cpython-3.13.9+freethreaded-linux-x86_64-gnu      <download available>
...
```

When I run `uv sync --python=3.14`, it prefers the freethreaded Python that's uv-managed over the distro-managed Python 3.14. However, since I don't have freethreaded Python 3.13 downloaded, it correctly uses the distro copy of that.

So if I delete the virtual environment in a repo, I get:

```
$ uv sync --python=3.13  # uses /usr/bin/python3.13
$ uv sync --python=3.14  # uses managed python3.14t
$ uv sync --python=3.14t # uses managed python3.14t
```

This also results in a weird situation when I install the non-freethreaded version of Python 3.14 managed:


```
$ uv sync --python=3.13  # uses /usr/bin/python3.13
$ uv sync --python=3.14  # uses managed python3.14
$ uv sync --python=3.14t # uses managed python3.14t
$ uv sync --python=3.14  # **remains** on managed python3.14t
```

The behaviour I would expect here is that swapping from python 3.14t to 3.14 would switch to the non-freethreaded version (or that we have a way to specify the non-freethreaded version even with both being managed).

## Workaround

The workaround here is pretty easy, though somewhat inconvenient: specify the specific Python binary by getting the value from `uv python list`. E.g. `uv sync --python=/usr/bin/python3.14`

### Platform

Kubuntu 26.04 amd64

### Version

uv 0.9.9 (4fac4cb7e 2025-11-12)  # (Installed from the snap)

### Python version

n/a

---

_Label `bug` added by @lengau on 2025-11-13 14:30_

---

_Comment by @FishAlchemist on 2025-11-15 02:37_

Actually, this is the expected behavior, not a bug. 
#### The relevant information is as follows:

Changelog for Version 0.9.0
> Previously, free-threaded variants of Python were considered experimental and required explicit opt-in (i.e., with 3.14t) for usage. Now uv will allow use of free-threaded Python 3.14+ interpreters without explicit selection. The GIL-enabled build of Python will still be preferred, e.g., when performing an installation with uv python install 3.14. However, e.g., if a free-threaded interpreter comes before a GIL-enabled build on the PATH, it will be used. This change does not apply to free-threaded Python 3.13 interpreters, which will continue to require opt-in.

 * https://github.com/astral-sh/uv/pull/16142

Changelog for Version 0.9.8
* https://github.com/astral-sh/uv/pull/16537

Therefore, you should submit a feature request to allow UV to change the default selection behavior.

---

_Comment by @zanieb on 2025-11-15 13:37_

Yeah @FishAlchemist is right that this is intended, though I'm sort of regretting the change.

Does using `+gil` work for you?

---

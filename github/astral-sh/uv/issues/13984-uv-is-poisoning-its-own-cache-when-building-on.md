---
number: 13984
title: uv is poisoning its own cache when building on Rosetta systems
type: issue
state: closed
author: hynek
labels:
  - question
  - external
assignees: []
created_at: 2025-06-12T08:13:41Z
updated_at: 2025-07-04T02:14:50Z
url: https://github.com/astral-sh/uv/issues/13984
synced_at: 2026-01-07T13:12:18-06:00
---

# uv is poisoning its own cache when building on Rosetta systems

---

_Issue opened by @hynek on 2025-06-12 08:13_

### Summary

I'm pretty sure this is a new-ish problem.

So, I develop on an ARM Mac with a Rosetta system and use Python both in x86 and ARM mode.

Recently-ish, I've started to have the problem that I have to clear the cache (`uv cache clean python-ldap`) when recreating a project virtualenv (`rm -rf .venv; uv sync`), for packages that need to be compiled.

`python-ldap==3.4.4` does not ship any wheels, so it has to be compiled for every architecture and every Python version, otherwise you get an opaque:

```
.venv/lib/python3.13/site-packages/ldap/__init__.py:34: in <module>
    import _ldap
E   ImportError: dynamic module does not define module export function (PyInit__ldap)
```

I guess the problem is that cached self-built wheels are shared across architectures?

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

### Python version

3.13.5

---

_Label `bug` added by @hynek on 2025-06-12 08:13_

---

_Comment by @konstin on 2025-06-12 08:14_

What wheel tag does python-ldap build to?

---

_Comment by @hynek on 2025-06-12 08:22_

Looks like `python_ldap-3.4.4-cp313-cp313-macosx_10_13_universal2.whl` which is clearly wrong, but I don't think they're doing it on purpose?

- https://github.com/python-ldap/python-ldap/blob/python-ldap-3.4.3/setup.cfg
- https://github.com/python-ldap/python-ldap/blob/python-ldap-3.4.3/pyproject.toml
- https://github.com/python-ldap/python-ldap/blob/python-ldap-3.4.3/setup.py

Either way, given the last release was in 2023, I wouldn't rely on them fixing it. 😬

---

_Comment by @konstin on 2025-06-12 08:25_

Given that they use the `universal2` tag, they are explicitly declaring that they are compatible with x86_64 and aarch64 systems, so uv has to assume it can reuse the wheel; The current behavior sounds correct to me.

---

_Label `bug` removed by @konstin on 2025-06-12 08:25_

---

_Label `question` added by @konstin on 2025-06-12 08:25_

---

_Comment by @hynek on 2025-06-12 08:50_

_Are_ they using the `universal2` tag, or is it some fallback default?

---

_Comment by @konstin on 2025-06-12 08:58_

That is the build backend using the tag, uv is not involved in deciding the tag, that's all on the side of the package.

---

_Label `external` added by @zanieb on 2025-06-12 13:43_

---

_Referenced in [python-ldap/python-ldap#586](../../python-ldap/python-ldap/issues/586.md) on 2025-06-13 08:57_

---

_Closed by @charliermarsh on 2025-07-04 02:14_

---

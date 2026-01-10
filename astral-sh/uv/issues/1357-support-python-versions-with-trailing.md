---
number: 1357
title: "Support Python versions with trailing `+`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-02-15T21:30:23Z
updated_at: 2024-02-20T17:57:29Z
url: https://github.com/astral-sh/uv/issues/1357
synced_at: 2026-01-10T01:23:05Z
---

# Support Python versions with trailing `+`

---

_Issue opened by @charliermarsh on 2024-02-15 21:30_

It was reported that a locally-built Python with `3.11.4+` errors out in our version parser:

https://gist.github.com/jsbueno/be1797cfe654c69245ddbea78e0360ff

---

_Label `bug` added by @charliermarsh on 2024-02-15 21:30_

---

_Comment by @BurntSushi on 2024-02-15 21:44_

The [Local Version Identifiers section of the Version Specifiers spec](https://packaging.python.org/en/latest/specifications/version-specifiers/#local-version-identifiers) seems to suggest that a local label needs to be non-empty, but it doesn't seem to come out and say it explicitly. With that said, the [regex for parsing version numbers](https://packaging.python.org/en/latest/specifications/version-specifiers/#appendix-parsing-version-strings-with-regular-expressions) does unambiguously require a non-empty local version segment if a `+` is present.

So at least with respect to the spec, it seems like the version number here is invalid. Is there any particular reason why `3.11.4+` is being used? If it's just a mistake, then perhaps that should be fixed instead.

But if this is a widespread or common practice, then perhaps there isn't too much harm in making our version parser slightly more flexible. I think the point here though would be to decide what the semantics of `3.11.4+` are. Is the version identical to `3.11.4`? And in particular, if someone gave us a `3.11.4+` version and we spit `3.11.4` back out at some point, would that be an issue? The question might seem strange, but it's the difference between a "version without a local segment" and a "version with an empty local segment." (The current representation of a `Version` doesn't distinguish between those cases.)

---

_Referenced in [astral-sh/uv#1402](../../astral-sh/uv/issues/1402.md) on 2024-02-16 00:29_

---

_Comment by @hauntsaninja on 2024-02-16 01:10_

I think the Python version isn't governed by PEP 440.

3.11.4+ is if you build off of random CPython commit. I think it comes from e.g. https://github.com/python/cpython/blob/c08c0679055d96c0397cf128bf7cc8134538b36a/Include/patchlevel.h#L26

Should be fine to treat it as 3.11.4

---

_Comment by @zanieb on 2024-02-16 01:14_

They seem semantically different; I would treat `3.11.4+` as fulfilling versions that are not pinned. It's rare that we request e.g.  `==3.11` or `==3.11.4`

---

_Comment by @charliermarsh on 2024-02-16 01:33_

What's the issue with treating this as equivalent to parsing `3.11.4`?

---

_Comment by @edgarrmondragon on 2024-02-16 01:43_

I second @hauntsaninja. The `+` is usually added to Python builds that are built off the `main` branch rather than a tag:

```
>>> import pprint
>>> from packaging.markers import default_environment
>>> pprint.pprint(default_environment())
{'implementation_name': 'cpython',
 'implementation_version': '3.13.0a3',
 'os_name': 'posix',
 'platform_machine': 'arm64',
 'platform_python_implementation': 'CPython',
 'platform_release': '23.4.0',
 'platform_system': 'Darwin',
 'platform_version': 'Darwin Kernel Version 23.4.0: Wed Feb  7 23:21:07 PST '
                     '2024; root:xnu-10063.100.637.501.2~2/RELEASE_ARM64_T8112',
 'python_full_version': '3.13.0a3+',
 'python_version': '3.13',
 'sys_platform': 'darwin'}
```

---

_Comment by @charliermarsh on 2024-02-16 01:48_

Makes sense. I'd also be open to just stripping a trailing `+` for those fields specifically... @BurntSushi, what do you think?

---

_Comment by @zanieb on 2024-02-16 02:03_

They're different versions — the `+` has meaning. Similar to `1.0.0rc`being different from `1.0.0`. I don't think treating them as distinct versions would cause any problems for our usages of Python versions though. If we treat them as different you could e.g. require a local version by requesting `3.13.0+`, but if you request `3.13` it should work fine even if `3.13.0+` is what we find.

---

_Referenced in [astral-sh/uv#1478](../../astral-sh/uv/issues/1478.md) on 2024-02-16 11:53_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-16 15:17_

---

_Comment by @konstin on 2024-02-20 12:10_

`python_full_version` needs to be a PEP 440 compliant version, otherwise environment markers break ([PEP 508 environment marker comparisons](https://peps.python.org/pep-0508/#environment-markers)). Depending on your interpretation of PEP 508, we would fall back to stringly comparisons for `python_full_version` where '3.9.0' is lower than '3.11.4+'.

There's an upstream cpython bug: https://github.com/python/cpython/issues/99968

---

_Referenced in [python/cpython#99968](../../python/cpython/issues/99968.md) on 2024-02-20 16:48_

---

_Referenced in [astral-sh/uv#1771](../../astral-sh/uv/pulls/1771.md) on 2024-02-20 17:35_

---

_Closed by @BurntSushi on 2024-02-20 17:57_

---

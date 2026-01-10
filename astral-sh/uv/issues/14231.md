```yaml
number: 14231
title: "error: No solution found when resolving dependencies for split"
type: issue
state: closed
author: MarkusSintonen
labels:
  - bug
assignees: []
created_at: 2025-06-24T08:31:17Z
updated_at: 2025-07-01T15:48:49Z
url: https://github.com/astral-sh/uv/issues/14231
synced_at: 2026-01-10T03:32:45Z
```

# error: No solution found when resolving dependencies for split

---

_Issue opened by @MarkusSintonen on 2025-06-24 08:31_

### Summary

Run following and it produces an error:
```
uv init --name myproj --python 3.13
uv add aiohttp==3.12.13
# Fails
uv add aiohttp-asgi-connector==1.1.2
```

It gives:
```
  × No solution found when resolving dependencies for split (python_full_version >= '3.9'
  │ and python_full_version < '4'):
  ╰─▶ Because only the following versions of aiohttp{python_full_version < '4'} are
      available:
          aiohttp{python_full_version < '4'}<=3.1.0
          aiohttp{python_full_version < '4'}==3.1.1
          aiohttp{python_full_version < '4'}==3.1.2
          ... long list of versions ...
          aiohttp{python_full_version < '4'}==3.10.10
          aiohttp{python_full_version < '4'}==3.10.11
          aiohttp{python_full_version < '4'}>3.11
      and aiohttp{python_full_version < '4'}==3.8.2 was yanked (reason: This version
      includes overly restrictive multidict upper boundary disallowing multidict v6+. The
      previous patch version didn't have that and this is now causing dependency resolution
      problems for the users who have an "incompatible" version pinned. This is not really
      necessary anymore and will be addressed in the next release v3.8.3

      https://github.com/aio-libs/aiohttp/pull/6950), we can conclude that
      aiohttp{python_full_version < '4'}>=3.1.0,<3.8.3 depends on aiohttp>=3.1.0,<=3.8.1.
      And because aiohttp-asgi-connector==1.1.2 depends on aiohttp{python_full_version <
      '4'}>=3.1.0,<3.11, we can conclude that aiohttp-asgi-connector==1.1.2 depends on one
      of:
          aiohttp>=3.1.0,<=3.8.1
          aiohttp>=3.8.3,<=3.10.11

      And because your project depends on aiohttp==3.12.13 and
      aiohttp-asgi-connector==1.1.2, we can conclude that your project's requirements are
      unsatisfiable.

      hint: Pre-releases are available for `aiohttp` in the requested range (e.g.,
      3.10.11rc0), but pre-releases weren't enabled (try: `--prerelease=allow`)
  help: If you want to add the package regardless of the failed resolution, provide the
        `--frozen` flag to skip locking and syncing.
```

I dont see anything wrong here in `aiohttp-asgi-connector`: https://github.com/thearchitector/aiohttp-asgi-connector/blob/main/pyproject.toml#L8-L9 So I suspect its a bug in UV dependency solving as UV seems to be using a wrong Python version for the constraint.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.14 (e7f596711 2025-06-23)

### Python version

Python 3.13.3

---

_Label `bug` added by @MarkusSintonen on 2025-06-24 08:31_

---

_Assigned to @konstin by @konstin on 2025-06-24 08:58_

---

_Comment by @konstin on 2025-06-24 08:59_

Minimal reproducible example:

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = [
  "anyio==4.4.0; python_full_version >= '3.11'",
  "anyio==4.5.0; python_full_version ~= '3.10.0'",
]
```

The problem is that we're converting
`anyio==4.5.0; python_full_version ~= '3.10.0'` to
`anyio{python_full_version >= '3.10' and python_full_version < '4'}>=4.5.0, <4.5.0+` instead of
`anyio{python_full_version >= '3.10' and python_full_version < '3.11'}>=4.5.0, <4.5.0+`, which is a bug in our requirements conversion.

---

_Comment by @MarkusSintonen on 2025-06-24 16:45_

Tried to find a workaround but couldn't find any. Is there a workaround?

---

_Comment by @konstin on 2025-06-24 19:48_

Yeah, you can use `aiohttp<3.11,>=3.1.0; python_version >= '3.8' and python_version < '3.9'`.

---

_Comment by @MarkusSintonen on 2025-06-25 05:45_

> Yeah, you can use `aiohttp<3.11,>=3.1.0; python_version >= '3.8' and python_version < '3.9'`.

Doesnt seem to work either. I tried following:
```
[project]
name = "myproj"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "aiohttp==3.12.13",
    "aiohttp<3.11,>=3.1.0; python_version >= '3.8' and python_version < '3.9'",
    "aiohttp-asgi-connector==1.1.2",
]
```

I'm not maintainer of these deps. So Im guessing there is no workaround then.

---

_Closed by @konstin on 2025-07-01 15:48_

---

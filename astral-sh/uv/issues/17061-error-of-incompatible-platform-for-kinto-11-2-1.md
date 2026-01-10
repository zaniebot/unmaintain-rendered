```yaml
number: 17061
title: "Error of incompatible platform for `kinto-11.2.1-cp3-none-any.whl` package"
type: issue
state: open
author: mjamroz
labels:
  - bug
  - external
assignees: []
created_at: 2025-12-10T07:26:32Z
updated_at: 2025-12-11T15:19:33Z
url: https://github.com/astral-sh/uv/issues/17061
synced_at: 2026-01-10T03:11:35Z
```

# Error of incompatible platform for `kinto-11.2.1-cp3-none-any.whl` package

---

_Issue opened by @mjamroz on 2025-12-10 07:26_

### Summary

After the last upgrade (0.9.16) we cannot install `kinto==11.2.1` package anymore as it says incompatible platform:

```
Resolved 41 packages in 1ms
      Built kinto==11.2.1
  × Failed to download and build `kinto==11.2.1`
  ╰─▶ The built wheel `kinto-11.2.1-cp3-none-any.whl` is not compatible with the current Python 3.14 on manylinux x86_64
  help: `kinto` (v11.2.1) was included because `b` (v1) depends on `kinto`
```

It is an old package (2018), maybe that time wheel naming was different/wrong. Asking authors to rename is rather no go, would be nice either include this kind of edge cases or an option to ignore that error (make it a warning?)

To recreate, try `uv sync` having `pyproject.toml` as:
```
[project]
name = "b"
version = "1"
description = "b"
authors = [{ name = "a" }]
requires-python = ">=3.14"
dependencies = [ "kinto==11.2.1" ]
```


### Platform

linux

### Version

uv 0.9.16

### Python version

Python 3.14.0

---

_Label `bug` added by @mjamroz on 2025-12-10 07:26_

---

_Comment by @mjamroz on 2025-12-10 07:29_

Also colleague noticed another (related?) issue (notice it tries to fetch macosx instead):
```
% uv sync --python-platform aarch64-unknown-linux-musl
Resolved 226 packages in 10ms
      Built psycopg2==2.9.11
  × Failed to download and build `psycopg2==2.9.11`
  ╰─▶ The built wheel `psycopg2-2.9.11-cp313-cp313-macosx_15_0_arm64.whl` is not compatible with the target Python 3.13 on musllinux aarch64. Consider using `--no-build` to disable building
      wheels.
  help: `psycopg2` (v2.9.11) was included because `...` (v117) depends on `psycopg2`
```

but `musllinux` exists:  https://mirrors.aliyun.com/pypi/simple/psycopg2-binary/


---

_Comment by @konstin on 2025-12-10 09:50_

For the first case, `cp3-none-any` is a semantically strange tag, while technically valid there isn't a proper use case for it (it says "This package needs CPython 3.x specifically, instead of just Python 3, but it doesn't use any CPython specific interface, and also works on any platform"). Newer versions of kinto fix that and use the standard `py3-none-any` tag, can you fix this by upgrading to a newer kinto version?

For the second case, the page you linked to is for `psycopg2-binary`, but the package that's being installed is `psycopg2`. The `-binary` here isn't information for a tool, it's an actual part of the package name, so uv can't replace one with the other.

---

_Comment by @mjamroz on 2025-12-11 07:02_

We cannot bump version, we need to stick to this particular version of kinto. :( But in the meantime I saw https://github.com/astral-sh/uv/pull/17064 PR which might be helpful in this case, right? 

---

_Label `external` added by @charliermarsh on 2025-12-11 15:19_

---

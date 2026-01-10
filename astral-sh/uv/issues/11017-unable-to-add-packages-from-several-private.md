---
number: 11017
title: Unable to add packages from several private sources
type: issue
state: closed
author: diskream
labels:
  - bug
assignees: []
created_at: 2025-01-28T11:28:09Z
updated_at: 2025-01-30T16:22:23Z
url: https://github.com/astral-sh/uv/issues/11017
synced_at: 2026-01-10T01:25:00Z
---

# Unable to add packages from several private sources

---

_Issue opened by @diskream on 2025-01-28 11:28_

### Summary

I'm trying to add 2 packages from 2 private repositories. If I add only one package, everything works fine and installs correctly. When I add more than one repo I'm starting to receive HTTP 404 response from my private gitlab registry:

```
Resolved 50 packages in 7ms
0.730   × Failed to download 'ecosys-lib==0.5.14.9'
0.730   ├─▶ Failed to fetch:
0.730   │   'https://private_gitlab.com/api/v4/projects/252/packages/pypi/files/74ae10ad64863b835ee2574b1c6dd3c0be6ef6c25df457282569e81922669298/ecosys_lib-0.5.14.9-py3-none-any.whl'
0.730   ╰─▶ HTTP status client error (404 Not Found) for url
0.730       (https://private_gitlab.com/api/v4/projects/252/packages/pypi/files/74ae10ad64863b835ee2574b1c6dd3c0be6ef6c25df457282569e81922669298/ecosys_lib-0.5.14.9-py3-none-any.whl)
0.730   help: 'ecosys-lib' (v0.5.14.9) was included because 'excel-service'
0.730         (v0.1.0) depends on 'ecosys-lib'
```

When I enter the link, the package downloads instantly. But I logged in, so gitlab is not forcing me to authorize. 
I can't understand why is it happening, because both sources works individually 
My pyproject:

```
[project]
name = "excel-service"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "excel-lib>=0.2.0",
    "ecosys-lib>=0.5.14.9",
]

[[tool.uv.index]]
name = "ecosys-lib"
url = "https://token:{token_one}@private_gitlab.com/api/v4/projects/252/packages/pypi/simple"
explicit = true


[[tool.uv.index]]
name = "excel-lib"
url = "https://token:{token_two}@private_gitlab.com/api/v4/projects/422/packages/pypi/simple"
explicit = true


[tool.uv.sources]
ecosys-lib = { index = "ecosys-lib" }
excel-lib = { index = "excel-lib" }
```



### Platform

macOS 14 arm64

### Version

uv 0.5.24 (Homebrew 2025-01-23)

### Python version

Python 3.13

---

_Label `bug` added by @diskream on 2025-01-28 11:28_

---

_Comment by @charliermarsh on 2025-01-28 14:50_

How are you providing the authentication? It's just hard-coded in your `pyproject.toml`?

---

_Comment by @romanzdk on 2025-01-28 14:51_

We have same issue just we do not provide any authentication - the private registry is "public" in the company network

---

_Comment by @diskream on 2025-01-28 17:30_

> How are you providing the authentication? It's just hard-coded in your `pyproject.toml`?

I tried different ways. The error appears when I provide authentication for both sources. 

I also tried git + HTTPS authentication with a token in the URL. The library installed correctly, but as mentioned in the docs, the token wasn't provided in pyproject or uv.lock. When it came time to run in Docker, I got an error with Git authentication. When I supplied a token, the same error occurred.

As temporary workaround I install one lib using `uv pip install excel-lib --index https://token:{token_two}@private_gitlab.com/api/v4/projects/422/packages/pypi/simple` right in Dockerfile, the second one remains in pyproject

---

_Comment by @zanieb on 2025-01-29 14:03_

Can you share verbose logs? Ideally with `RUST_LOG=uv=trace`? (be sure to elide any secrets)

---

_Assigned to @zanieb by @zanieb on 2025-01-29 14:03_

---

_Comment by @diskream on 2025-01-29 14:05_

> Can you share verbose logs? Ideally with `RUST_LOG=uv=trace`? (be sure to elide any secrets)

Sure. Log trace is quite long. How can I share it?

---

_Comment by @zanieb on 2025-01-29 14:27_

[Gist](https://gist.github.com) is a good option.

---

_Comment by @diskream on 2025-01-29 14:48_

[I hope this will help](https://gist.github.com/diskream/269aadaff60b63271af9b89e3aac6a27)

---

_Comment by @zanieb on 2025-01-29 15:00_

Thank you!

So there are logs like

```
TRACE Caching credentials for https://token:{token_two}@private_gitlab.com/api/v4/projects/422/packages/pypi/simple
TRACE Caching credentials for https://token:{token_one}@private_gitlab.com/api/v4/projects/252/packages/pypi/simple
TRACE No credentials in cache for URL https://private_gitlab.com/api/v4/projects/422/packages/pypi/files/57f33c110335e9ef1f0ac7dc042e9d3acff1953bab91e2c34b852f49223b2f76/excel_lib-0.2.0-cp313-cp313-musllinux_1_2_aarch64.whl
TRACE Found cached credentials for realm https://private_gitlab.com
```

Basically what's happening is the file download URL is `https://private_gitlab.com/api/v4/projects/422/packages/pypi/files/` which is not a child path of `https://token:{token_two}@private_gitlab.com/api/v4/projects/422/packages/pypi/simple` so we do not propagate credentials (for security 😬). Then when that request fails, we fallback to a _realm_ level credential store (basically the domain) which is fine if you only have one secret for that realm but in your situation you'll end up with incorrect credentials for the other project.

This would be solved by https://github.com/astral-sh/uv/issues/4583

---

_Referenced in [astral-sh/uv#11074](../../astral-sh/uv/pulls/11074.md) on 2025-01-29 17:53_

---

_Comment by @diskream on 2025-01-29 17:58_

Thank you! I'll be waiting

---

_Comment by @zanieb on 2025-01-29 18:00_

You could try https://github.com/astral-sh/uv/pull/11074 if you want

I'm not sure if that's the approach we want to take, but it should fix the problem.

---

_Closed by @zanieb on 2025-01-30 16:22_

---

_Closed by @zanieb on 2025-01-30 16:22_

---

_Referenced in [astral-sh/uv#12004](../../astral-sh/uv/issues/12004.md) on 2025-03-06 13:25_

---

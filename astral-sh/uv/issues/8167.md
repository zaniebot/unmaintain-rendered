```yaml
number: 8167
title: Package not found from Azure DevOps using a token
type: issue
state: closed
author: AndreuCodina
labels:
  - error messages
assignees: []
created_at: 2024-10-14T08:02:08Z
updated_at: 2024-10-16T17:34:30Z
url: https://github.com/astral-sh/uv/issues/8167
synced_at: 2026-01-10T04:45:10Z
```

# Package not found from Azure DevOps using a token

---

_Issue opened by @AndreuCodina on 2024-10-14 08:02_

With `uv sync`, we've tried to download the package `my-private-lib==1.0.0` from Azure DevOps, a private registry.

Using the keyring authentication, we can get the package:

```toml
[project]
name = "my-repository"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "my-private-lib==1.0.0",
]

[tool.uv]
keyring-provider = "subprocess"
extra-index-url = [
    "https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/",
]
```

But using the token we have to use in the Dockerfile and CI/CD, we can't:

```toml
[tool.uv]
keyring-provider = "disabled"
extra-index-url = [
    "https://myprivatetoken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/",
]
```

> $  uv sync --verbose

```
DEBUG uv 0.4.20
DEBUG Found project root: `/workspaces/my-repository`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/workspaces/my-repository/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG Found static `pyproject.toml` for: my-repository @ file:///workspaces/my-repository
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: my-repository*
DEBUG Searching for a compatible version of my-repository @ file:///workspaces/my-repository (*)
DEBUG Adding transitive dependency for my-repository==0.1.0: my-private-lib==1.0.0
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-lib/
DEBUG No cache entry for: https://pypi.org/simple/my-private-lib/
DEBUG Searching for a compatible version of my-private-lib (==1.0.0)
DEBUG No compatible version found for: my-private-lib
DEBUG Searching for a compatible version of my-repository @ file:///workspaces/my-repository (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: my-repository
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-lib was not found in the package registry and your project depends on my-private-lib==1.0.0, we can conclude that your
      project's requirements are unsatisfiable.
```

> $ RUST_LOG=uv=trace uv sync

```
DEBUG uv 0.4.20
DEBUG Found project root: `/workspaces/my-repository`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/workspaces/my-repository/.python-version`
TRACE Cached interpreter info for Python 3.12.7, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `Python 3.12`
TRACE Caching credentials for https://myprivatetoken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
⠙ Resolving dependencies...                                                                                                                                                             TRACE Performing lookahead for my-repository @ file:///workspaces/my-repository
DEBUG Found static `pyproject.toml` for: my-repository @ file:///workspaces/my-repository
DEBUG No workspace root found, using project root
⠙ Resolving dependencies...                                                                                                                                                             DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: my-repository*
DEBUG Searching for a compatible version of my-repository @ file:///workspaces/my-repository (*)
⠙ my-repository==0.1.0                                                                                                                                                               DEBUG Adding transitive dependency for my-repository==0.1.0: my-private-lib==1.0.0
TRACE Fetching metadata for my-private-lib from https://myprivatetoken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-lib/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/29fb3946eb316a55/my-private-lib.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-lib/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-lib/
TRACE Handling request for https://myprivatetoken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-lib/
TRACE Request for https://myprivatetoken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-lib/ is missing a password, looking for credentials
TRACE No password in cache for realm myprivatetoken@https://pkgs.dev.azure.com
TRACE No password in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-lib/
⠼ my-repository==0.1.0                                                                                                                                                               TRACE Fetching metadata for my-private-lib from https://pypi.org/simple/my-private-lib/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/pypi/my-private-lib.rkyv
DEBUG No cache entry for: https://pypi.org/simple/my-private-lib/
TRACE Sending fresh GET request for https://pypi.org/simple/my-private-lib/
TRACE Handling request for https://pypi.org/simple/my-private-lib/
TRACE Request for https://pypi.org/simple/my-private-lib/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/my-private-lib/
TRACE Attempting unauthenticated request for https://pypi.org/simple/my-private-lib/
⠦ my-repository==0.1.0                                                                                                                                                               TRACE Request for https://pypi.org/simple/my-private-lib/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm https://pypi.org
TRACE Received package metadata for: my-private-lib
DEBUG Searching for a compatible version of my-private-lib (==1.0.0)
TRACE Selecting candidate for my-private-lib with range ==1.0.0 with 0 remote versions
DEBUG No compatible version found for: my-private-lib
DEBUG Searching for a compatible version of my-repository @ file:///workspaces/my-repository (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: my-repository
TRACE Resolver derivation tree before reduction
  root==0a0.dev0 depends on my-repository*
      my-repository==0.1.0 depends on my-private-lib==1.0.0
      my-private-lib not found in the package registry
    no versions of my-repository<0.1.0 | >0.1.0
TRACE Resolver derivation tree after reduction
  my-repository==0.1.0 depends on my-private-lib==1.0.0
  my-private-lib not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-lib was not found in the package registry and your project depends on my-private-lib==1.0.0, we can conclude that your
      project's requirements are unsatisfiable.
```

---

_Comment by @AndreuCodina on 2024-10-15 18:18_

Sorry, the provided token didn't have read permissions. But the error message isn't correct (`Because my-private-lib was not found in the package registry`). Could you improve it, please?

---

_Comment by @charliermarsh on 2024-10-15 18:19_

We generally can't, no, because some registries treat a 403 as "package not found on the index" rather than "invalid credentials".

---

_Label `question` added by @zanieb on 2024-10-15 18:25_

---

_Comment by @zanieb on 2024-10-15 18:25_

I guess we could have a hint that says maybe your credentials are wrong for some specific package indexes


---

_Comment by @AndreuCodina on 2024-10-15 18:26_

> We generally can't, no, because some registries treat a 403 as "package not found on the index" rather than "invalid credentials".

And can't you detect the URL (the host starts with a string) is from Azure DevOps?

---

_Comment by @charliermarsh on 2024-10-15 18:29_

Isn't it Azure itself that returns 401 when a package isn't found? https://github.com/astral-sh/uv/issues/3291


---

_Closed by @AndreuCodina on 2024-10-15 20:24_

---

_Reopened by @charliermarsh on 2024-10-15 20:55_

---

_Comment by @charliermarsh on 2024-10-15 20:55_

I'm gonna re-open because I think we should be able to at least add a hint for this.

---

_Comment by @charliermarsh on 2024-10-15 20:56_

I also did some testing and it looks like Azure _does_ return a 404 when a package isn't found. Maybe we were confused by https://github.com/astral-sh/uv/issues/3291? I don't know.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-16 02:11_

---

_Label `question` removed by @charliermarsh on 2024-10-16 02:12_

---

_Label `error messages` added by @charliermarsh on 2024-10-16 02:12_

---

_Closed by @charliermarsh on 2024-10-16 17:34_

---

```yaml
number: 11316
title: Private package not found from Azure DevOps
type: issue
state: open
author: AndreuCodina
labels:
  - bug
assignees: []
created_at: 2025-02-07T14:03:51Z
updated_at: 2025-02-07T14:40:32Z
url: https://github.com/astral-sh/uv/issues/11316
synced_at: 2026-01-12T16:00:34Z
```

# Private package not found from Azure DevOps

---

_@AndreuCodina_

### Summary

In a new project, I can't download a private package from Azure DevOps.

I've created [a repository](https://github.com/AndreuCodina/update-version-bug) with this pyproject.toml:

```toml
[project]
name = "update-version-bug"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["private-package==1.0.3"]

[tool.uv]
required-version = ">=0.5.29"

[[tool.uv.index]]
name = "myfeed"
url = "https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple"
```

I've created Personal Access Token in Azure DevOps and created this environment variable:

```bash
export UV_INDEX_MYFEED_PASSWORD="awdibawdiawdoawdoawdoiwandanwdadwnawodnadwnawdinawidnawdnawd"
```

But I can't execute `uv sync` successfully:

```bash
$ RUST_LOG=uv=trace uv sync  

DEBUG uv 0.5.29 (Homebrew 2025-02-06)
DEBUG Found project root: `/Users/andreu/update-version-bug`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/Users/andreu/update-version-bug` at `/var/folders/hr/_awdawdadaw/T/uv-awdaddwadw.lock`
DEBUG Acquired lock for `/Users/andreu/update-version-bug`
DEBUG Reading Python requests from version file at `/Users/andreu/update-version-bug/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/Users/andreu/.local/share/uv/python`
TRACE Searching PATH for executables: python3.12, python3, python
TRACE Checking `PATH` directory for interpreters: /Library/Frameworks/Python.framework/Versions/3.12/bin
TRACE Found possible Python executable: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
TRACE Cached interpreter info for Python 3.12.1, skipping probing: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
DEBUG Found `cpython-3.12.1-macos-aarch64-none` at `/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12` (first executable in the search path)
Using CPython 3.12.1 interpreter at: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
DEBUG Released lock at `/var/folders/hr/_awdawdadaw/T/uv-awdaddwadw.lock`
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: update-version-bug @ file:///Users/andreu/update-version-bug
DEBUG No workspace root found, using project root
TRACE Performing lookahead for update-version-bug @ file:///Users/andreu/update-version-bug
DEBUG Solving with installed Python version: 3.12.1
DEBUG Solving with target Python version: >=3.12
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: update-version-bug*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: update-version-bug. remaining choices: 
DEBUG Searching for a compatible version of update-version-bug @ file:///Users/andreu/update-version-bug (*)
DEBUG Adding direct dependency: private-package>=1.0.3, <1.0.3+
TRACE Fetching metadata for private-package from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple/private-package/
TRACE Assigned packages: root==0a0.dev0, update-version-bug==0.1.0
TRACE Chose package for decision: private-package. remaining choices: 
TRACE No cache entry exists for /Users/andreu/Library/Caches/uv/simple-v15/index/29fb3946eb316a55/private-package.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple/private-package/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple/private-package/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple/private-package/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple/private-package/ is missing a password, looking for credentials
TRACE No credentials in cache for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple/private-package/
DEBUG No netrc file found
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.dev.azure.com")), port: None, path: "/mycompany/_packaging/MyProject/pypi/simple/private-package/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple/private-package/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for private-package from https://pypi.org/simple/private-package/
TRACE No cache entry exists for /Users/andreu/Library/Caches/uv/simple-v15/pypi/private-package.rkyv
DEBUG No cache entry for: https://pypi.org/simple/private-package/
TRACE Sending fresh GET request for https://pypi.org/simple/private-package/
TRACE Handling request for https://pypi.org/simple/private-package/
TRACE Request for https://pypi.org/simple/private-package/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/private-package/
TRACE Attempting unauthenticated request for https://pypi.org/simple/private-package/
TRACE Request for https://pypi.org/simple/private-package/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm https://pypi.org
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple/private-package/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pypi.org/simple/private-package/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Received package metadata for: private-package
DEBUG Searching for a compatible version of private-package (>=1.0.3, <1.0.3+)
TRACE Selecting candidate for private-package with range >=1.0.3, <1.0.3+ with 0 remote versions
DEBUG No compatible version found for: private-package
DEBUG Recording unit propagation conflict of private-package from incompatibility of (update-version-bug)
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: update-version-bug. remaining choices: 
DEBUG Searching for a compatible version of update-version-bug @ file:///Users/andreu/update-version-bug (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: update-version-bug
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on update-version-bug*
  term update-version-bug*
    term update-version-bug==0.1.0
      update-version-bug==0.1.0 depends on private-package==1.0.3
      private-package not found in the package registry
    no versions of update-version-bug<0.1.0 | >0.1.0
TRACE Resolver derivation tree after reduction
term update-version-bug==0.1.0
  update-version-bug==0.1.0 depends on private-package==1.0.3
  private-package not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because private-package was not found in the package registry and your project depends on private-package==1.0.3, we can conclude that your project's requirements
      are unsatisfiable.

      hint: An index URL (https://pkgs.dev.azure.com/mycompany/_packaging/MyProject/pypi/simple) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```

### Platform

macOs 15.2 arm64 or Windows Subsystem for Linux

### Version

0.5.29

### Python version

3.12

---

_Label `bug` added by @AndreuCodina on 2025-02-07 14:03_

---

_Comment by @charliermarsh on 2025-02-07 14:05_

Try setting both the username and password via env var? We don't "mix" credentials like that.

---

_Comment by @AndreuCodina on 2025-02-07 14:12_

> Try setting both the username and password via env var? We don't "mix" credentials like that.

It works when we set both:

```bash
export UV_INDEX_MYFEED_PASSWORD="awdibawdiawdoawdoawdoiwandanwdadwnawodnadwnawdinawidnawdnawd"
export UV_INDEX_MYFEED_USERNAME="VssSessionToken"
```

Great! But I don't understand why. In a Dockerfile, if we don't set `UV_INDEX_MYFEED_USERNAME`, it works, but in local we have to set both.

---

_Comment by @charliermarsh on 2025-02-07 14:30_

Happy to take a look if you can provide a minimal example to reproduce.

---

_Comment by @AndreuCodina on 2025-02-07 14:40_

I can't add it to the repository for security reasons, but these are the Dockerfile and the commands:

```
FROM ghcr.io/astral-sh/uv:0.5.29-python3.12-bookworm-slim AS builder

ENV UV_KEYRING_PROVIDER=disabled
# ENV UV_INDEX_MYFEED_USERNAME=VssSessionToken
ENV UV_COMPILE_BYTECODE=1
ENV UV_NO_CACHE=1

WORKDIR /app

COPY pyproject.toml uv.lock ./

RUN --mount=type=secret,id=UV_INDEX_MYFEED_PASSWORD \
    --mount=type=cache,target=/root/.cache/uv \
    export UV_INDEX_MYFEED_PASSWORD=$(cat /run/secrets/UV_INDEX_MYFEED_PASSWORD) \
    && uv sync --no-dev --frozen --no-install-project
COPY . .
RUN --mount=type=secret,id=UV_INDEX_MYFEED_PASSWORD \
    --mount=type=cache,target=/root/.cache/uv \
    export UV_INDEX_MYFEED_PASSWORD=$(cat /run/secrets/UV_INDEX_MYFEED_PASSWORD) \
    && uv sync --no-dev --frozen

FROM python:3.12-slim-bookworm AS final

WORKDIR /app

CMD ["sleep", "infinity"]
```

```bash
$ export UV_INDEX_MYFEED_PASSWORD="awdibawdiawdoawdoawdoiwandanwdadwnawodnadwnawdinawidnawdnawd"

$ docker build \
    --tag update-version-bug:0.1.0 \
    --platform=linux/amd64 \
    --secret id=UV_INDEX_MYFEED_PASSWORD \
    .
```

---

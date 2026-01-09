---
number: 15082
title: Gemfury private registry with UV
type: issue
state: open
author: Lyranile
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-08-05T13:49:40Z
updated_at: 2025-09-15T13:52:37Z
url: https://github.com/astral-sh/uv/issues/15082
synced_at: 2026-01-07T13:12:19-06:00
---

# Gemfury private registry with UV

---

_Issue opened by @Lyranile on 2025-08-05 13:49_

Hey,

I'm having trouble integrating my internal package in gemfury with uv

```
× No solution found when resolving dependencies:
  ╰─:arrow_forward: Because my-private-package was not found in the package registry and your project depends on my-private-package, we can conclude that your project's
      requirements are unsatisfiable.

      hint: An index URL https://pypi.fury.org/myorg could not be queried due to a lack of valid authentication credentials (401
      Unauthorized).
```

I've tried with repo.fury.org as well as pypi.fury.org, as well as with the token embedded ${TOKEN}@pypi.fury.org
Also, I've tried setting the token to the UV_INDEX_PRIVATE_REGISTRY_USERNAME and UV_INDEX_PRIVATE_REGISTRY_PASSWORD and many other configurations

The bottom line is that this configuration in auth.toml of poetry worked
```
[http-basic.fury]
username = ${TOKEN}
```

With the URL set in the pyproject
```
[[tool.poetry.source]]
name     = "fury"
url          = "https://pypi.fury.io/myorg/"
```

And I couldn't get any uv configuration to work

---

_Comment by @zanieb on 2025-08-05 16:27_

Please share verbose logs (`-vv`) for the invocation with a minimal `pyproject.toml`

---

_Label `question` added by @zanieb on 2025-08-05 16:27_

---

_Label `needs-mre` added by @zanieb on 2025-08-05 16:27_

---

_Comment by @tkfoss on 2025-09-05 09:45_

@zanieb getting the same thing here, and it's keeping us on poetry.

tried to auth with username+password, .netrc, and finally `uv auth login --token`

pyproject
```toml
[project]
name = "projectname"
version="0.0.0"

dependencies = [
    "gemrufy_hosted_pkgn==0.5.8",
]

[tool.uv.sources]
gemrufy_hosted_pkgn = { index = "ACMEICN" }

[[tool.uv.index]]
name = "ACMEICN"
url = "https://pypi.fury.io/ACMEICN/"
explicit = true
authenticate = "always"
```
logs
```bash
DEBUG uv 0.8.15 (8473ecba1 2025-09-03)
DEBUG Found project root: `/Users/tom/work/projectname`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/Users/tom/work/projectname` at `/var/folders/v8/z1pq38xd0zd8jq8rv8nvmmhc0000gn/T/uv-e5e73efd60249d8f.lock`
DEBUG Acquired lock for `/Users/tom/work/projectname`
DEBUG No Python version file found in workspace: /Users/tom/work/projectname
DEBUG Checking for Python environment at: `.venv`
TRACE Found cached interpreter info for Python 3.12.11, skipping query of: .venv/bin/python3
TRACE The virtual environment's Python interpreter meets the Python preference: `prefer managed`
DEBUG Released lock at `/var/folders/v8/z1pq38xd0zd8jq8rv8nvmmhc0000gn/T/uv-e5e73efd60249d8f.lock`
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.12`.
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: projectname @ file:///Users/tom/work/projectname
DEBUG No workspace root found, using project root
TRACE Performing lookahead for projectname @ file:///Users/tom/work/projectname
DEBUG Solving with installed Python version: 3.12.11
DEBUG Solving with target Python version: >=3.12
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: projectname*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: projectname. remaining choices: 
DEBUG Searching for a compatible version of projectname @ file:///Users/tom/work/projectname (*)
DEBUG Adding direct dependency: gemrufy_hosted_pkgn>=0.5.8, <0.5.8+
TRACE Assigned packages: root==0a0.dev0, projectname==0.0.0
TRACE Chose package for decision: gemrufy_hosted_pkgn. remaining choices: 
TRACE Fetching metadata for gemrufy_hosted_pkgn from https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
TRACE No cache entry exists for /Users/tom/.cache/uv/simple-v17/index/ecacaa386cc082fe/gemrufy_hosted_pkgn.rkyv
DEBUG No cache entry for: https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
TRACE Sending fresh GET request for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
TRACE Handling request for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/ with authentication policy always
TRACE Request for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
TRACE Checking for credentials for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
TRACE No credentials in cache for URL https://pypi.fury.io/ACMEICN/
TRACE No credentials in cache for realm https://pypi.fury.io
DEBUG No netrc file found
TRACE Checking lock for `credentials store` at `/Users/tom/.local/share/uv/credentials/credentials.toml.lock`
DEBUG Acquired lock for `credentials store`
DEBUG Loaded credential file /Users/tom/.local/share/uv/credentials/credentials.toml
DEBUG Released lock at `/Users/tom/.local/share/uv/credentials/credentials.toml.lock`
DEBUG Checking text store for credentials for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
DEBUG Found credentials in plaintext store for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
TRACE Retrying request for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/ with Basic { username: Username(Some("__token__")), password: Some(****) }
TRACE Considering retry of response HTTP 404 Not Found for https://pypi.fury.io/ACMEICN/gemrufy_hosted_pkgn/
TRACE Cannot retry nested reqwest middleware error
TRACE Received package metadata for: gemrufy_hosted_pkgn
DEBUG Searching for a compatible version of gemrufy_hosted_pkgn (>=0.5.8, <0.5.8+)
TRACE Selecting candidate for gemrufy_hosted_pkgn with range >=0.5.8, <0.5.8+ with 0 remote versions
DEBUG No compatible version found for: gemrufy_hosted_pkgn
DEBUG Recording unit propagation conflict of gemrufy_hosted_pkgn from incompatibility of (projectname)
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: projectname. remaining choices: 
DEBUG Searching for a compatible version of projectname @ file:///Users/tom/work/projectname (<0.0.0 | >0.0.0)
DEBUG No compatible version found for: projectname
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on projectname*
  term projectname*
    term projectname==0.0.0
      projectname==0.0.0 depends on gemrufy_hosted_pkgn==0.5.8
      gemrufy_hosted_pkgn not found in the package registry
    no versions of projectname<0.0.0 | >0.0.0
TRACE Resolver derivation tree after reduction
term projectname==0.0.0
  projectname==0.0.0 depends on gemrufy_hosted_pkgn==0.5.8
  gemrufy_hosted_pkgn not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because gemrufy_hosted_pkgn was not found in the package registry and your project depends on gemrufy_hosted_pkgn==0.5.8,
      we can conclude that your project's requirements are unsatisfiable.
DEBUG Released lock at `/Users/tom/work/projectname/.venv/.lock`
```

---

_Comment by @konstin on 2025-09-05 09:50_

Can you share example invocations with redacted credentials for both poetry and uv? The "No credentials in cache" should not happen when credentials are set.

---

_Comment by @tkfoss on 2025-09-05 09:57_

Tried:
```
uv auth login  https://pypi.fury.io/ACMEINC/
uv auth login --username $... --password $... https://pypi.fury.io/ACMEINC/
uv auth login  --token $... https://pypi.fury.io/ACMEINC/
```

with permutations of 
- no username, password -> breaks conf file and results in error
- username, password
- password as username, password

resolved by:
```bash
rm ~/.local/share/uv/credentials/credentials.toml.lock
rm ~/.local/share/uv/credentials/credentials.toml
rm -rf ~/.cache/uv
uv auth login --username $GEMFURY_TOKEN --password $GEMFURY_TOKEN https://pypi.fury.io/ACMEINC/
```

---

_Comment by @zanieb on 2025-09-08 13:54_

You can't set both the username and password to the token.

It sounds like you want `UV_INDEX_PRIVATE_REGISTRY_USERNAME=$GEMFURY_TOKEN`?

---

_Comment by @mortenboldt on 2025-09-15 13:50_

> You can't set both the username and password to the token.
> 
> It sounds like you want `UV_INDEX_PRIVATE_REGISTRY_USERNAME=$GEMFURY_TOKEN`?

Yeah, probably `UV_INDEX_FURY_USERNAME=$GEMFURY_TOKEN` and `UV_INDEX_FURY_PASSWORD=NOPASS`
Setting those 2 env vars should authenticate you to a Gemfury repo, that you have named "fury" in your `pyproject.toml` file. That of course also requires you to have a `GEMFURY_TOKEN` env var set with the access token.

---

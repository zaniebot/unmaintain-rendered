---
number: 11236
title: Keyring integration requires entry at /simple/package/, not /simple/
type: issue
state: closed
author: Willem-J-an
labels:
  - bug
assignees: []
created_at: 2025-02-05T07:54:33Z
updated_at: 2025-04-29T21:37:04Z
url: https://github.com/astral-sh/uv/issues/11236
synced_at: 2026-01-07T13:12:18-06:00
---

# Keyring integration requires entry at /simple/package/, not /simple/

---

_Issue opened by @Willem-J-an on 2025-02-05 07:54_

### Summary

I'm trying to use the keyring integration to authenticate to private repo for one of the packages.
The behavior that I see for `uv sync --extra dev --verbose` is: (See below for corresponding pyproject extract)
> Note that this behavior is consistent across uv add, uv pip install, uv sync, etc

IF `keyring get https://private-sandbox.com/pypi/simple/ Token` yields the password:
```
Checking keyring for credentials for Token@https://private-sandbox.com/pypi/simple/authlib/
# The first {index}/{random-package} is checked for a keyring entry, this is not found
# toolbox package can not be resolved and installation fails
```
Now if I "hack" it, and set the secret to the {random-package} I saw being used:
IF `keyring get https://private-sandbox.com/pypi/simple/authlib/ Token` yields the password:
```
Checking keyring for credentials for Token@https://private-sandbox.com/pypi/simple/authlib/
Found credentials in keyring for https://private-sandbox.com/pypi/simple/authlib/
# Toolbox installation can now be resolved and installation succeeds.
```

The expected behavior would be for uv to check for keyring at "repo endswith /simple/" not "repo endswith /simple/random-package/".

```
dependencies = [
    "toolbox==0.7.8",
    "authlib>=1.4.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project.optional-dependencies]
dev = [
  "basedpyright",
  "ruff",
]

[tool.uv.sources]
toolbox = { index = "sandbox" }

[[tool.uv.index]]
name = "sandbox"
url = "https://Token@private-sandbox.com/pypi/simple/"
explicit = true

[[tool.uv.index]]
url = "https://pypi.org/simple"
default = true
```



### Platform

Linux 6.8.0-52-generic x86_64 GNU/Linux

### Version

uv 0.5.28

### Python version

Python 3.11.11

---

_Label `bug` added by @Willem-J-an on 2025-02-05 07:54_

---

_Renamed from "Keyring integration quirky" to "Keyring integration requires entry at /simple/package/, not /simple/" by @Willem-J-an on 2025-02-05 07:55_

---

_Comment by @zanieb on 2025-02-05 15:23_

Thanks for the report!

What keyring plugin are you using? This behavior actually differs per keyring plugin — and I'd argue that the behavior that checks for a prefix-match is correct rather than requiring the user to provide the exact URL for the index root.

---

_Comment by @Willem-J-an on 2025-02-05 15:41_

Do you mean keyring backend, or keyring plugin in uv?
I think it's just python keyring with subprocess mode.

You argue that check for prefix match is correct, so that would be setting /simple/, not /simple/package/ right? So you agree with my bug report then?🤔



---

_Comment by @zanieb on 2025-02-05 15:45_

I'm talking about the keyring backend.

> You argue that check for prefix match is correct, so that would be setting /simple/, not /simple/package/ right? So you agree with my bug report then?

I'm saying that keyring should behave as such:

If there's a request for `https://example/com/foo/bar` and there are credentials for `https://example/com/foo` it should return them.

---

_Comment by @zanieb on 2025-02-05 15:45_

I think I'll need trace logs to dig in further.

---

_Comment by @Willem-J-an on 2025-02-06 07:17_

I see what you mean, my keyring backends don't work that way.
```
keyring.backends.chainer.ChainerBackend (priority: 10)
keyring.backends.fail.Keyring (priority: 0)
keyring.backends.libsecret.Keyring (priority: 4.8)
keyring.backends.SecretService.Keyring (priority: 5)
```
I have these and all of them return the secret (except for the fail.Keyring), but not by prefix. 

---

_Comment by @Willem-J-an on 2025-02-06 08:36_

Decided to look into the rust code, lord bless my soul.

If the first attempt fails, a second attempt is made to check for host credentials.
[See keyring.rs](https://github.com/astral-sh/uv/blob/5ce48cf79910975c089046d97a45334ad2b3cf0a/crates/uv-auth/src/keyring.rs#L66C8-L80C10)
It's a bit finicky (No ending backslash, no protocol - these are present in the absolute URI), but:
IF `keyring get private-sandbox.com Token` yields the token, the token is successfully retrieved.

This seems like a solid workaround for my case. I do think the docs need some love on this topic.

---

_Comment by @zanieb on 2025-02-06 14:28_

Can you share those trace logs? I'm surprised you'd need to do that.

---

_Comment by @Willem-J-an on 2025-02-06 15:21_

```
 .venv  ~/code/sample  RUST_LOG=uv=trace uv sync -v
DEBUG uv 0.5.28
DEBUG Found project root: `/home/user/code/sample`
DEBUG No workspace root found, using project root
DEBUG Using Python request `>=3.11` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
TRACE Ignoring stale interpreter markers for: .venv/bin/python3
TRACE Querying interpreter executable at /home/user/code/sample/.venv/bin/python3
DEBUG The virtual environment's Python version satisfies `>=3.11`
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: sample-project @ file:///home/user/code/sample
DEBUG No workspace root found, using project root
TRACE Performing lookahead for sample-project @ file:///home/user/code/sample
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11
DEBUG Narrowed `requires-python` bound to: >=3.11
DEBUG Narrowed `requires-python` bound to: >=3.11
DEBUG Solving split (python_full_version >= '3.12') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.11" }]), range: RequiresPythonRange(LowerBound(Included("3.12")), UpperBound(Unbounded)) })
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: sample-project*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: sample-project. remaining choices:
DEBUG Searching for a compatible version of sample-project @ file:///home/user/code/sample (*)
DEBUG Adding direct dependency: cachy>=0.3.0, <0.3.0+
TRACE Assigned packages: root==0a0.dev0, sample-project==0.1.0
TRACE Chose package for decision: cachy. remaining choices:
TRACE Fetching metadata for cachy from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Fetching metadata for cachy from https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/cachy.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/ is missing a password, looking for credentials
TRACE No password in cache for realm Token@https://pkgs.private-repo.com
TRACE No password in cache for URL https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
DEBUG No netrc file found
DEBUG Checking keyring for credentials for Token@https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Checking keyring for URL https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/33e39c716258d6c3/cachy.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/ is missing a password, looking for credentials
TRACE No password in cache for realm Token@https://pkgs.private-repo.com
TRACE No password in cache for URL https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Checking keyring for host pkgs.private-repo.com
TRACE Skipping fetch of credentials for https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/cachy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for cachy from https://pypi.org/simple/cachy/
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/pypi/cachy.rkyv
DEBUG No cache entry for: https://pypi.org/simple/cachy/
TRACE Sending fresh GET request for https://pypi.org/simple/cachy/
TRACE Handling request for https://pypi.org/simple/cachy/
TRACE Request for https://pypi.org/simple/cachy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/cachy/
TRACE Attempting unauthenticated request for https://pypi.org/simple/cachy/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/feed/pypi/simple/cachy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Received package metadata for: cachy
DEBUG Searching for a compatible version of cachy (>=0.3.0, <0.3.0+)
TRACE Selecting candidate for cachy with range >=0.3.0, <0.3.0+ with 0 remote versions
DEBUG No compatible version found for: cachy
DEBUG Recording unit propagation conflict of cachy from incompatibility of (sample-project)
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: sample-project. remaining choices:
DEBUG Searching for a compatible version of sample-project @ file:///home/user/code/sample (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: sample-project
TRACE Cached request https://pypi.org/simple/cachy/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: cachy
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on sample-project*
  term sample-project*
    term sample-project==0.1.0
      sample-project==0.1.0 depends on cachy==0.3.0
      cachy not found in the package registry
    no versions of sample-project<0.1.0 | >0.1.0
TRACE Resolver derivation tree after reduction
term sample-project==0.1.0
  sample-project==0.1.0 depends on cachy==0.3.0
  cachy not found in the package registry
  × No solution found when resolving dependencies for split (python_full_version >= '3.12'):
  ╰─▶ Because cachy was not found in the package registry and your project depends on cachy==0.3.0, we can conclude that your project's requirements are unsatisfiable.

      hint: An index URL (https://pkgs.private-repo.com/_packaging/feed/pypi/simple/) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).

      hint: An index URL (https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).

 .venv  ~/code/sample  pass show token | keyring set pkgs.private-repo.com Token

 .venv  ~/code/sample  RUST_LOG=uv=trace uv sync -v
DEBUG uv 0.5.28
DEBUG Found project root: `/home/user/code/sample`
DEBUG No workspace root found, using project root
DEBUG Using Python request `>=3.11` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.11.11, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `>=3.11`
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: sample-project @ file:///home/user/code/sample
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `sample-project==0.1.0`
  Requested: {Requirement { name: PackageName("cachy"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.3.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/feed/pypi/simple/", query: None, fragment: None }), conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("authlib"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.4.0" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("toolbox"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.7.8" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/", query: None, fragment: None }), conflict: None }, origin: None }}
TRACE Performing lookahead for sample-project @ file:///home/user/code/sample
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11
DEBUG Narrowed `requires-python` bound to: >=3.11
DEBUG Narrowed `requires-python` bound to: >=3.11
DEBUG Solving split (python_full_version >= '3.12') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.11" }]), range: RequiresPythonRange(LowerBound(Included("3.12")), UpperBound(Unbounded)) })
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: sample-project*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: sample-project. remaining choices:
DEBUG Searching for a compatible version of sample-project @ file:///home/user/code/sample (*)
DEBUG Adding direct dependency: cachy>=0.3.0, <0.3.0+
TRACE Assigned packages: root==0a0.dev0, sample-project==0.1.0
TRACE Chose package for decision: cachy. remaining choices:
TRACE Fetching metadata for cachy from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Fetching metadata for cachy from https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/cachy.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/ is missing a password, looking for credentials
TRACE No password in cache for realm Token@https://pkgs.private-repo.com
TRACE No password in cache for URL https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
DEBUG No netrc file found
DEBUG Checking keyring for credentials for Token@https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Checking keyring for URL https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/33e39c716258d6c3/cachy.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/ is missing a password, looking for credentials
TRACE No password in cache for realm Token@https://pkgs.private-repo.com
TRACE No password in cache for URL https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Checking keyring for host pkgs.private-repo.com
DEBUG Found credentials in keyring for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Using credentials from previous fetch for https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/cachy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for cachy from https://pypi.org/simple/cachy/
TRACE Cached request https://pypi.org/simple/cachy/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/cachy/
TRACE Received package metadata for: cachy
TRACE Updating cached credentials for https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/ to Credentials { username: Username(Some("Token")), password: Some("NotMyPassword,PromiseDad") }
TRACE Cached request https://pkgs.private-repo.com/_packaging/feed/pypi/simple/cachy/ is not storable because its response has a 'no-store' cache-control directive
TRACE Received package metadata for: cachy
DEBUG Searching for a compatible version of cachy (>=0.3.0, <0.3.0+)
TRACE Selecting candidate for cachy with range >=0.3.0, <0.3.0+ with 7 remote versions
TRACE Found candidate for package cachy with range >=0.3.0, <0.3.0+ after 1 steps: 0.3.0 version
TRACE Returning candidate for package cachy with range >=0.3.0, <0.3.0+ after 1 steps
DEBUG Selecting: cachy==0.3.0 [compatible] (cachy-0.3.0-py2.py3-none-any.whl)
TRACE No cache entry exists for /home/user/.cache/uv/wheels-v3/index/33e39c716258d6c3/cachy/cachy-0.3.0-py2.py3-none-any.msgpack
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Sending fresh HEAD request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Handling request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Attempting unauthenticated request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl", query: None, fragment: Some("sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7") }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(405), url: "https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7" }))) }
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for cachy-0.3.0-py2.py3-none-any.whl; streaming wheel
TRACE No cache entry exists for /home/user/.cache/uv/wheels-v3/index/33e39c716258d6c3/cachy/cachy-0.3.0-py2.py3-none-any.msgpack
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Handling request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Attempting unauthenticated request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7
TRACE Request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7 failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://pkgs.private-repo.com
TRACE Retrying request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7 with credentials from cache Credentials { username: Username(Some("Token")), password: Some("NotMyPassword,PromiseDad") }
TRACE Cached request https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl#sha256=338ca09c8860e76b275aff52374330efedc4d5a5e45dc1c5b539c1ead0786fe7 is storable because its response has a heuristically cacheable status code 200
TRACE Received built distribution metadata for: cachy==0.3.0
TRACE Assigned packages: root==0a0.dev0, sample-project==0.1.0, cachy==0.3.0
DEBUG Tried 2 versions: cachy 1, sample-project 1
DEBUG split `python_full_version >= '3.12'` resolution took 0.822s
DEBUG Solving split (python_full_version == '3.11.*') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.11" }]), range: RequiresPythonRange(LowerBound(Included("3.11")), UpperBound(Excluded("3.12"))) })
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: sample-project*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: sample-project. remaining choices:
DEBUG Searching for a compatible version of sample-project @ file:///home/user/code/sample (*)
DEBUG Adding direct dependency: cachy>=0.3.0, <0.3.0+
TRACE Assigned packages: root==0a0.dev0, sample-project==0.1.0
TRACE Chose package for decision: cachy. remaining choices:
DEBUG Searching for a compatible version of cachy (>=0.3.0, <0.3.0+)
TRACE Using preference cachy 0.3.0
DEBUG Selecting: cachy==0.3.0 [preference] (cachy-0.3.0-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, sample-project==0.1.0, cachy==0.3.0
DEBUG Tried 2 versions: cachy 1, sample-project 1
DEBUG split `python_full_version == '3.11.*'` resolution took 0.000s
INFO Solved your requirements for 2 environments
DEBUG Distinct solution for split (python_full_version >= '3.12') with 2 packages
DEBUG Distinct solution for split (python_full_version == '3.11.*') with 2 packages
TRACE Resolution: ResolverEnvironment { kind: Universal { initial_forks: [python_full_version >= '3.12', python_full_version == '3.11.*'], markers: python_full_version >= '3.12', include: {}, exclude: {} } }
TRACE Resolution edge: sample-project -> cachy
TRACE Resolution edge:     0.1.0 -> 0.3.0
TRACE Resolution edge: ROOT -> sample-project
TRACE Resolution edge:     0a0.dev0 -> 0.1.0
TRACE Resolution: ResolverEnvironment { kind: Universal { initial_forks: [python_full_version >= '3.12', python_full_version == '3.11.*'], markers: python_full_version == '3.11.*', include: {}, exclude: {} } }
TRACE Resolution edge: sample-project -> cachy
TRACE Resolution edge:     0.1.0 -> 0.3.0
TRACE Resolution edge: ROOT -> sample-project
TRACE Resolution edge:     0a0.dev0 -> 0.1.0
Resolved 2 packages in 826ms
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/feed/pypi/simple/
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: sample-project @ file:///home/user/code/sample
DEBUG Identified uncached distribution: cachy==0.3.0
TRACE Checking lock for `/home/user/.cache/uv/sdists-v7/editable/0da8e777a80d7ce4` at `/home/user/.cache/uv/sdists-v7/editable/0da8e777a80d7ce4/.lock`
DEBUG Acquired lock for `/home/user/.cache/uv/sdists-v7/editable/0da8e777a80d7ce4`
TRACE No cache entry exists for /home/user/.cache/uv/wheels-v3/index/33e39c716258d6c3/cachy/cachy-0.3.0-py2.py3-none-any.http
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl
TRACE Handling request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl
TRACE Request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl
DEBUG Building: sample-project @ file:///home/user/code/sample
TRACE Preview is disabled, not checking for direct build
DEBUG No workspace root found, using project root
DEBUG Assessing Python executable as base candidate: /home/user/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11
DEBUG Using base executable for virtual environment: /home/user/.local/share/uv/python/cpython-3.11.11-linux-x86_64-gnu/bin/python3.11
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11.11
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: hatchling*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: hatchling. remaining choices:
TRACE Fetching metadata for hatchling from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/hatchling/
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/hatchling.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/hatchling/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/hatchling/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/hatchling/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/hatchling/ is missing a password, looking for credentials
TRACE Found cached credentials for realm Token@https://pkgs.private-repo.com
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/hatchling/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/hatchling/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for hatchling from https://pypi.org/simple/hatchling/
TRACE Cached request https://pypi.org/simple/hatchling/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
TRACE Received package metadata for: hatchling
TRACE Selecting candidate for hatchling with range * with 76 remote versions
TRACE Found candidate for package hatchling with range * after 1 steps: 1.27.0 version
TRACE Returning candidate for package hatchling with range * after 1 steps
DEBUG Searching for a compatible version of hatchling (*)
TRACE Selecting candidate for hatchling with range * with 76 remote versions
TRACE Found candidate for package hatchling with range * after 1 steps: 1.27.0 version
TRACE Returning candidate for package hatchling with range * after 1 steps
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/08/e7/ae38d7a6dfba0533684e0b2136817d667588ae3ec984c1a4e5df5eb88482/hatchling-1.27.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/e7/ae38d7a6dfba0533684e0b2136817d667588ae3ec984c1a4e5df5eb88482/hatchling-1.27.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: hatchling==1.27.0
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
TRACE Fetching metadata for packaging from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/packaging/
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0
TRACE Chose package for decision: packaging. remaining choices: trove-classifiers, pathspec, pluggy
TRACE Fetching metadata for pathspec from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pathspec/
TRACE Fetching metadata for pluggy from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pluggy/
TRACE Fetching metadata for trove-classifiers from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/trove-classifiers/
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/packaging.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/packaging/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/packaging/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/packaging/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/packaging/ is missing a password, looking for credentials
TRACE Found cached credentials for realm Token@https://pkgs.private-repo.com
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/pathspec.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pathspec/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pathspec/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pathspec/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pathspec/ is missing a password, looking for credentials
TRACE Found cached credentials for realm Token@https://pkgs.private-repo.com
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/pluggy.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pluggy/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pluggy/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pluggy/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pluggy/ is missing a password, looking for credentials
TRACE Found cached credentials for realm Token@https://pkgs.private-repo.com
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/trove-classifiers.rkyv
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/trove-classifiers/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/trove-classifiers/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/trove-classifiers/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/trove-classifiers/ is missing a password, looking for credentials
TRACE Found cached credentials for realm Token@https://pkgs.private-repo.com
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/trove-classifiers/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/trove-classifiers/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for trove-classifiers from https://pypi.org/simple/trove-classifiers/
TRACE Cached request https://pypi.org/simple/trove-classifiers/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
TRACE Received package metadata for: trove-classifiers
TRACE Selecting candidate for trove-classifiers with range * with 101 remote versions
TRACE Found candidate for package trove-classifiers with range * after 1 steps: 2025.1.15.22 version
TRACE Returning candidate for package trove-classifiers with range * after 1 steps
TRACE Cached request https://files.pythonhosted.org/packages/2b/c5/6422dbc59954389b20b2aba85b737ab4a552e357e7ea14b52f40312e7c84/trove_classifiers-2025.1.15.22-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2b/c5/6422dbc59954389b20b2aba85b737ab4a552e357e7ea14b52f40312e7c84/trove_classifiers-2025.1.15.22-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: trove-classifiers==2025.1.15.22
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/packaging/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/packaging/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for packaging from https://pypi.org/simple/packaging/
TRACE Cached request https://pypi.org/simple/packaging/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
TRACE Received package metadata for: packaging
TRACE Selecting candidate for packaging with range >=24.2 with 46 remote versions
DEBUG Searching for a compatible version of packaging (>=24.2)
TRACE Found candidate for package packaging with range >=24.2 after 1 steps: 24.2 version
TRACE Selecting candidate for packaging with range >=24.2 with 46 remote versions
TRACE Returning candidate for package packaging with range >=24.2 after 1 steps
TRACE Found candidate for package packaging with range >=24.2 after 1 steps: 24.2 version
TRACE Returning candidate for package packaging with range >=24.2 after 1 steps
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: packaging==24.2
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, packaging==24.2
TRACE Chose package for decision: pathspec. remaining choices: trove-classifiers, pluggy
TRACE Request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://pkgs.private-repo.com
TRACE Retrying request for https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl with credentials from cache Credentials { username: Username(Some("Token")), password: Some("NotMyPassword,PromiseDad") }
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/pluggy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pluggy/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pluggy from https://pypi.org/simple/pluggy/
TRACE Cached request https://pypi.org/simple/pluggy/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
TRACE Received package metadata for: pluggy
TRACE Selecting candidate for pluggy with range >=1.0.0 with 23 remote versions
TRACE Found candidate for package pluggy with range >=1.0.0 after 1 steps: 1.5.0 version
TRACE Returning candidate for package pluggy with range >=1.0.0 after 1 steps
TRACE Cached request https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: pluggy==1.5.0
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/pathspec/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/pathspec/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for pathspec from https://pypi.org/simple/pathspec/
TRACE Cached request https://pypi.org/simple/pathspec/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
TRACE Received package metadata for: pathspec
TRACE Selecting candidate for pathspec with range >=0.10.1 with 31 remote versions
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
TRACE Selecting candidate for pathspec with range >=0.10.1 with 31 remote versions
TRACE Found candidate for package pathspec with range >=0.10.1 after 1 steps: 0.12.1 version
TRACE Returning candidate for package pathspec with range >=0.10.1 after 1 steps
TRACE Found candidate for package pathspec with range >=0.10.1 after 1 steps: 0.12.1 version
TRACE Returning candidate for package pathspec with range >=0.10.1 after 1 steps
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: pathspec==0.12.1
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, packaging==24.2, pathspec==0.12.1
TRACE Chose package for decision: pluggy. remaining choices: trove-classifiers
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
TRACE Selecting candidate for pluggy with range >=1.0.0 with 23 remote versions
TRACE Found candidate for package pluggy with range >=1.0.0 after 1 steps: 1.5.0 version
TRACE Returning candidate for package pluggy with range >=1.0.0 after 1 steps
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, packaging==24.2, pathspec==0.12.1, pluggy==1.5.0
TRACE Chose package for decision: trove-classifiers. remaining choices:
DEBUG Searching for a compatible version of trove-classifiers (*)
TRACE Selecting candidate for trove-classifiers with range * with 101 remote versions
TRACE Found candidate for package trove-classifiers with range * after 1 steps: 2025.1.15.22 version
TRACE Returning candidate for package trove-classifiers with range * after 1 steps
DEBUG Selecting: trove-classifiers==2025.1.15.22 [compatible] (trove_classifiers-2025.1.15.22-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, packaging==24.2, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2025.1.15.22
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.226s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.11.11", version: "3.11.11" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.8.0-52-generic", platform_system: "Linux", platform_version: "#53-Ubuntu SMP PREEMPT_DYNAMIC Sat Jan 11 00:06:25 UTC 2025", python_full_version: StringVersion { string: "3.11.11", version: "3.11.11" }, python_version: StringVersion { string: "3.11", version: "3.11" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: hatchling -> packaging
TRACE Resolution edge:     1.27.0 -> 24.2
TRACE Resolution edge: hatchling -> trove-classifiers
TRACE Resolution edge:     1.27.0 -> 2025.1.15.22
TRACE Resolution edge: ROOT -> hatchling
TRACE Resolution edge:     0a0.dev0 -> 1.27.0
TRACE Resolution edge: hatchling -> pathspec
TRACE Resolution edge:     1.27.0 -> 0.12.1
TRACE Resolution edge: hatchling -> pluggy
TRACE Resolution edge:     1.27.0 -> 1.5.0
DEBUG Installing in packaging==24.2, pathspec==0.12.1, pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.1.15.22 in /home/user/.cache/uv/builds-v0/.tmpgY4NYR
DEBUG Requirement already cached: packaging==24.2
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: hatchling==1.27.0
DEBUG Requirement already cached: trove-classifiers==2025.1.15.22
DEBUG Installing build requirements: packaging==24.2, pathspec==0.12.1, pluggy==1.5.0, hatchling==1.27.0, trove-classifiers==2025.1.15.22
TRACE Extracting file name=PackageName("packaging")
TRACE Extracting file name=PackageName("pluggy")
TRACE Extracting file name=PackageName("hatchling")
TRACE Extracting file name=PackageName("trove-classifiers")
TRACE Extracting file name=PackageName("pathspec")
TRACE Extracted 14 files name=PackageName("pluggy")
TRACE Extracted 13 files name=PackageName("pathspec")
TRACE Extracted 8 files name=PackageName("trove-classifiers")
TRACE No entrypoints name=PackageName("pluggy")
TRACE No entrypoints name=PackageName("pathspec")
TRACE No data name=PackageName("pluggy")
TRACE No data name=PackageName("pathspec")
TRACE Writing installer metadata name=PackageName("pluggy")
TRACE Writing installer metadata name=PackageName("pathspec")
TRACE No entrypoints name=PackageName("trove-classifiers")
TRACE No data name=PackageName("trove-classifiers")
TRACE Writing installer metadata name=PackageName("trove-classifiers")
TRACE Extracted 23 files name=PackageName("packaging")
TRACE Writing record name=PackageName("pathspec")
TRACE Writing record name=PackageName("pluggy")
TRACE No entrypoints name=PackageName("packaging")
TRACE Writing record name=PackageName("trove-classifiers")
TRACE No data name=PackageName("packaging")
TRACE Writing installer metadata name=PackageName("packaging")
TRACE Writing record name=PackageName("packaging")
TRACE Extracted 73 files name=PackageName("hatchling")
TRACE Writing entrypoints name=PackageName("hatchling")
TRACE No data name=PackageName("hatchling")
TRACE Writing installer metadata name=PackageName("hatchling")
TRACE Writing record name=PackageName("hatchling")
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11.11
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
TRACE Fetching metadata for editables from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/editables/
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: hatchling. remaining choices: editables
DEBUG Searching for a compatible version of hatchling (*)
TRACE Selecting candidate for hatchling with range * with 76 remote versions
TRACE Found candidate for package hatchling with range * after 1 steps: 1.27.0 version
TRACE Returning candidate for package hatchling with range * after 1 steps
DEBUG Selecting: hatchling==1.27.0 [compatible] (hatchling-1.27.0-py3-none-any.whl)
TRACE Selecting candidate for hatchling with range * with 76 remote versions
TRACE Found candidate for package hatchling with range * after 1 steps: 1.27.0 version
TRACE Returning candidate for package hatchling with range * after 1 steps
TRACE No cache entry exists for /home/user/.cache/uv/simple-v15/index/a3e0f12ac11e205e/editables.rkyv
DEBUG Adding transitive dependency for hatchling==1.27.0: packaging>=24.2
DEBUG No cache entry for: https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/editables/
DEBUG Adding transitive dependency for hatchling==1.27.0: pathspec>=0.10.1
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/editables/
DEBUG Adding transitive dependency for hatchling==1.27.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.27.0: trove-classifiers*
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/editables/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/editables/ is missing a password, looking for credentials
TRACE Found cached credentials for realm Token@https://pkgs.private-repo.com
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0
TRACE Chose package for decision: editables. remaining choices: trove-classifiers, packaging, pathspec, pluggy
TRACE Selecting candidate for trove-classifiers with range * with 101 remote versions
TRACE Found candidate for package trove-classifiers with range * after 1 steps: 2025.1.15.22 version
TRACE Returning candidate for package trove-classifiers with range * after 1 steps
TRACE Selecting candidate for pluggy with range >=1.0.0 with 23 remote versions
TRACE Found candidate for package pluggy with range >=1.0.0 after 1 steps: 1.5.0 version
TRACE Returning candidate for package pluggy with range >=1.0.0 after 1 steps
TRACE Selecting candidate for pathspec with range >=0.10.1 with 31 remote versions
TRACE Found candidate for package pathspec with range >=0.10.1 after 1 steps: 0.12.1 version
TRACE Returning candidate for package pathspec with range >=0.10.1 after 1 steps
TRACE Selecting candidate for packaging with range >=24.2 with 46 remote versions
TRACE Found candidate for package packaging with range >=24.2 after 1 steps: 24.2 version
TRACE Returning candidate for package packaging with range >=24.2 after 1 steps
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/editables/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/editables/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for editables from https://pypi.org/simple/editables/
TRACE Cached request https://pypi.org/simple/editables/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/editables/
TRACE Received package metadata for: editables
TRACE Selecting candidate for editables with range >=0.3, <1.dev0 with 5 remote versions
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
TRACE Selecting candidate for editables with range >=0.3, <1.dev0 with 5 remote versions
TRACE Found candidate for package editables with range >=0.3, <1.dev0 after 1 steps: 0.5 version
TRACE Found candidate for package editables with range >=0.3, <1.dev0 after 1 steps: 0.5 version
TRACE Returning candidate for package editables with range >=0.3, <1.dev0 after 1 steps
TRACE Returning candidate for package editables with range >=0.3, <1.dev0 after 1 steps
DEBUG Selecting: editables==0.5 [compatible] (editables-0.5-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: editables==0.5
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, editables==0.5
TRACE Chose package for decision: packaging. remaining choices: trove-classifiers, pluggy, pathspec
DEBUG Searching for a compatible version of packaging (>=24.2)
TRACE Selecting candidate for packaging with range >=24.2 with 46 remote versions
TRACE Found candidate for package packaging with range >=24.2 after 1 steps: 24.2 version
TRACE Returning candidate for package packaging with range >=24.2 after 1 steps
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, editables==0.5, packaging==24.2
TRACE Chose package for decision: pathspec. remaining choices: trove-classifiers, pluggy
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
TRACE Selecting candidate for pathspec with range >=0.10.1 with 31 remote versions
TRACE Found candidate for package pathspec with range >=0.10.1 after 1 steps: 0.12.1 version
TRACE Returning candidate for package pathspec with range >=0.10.1 after 1 steps
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, editables==0.5, packaging==24.2, pathspec==0.12.1
TRACE Chose package for decision: pluggy. remaining choices: trove-classifiers
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
TRACE Selecting candidate for pluggy with range >=1.0.0 with 23 remote versions
TRACE Found candidate for package pluggy with range >=1.0.0 after 1 steps: 1.5.0 version
TRACE Returning candidate for package pluggy with range >=1.0.0 after 1 steps
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, editables==0.5, packaging==24.2, pathspec==0.12.1, pluggy==1.5.0
TRACE Chose package for decision: trove-classifiers. remaining choices:
DEBUG Searching for a compatible version of trove-classifiers (*)
TRACE Selecting candidate for trove-classifiers with range * with 101 remote versions
TRACE Found candidate for package trove-classifiers with range * after 1 steps: 2025.1.15.22 version
TRACE Returning candidate for package trove-classifiers with range * after 1 steps
DEBUG Selecting: trove-classifiers==2025.1.15.22 [compatible] (trove_classifiers-2025.1.15.22-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, hatchling==1.27.0, editables==0.5, packaging==24.2, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2025.1.15.22
DEBUG Tried 6 versions: editables 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG marker environment resolution took 0.086s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.11.11", version: "3.11.11" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.8.0-52-generic", platform_system: "Linux", platform_version: "#53-Ubuntu SMP PREEMPT_DYNAMIC Sat Jan 11 00:06:25 UTC 2025", python_full_version: StringVersion { string: "3.11.11", version: "3.11.11" }, python_version: StringVersion { string: "3.11", version: "3.11" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: hatchling -> packaging
TRACE Resolution edge:     1.27.0 -> 24.2
TRACE Resolution edge: hatchling -> trove-classifiers
TRACE Resolution edge:     1.27.0 -> 2025.1.15.22
TRACE Resolution edge: ROOT -> editables
TRACE Resolution edge:     0a0.dev0 -> 0.5
TRACE Resolution edge: ROOT -> hatchling
TRACE Resolution edge:     0a0.dev0 -> 1.27.0
TRACE Resolution edge: hatchling -> pathspec
TRACE Resolution edge:     1.27.0 -> 0.12.1
TRACE Resolution edge: hatchling -> pluggy
TRACE Resolution edge:     1.27.0 -> 1.5.0
DEBUG Installing in packaging==24.2, editables==0.5, pluggy==1.5.0, pathspec==0.12.1, hatchling==1.27.0, trove-classifiers==2025.1.15.22 in /home/user/.cache/uv/builds-v0/.tmpgY4NYR
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("packaging"), version: "24.2", path: "/home/user/.cache/uv/builds-v0/.tmpgY4NYR/lib/python3.11/site-packages/packaging-24.2.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "24.2" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: packaging==24.2
DEBUG Requirement already cached: editables==0.5
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("pluggy"), version: "1.5.0", path: "/home/user/.cache/uv/builds-v0/.tmpgY4NYR/lib/python3.11/site-packages/pluggy-1.5.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.5.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: pluggy==1.5.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("pathspec"), version: "0.12.1", path: "/home/user/.cache/uv/builds-v0/.tmpgY4NYR/lib/python3.11/site-packages/pathspec-0.12.1.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.12.1" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: pathspec==0.12.1
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("hatchling"), version: "1.27.0", path: "/home/user/.cache/uv/builds-v0/.tmpgY4NYR/lib/python3.11/site-packages/hatchling-1.27.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.27.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: hatchling==1.27.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("trove-classifiers"), version: "2025.1.15.22", path: "/home/user/.cache/uv/builds-v0/.tmpgY4NYR/lib/python3.11/site-packages/trove_classifiers-2025.1.15.22.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2025.1.15.22" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }), conflict: None }
DEBUG Requirement already installed: trove-classifiers==2025.1.15.22
DEBUG Installing build requirement: editables==0.5
TRACE Extracting file name=PackageName("editables")
TRACE Extracted 7 files name=PackageName("editables")
TRACE No entrypoints name=PackageName("editables")
TRACE No data name=PackageName("editables")
TRACE Writing installer metadata name=PackageName("editables")
TRACE Writing record name=PackageName("editables")
DEBUG Calling `hatchling.build.build_editable("/home/user/.cache/uv/builds-v0/.tmp6IUpZP", {}, None)`
TRACE Cached request https://pkgs.private-repo.com/_packaging/fff0d200-7487-4c06-89d5-5ad3c4f69d97/pypi/download/cachy/0.3/cachy-0.3.0-py2.py3-none-any.whl is storable because its response has a heuristically cacheable status code 200
DEBUG Finished building: sample-project @ file:///home/user/code/sample
DEBUG Released lock at `/home/user/.cache/uv/sdists-v7/editable/0da8e777a80d7ce4/.lock`
Prepared 2 packages in 556ms
TRACE Extracting file name=PackageName("sample-project")
TRACE Extracting file name=PackageName("cachy")
TRACE Extracted 4 files name=PackageName("sample-project")
TRACE No entrypoints name=PackageName("sample-project")
TRACE No data name=PackageName("sample-project")
TRACE Writing installer metadata name=PackageName("sample-project")
TRACE Writing record name=PackageName("sample-project")
TRACE Extracted 28 files name=PackageName("cachy")
TRACE No entrypoints name=PackageName("cachy")
TRACE No data name=PackageName("cachy")
TRACE Writing installer metadata name=PackageName("cachy")
TRACE Writing record name=PackageName("cachy")
Installed 2 packages in 1ms
 + cachy==0.3.0
 + sample-project==0.1.0 (from file:///home/user/code/sample)
```

---

_Comment by @zanieb on 2025-02-06 16:57_

Thank you!

One thing of interest to me is that we cache the username for the root of the index

```
TRACE Caching credentials for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/
```

But we don't consider that cache for subsequent requests

```
TRACE Fetching metadata for cachy from https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Sending fresh GET request for https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Handling request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Request for https://Token@pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/ is missing a password, looking for credentials
TRACE No password in cache for realm Token@https://pkgs.private-repo.com
TRACE No password in cache for URL https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
DEBUG No netrc file found
DEBUG Checking keyring for credentials for Token@https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Checking keyring for URL https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.private-repo.com")), port: None, path: "/_packaging/Sandbox/pypi/simple/cachy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://pkgs.private-repo.com/_packaging/Sandbox/pypi/simple/cachy/" }))) }
TRACE Cannot retry error: not an IO error
```

A few possibilities...

(1) In the vein of https://github.com/astral-sh/uv/issues/4583 we could know this is a package in an index URL and check the keyring for the root if we cannot find credentials for the password
(2) We could eagerly fetch credentials for the root from the keying (before we make requests) which would mean there's a matching password in the cache later
(3) We could use existing cache entries for a parent URL as a hint that we should query the keyring again


---

_Referenced in [astral-sh/uv#11391](../../astral-sh/uv/issues/11391.md) on 2025-02-10 17:07_

---

_Referenced in [astral-sh/uv#11507](../../astral-sh/uv/issues/11507.md) on 2025-02-14 13:02_

---

_Referenced in [astral-sh/uv#12004](../../astral-sh/uv/issues/12004.md) on 2025-03-06 13:25_

---

_Comment by @zanieb on 2025-03-20 22:07_

Note this is a duplicate of https://github.com/astral-sh/uv/issues/4056

---

_Referenced in [astral-sh/uv#12651](../../astral-sh/uv/pulls/12651.md) on 2025-04-03 14:15_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-11 07:32_

---

_Closed by @zanieb on 2025-04-29 21:37_

---

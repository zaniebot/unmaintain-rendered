---
number: 8563
title: Getting dependency from a private Gitlab package registry in CI works with Personal and Deploy token, but not Job token
type: issue
state: closed
author: tomschelsen
labels: []
assignees: []
created_at: 2024-10-25T12:10:38Z
updated_at: 2025-02-07T09:49:03Z
url: https://github.com/astral-sh/uv/issues/8563
synced_at: 2026-01-10T01:24:30Z
---

# Getting dependency from a private Gitlab package registry in CI works with Personal and Deploy token, but not Job token

---

_Issue opened by @tomschelsen on 2024-10-25 12:10_

* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.

```
UV_INDEX_MY_REPO_USERNAME=gitlab-ci-token UV_INDEX_MY_REPO_PASSWORD=$CI_JOB_TOKEN uv run --verbose my_script.py
DEBUG uv 0.4.26
DEBUG Found project root: `/builds/my/project/path`
DEBUG No workspace root found, using project root
DEBUG Discovered project `my-dependency` at: /builds/my/project/path
DEBUG Reading requests from `/builds/my/project/path/.python-version`
DEBUG Searching for Python 3.8 in managed installations or system path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/root/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.cache/.tmp66Csrb
DEBUG Extracting cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Moving /root/.local/share/uv/python/.cache/.tmp66Csrb/python to /root/.local/share/uv/python/cpython-3.8.20-linux-x86_64-gnu
DEBUG Released lock at `/root/.local/share/uv/python/.lock`
Using CPython 3.8.20
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: my-dependency @ file:///builds/my/project/path
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched `requires-dist` for: `my-dependency==0.1.0`
  Expected: {Requirement { name: PackageName("my-dependency"), extras: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.1.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/api/v4/projects/7777/packages/pypi/simple", query: None, fragment: None }) }, origin: None }}
  Actual: {Requirement { name: PackageName("my-dependency"), extras: [], marker: true, source: Git { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "my/dependency/project/path", query: None, fragment: None }, reference: DefaultBranch, precise: None, subdirectory: None, url: VerbatimUrl { url: Url { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "my/dependency/project/path", query: None, fragment: None }, given: None } }, origin: None }}
DEBUG Inserting Git reference into resolver: `RepositoryReference { url: RepositoryUrl(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "my/dependency/project/path", query: None, fragment: None }), reference: DefaultBranch }` at `8322677054c7c1906e1942a59f17375ef1edc625`
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8
DEBUG Adding direct dependency: my-dependency*
DEBUG Searching for a compatible version of my-dependency @ file:///builds/my/project/path (*)
DEBUG Adding transitive dependency for my-dependency==0.1.0: my-dependency==0.1.0
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
DEBUG Searching for a compatible version of my-dependency (==0.1.0)
DEBUG No compatible version found for: my-dependency
DEBUG Searching for a compatible version of my-dependency @ file:///builds/my/project/path (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: my-dependency
  × No solution found when resolving dependencies:
  ╰─▶ Because my-dependency was not found in the package registry and your
      project depends on my-dependency==0.1.0, we can conclude that your project's
      requirements are unsatisfiable.
Cleaning up project directory and file based variables
00:01
ERROR: Job failed: exit code 1
```

* The current uv version / platform

uv 0.4.26, Docker image : ghcr.io/astral-sh/uv:bookworm-slim

In other projects (same Gitlab instance), doing this works : 

`pip install my_dependency --extra-index-url https://gitlab-ci-token:$CI_JOB_TOKEN@my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple`

And as said in title using uv as in first example with a Personnal token or a "special" group-level Deploy token (https://docs.gitlab.com/ee/user/project/deploy_tokens/#gitlab-deploy-token) works as well, with the exact same pyproject.toml file, which looks like : 

```
[project]
name = "my-project"
version = "0.1.0"
description = ""
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "my-dependency == 0.1.0",
]

[tool.uv.sources]
my-dependency = { index = "my-repo" }

[[tool.uv.index]]
name = "my-repo"
url = "my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple"
default = true
```


---

_Comment by @zanieb on 2024-10-25 13:26_

Do other tools work with the other token? If authentication is working for some tokens, it seems unlikely that uv is at fault?

---

_Comment by @zanieb on 2024-10-25 13:27_

Oh sorry I see it works with `pip`? Can you set `RUST_LOG=uv=trace` to get us some more authentication flow logs?

---

_Comment by @tomschelsen on 2024-10-25 15:21_

Sure 

```
$ RUST_LOG=uv=trace UV_INDEX_MY_REPO_USERNAME=gitlab-ci-token UV_INDEX_MY_REPO_PASSWORD=$CI_JOB_TOKEN uv run --verbose my_script.py
DEBUG uv 0.4.26
DEBUG Found project root: `/builds/my/project/path`
DEBUG No workspace root found, using project root
DEBUG Discovered project `my-project` at: /builds/my/project/path
DEBUG Reading requests from `/builds/my/project/path/.python-version`
DEBUG Searching for Python 3.8 in managed installations or system path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36-9+deb12u8) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`
TRACE Searching PATH for executables: python, python3, python3.8
TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Checking `PATH` directory for interpreters: /sbin
TRACE Checking `PATH` directory for interpreters: /bin
DEBUG Requested Python not found, checking for available download...
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36-9+deb12u8) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`
TRACE Checking lock for `/root/.local/share/uv/python` at `/root/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/root/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
TRACE Handling request for https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Request for https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.cache/.tmpuGyC3A
DEBUG Extracting cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Moving /root/.local/share/uv/python/.cache/.tmpuGyC3A/python to /root/.local/share/uv/python/cpython-3.8.20-linux-x86_64-gnu
TRACE Querying interpreter executable at /root/.local/share/uv/python/cpython-3.8.20-linux-x86_64-gnu/bin/python3
DEBUG Released lock at `/root/.local/share/uv/python/.lock`
Using CPython 3.8.20
Creating virtual environment at: .venv
TRACE Caching credentials for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: my-project @ file:///builds/my/project/path
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched `requires-dist` for: `my-project==0.1.0`
  Expected: {Requirement { name: PackageName("my-dependency"), extras: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.1.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/api/v4/projects/7777/packages/pypi/simple", query: None, fragment: None }) }, origin: None }}
  Actual: {Requirement { name: PackageName("my-dependency"), extras: [], marker: true, source: Git { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/my/dependency/project/path", query: None, fragment: None }, reference: DefaultBranch, precise: None, subdirectory: None, url: VerbatimUrl { url: Url { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/my/dependency/project/path", query: None, fragment: None }, given: None } }, origin: None }}
DEBUG Inserting Git reference into resolver: `RepositoryReference { url: RepositoryUrl(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/my/dependency/project/path", query: None, fragment: None }), reference: DefaultBranch }` at `8322677054c7c1906e1942a59f17375ef1edc625`
TRACE Performing lookahead for my-project @ file:///builds/my/project/path
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8
DEBUG Adding direct dependency: my-project*
DEBUG Searching for a compatible version of my-project @ file:///builds/my/project/path (*)
DEBUG Adding transitive dependency for my-project==0.1.0: my-dependency==0.1.0
TRACE Fetching metadata for my-dependency from https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/90dddf5158c1a224/my-dependency.rkyv
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Sending fresh GET request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/ is fully authenticated
TRACE Received package metadata for: my-dependency
DEBUG Searching for a compatible version of my-dependency (==0.1.0)
TRACE Selecting candidate for my-dependency with range ==0.1.0 with 0 remote versions
DEBUG No compatible version found for: my-dependency
DEBUG Searching for a compatible version of my-project @ file:///builds/my/project/path (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: my-project
TRACE Resolver derivation tree before reduction
  root==0a0.dev0 depends on my-project*
      my-project==0.1.0 depends on my-dependency==0.1.0
      my-dependency not found in the package registry
    no versions of my-project<0.1.0 | >0.1.0
TRACE Resolver derivation tree after reduction
  my-project==0.1.0 depends on my-dependency==0.1.0
  my-dependency not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because my-dependency was not found in the package registry and your
      project depends on my-dependency==0.1.0, we can conclude that your project's
      requirements are unsatisfiable.
Cleaning up project directory and file based variables
ERROR: Job failed: exit code 1
```

---

_Comment by @tomschelsen on 2024-10-25 15:37_

And as a comparison, the one that works (with the "special" gitlab-deploy-token as in https://docs.gitlab.com/ee/user/project/deploy_tokens/#gitlab-deploy-token ).

You can see that at some point there is a `Username(Some("gitlab+deploy-token-328"))` (which is the actual username corresponding to that deploy token), whereas there is never a `Username(Some("gitlab-ci-token"))` or any Username struct in the previous non-working example.

```
$ RUST_LOG=uv=trace UV_INDEX_MY_REPO_USERNAME=$CI_DEPLOY_USER UV_INDEX_MY_REPO_PASSWORD=$CI_DEPLOY_PASSWORD uv run --verbose my_script.py
DEBUG uv 0.4.26
DEBUG Found project root: `/builds/my/project/path`
DEBUG No workspace root found, using project root
DEBUG Discovered project `my-project` at: /builds/my/project/path
DEBUG Reading requests from `/builds/my/project/path/.python-version`
DEBUG Searching for Python 3.8 in managed installations or system path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36-9+deb12u8) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`
TRACE Searching PATH for executables: python, python3, python3.8
TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /usr/sbin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Checking `PATH` directory for interpreters: /sbin
TRACE Checking `PATH` directory for interpreters: /bin
DEBUG Requested Python not found, checking for available download...
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36-9+deb12u8) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`
TRACE Checking lock for `/root/.local/share/uv/python` at `/root/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/root/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
TRACE Handling request for https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Request for https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /root/.local/share/uv/python/.cache/.tmpf2LMA5
DEBUG Extracting cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Moving /root/.local/share/uv/python/.cache/.tmpf2LMA5/python to /root/.local/share/uv/python/cpython-3.8.20-linux-x86_64-gnu
TRACE Querying interpreter executable at /root/.local/share/uv/python/cpython-3.8.20-linux-x86_64-gnu/bin/python3
DEBUG Released lock at `/root/.local/share/uv/python/.lock`
Using CPython 3.8.20
Creating virtual environment at: .venv
TRACE Caching credentials for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: my-project @ file:///builds/my/project/path
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched `requires-dist` for: `my-project==0.1.0`
  Expected: {Requirement { name: PackageName("my-dependency"), extras: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "0.1.0" }]), index: Some(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/api/v4/projects/7777/packages/pypi/simple", query: None, fragment: None }) }, origin: None }}
  Actual: {Requirement { name: PackageName("my-dependency"), extras: [], marker: true, source: Git { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/my/dependency/project/path", query: None, fragment: None }, reference: DefaultBranch, precise: None, subdirectory: None, url: VerbatimUrl { url: Url { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/my/dependency/project/path", query: None, fragment: None }, given: None } }, origin: None }}
DEBUG Inserting Git reference into resolver: `RepositoryReference { url: RepositoryUrl(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my.gitlab.domain")), port: None, path: "/my/dependency/project/path", query: None, fragment: None }), reference: DefaultBranch }` at `8322677054c7c1906e1942a59f17375ef1edc625`
TRACE Performing lookahead for my-project @ file:///builds/my/project/path
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8
DEBUG Adding direct dependency: my-project*
DEBUG Searching for a compatible version of my-project @ file:///builds/my/project/path (*)
DEBUG Adding transitive dependency for my-project==0.1.0: my-dependency==0.1.0
TRACE Fetching metadata for my-dependency from https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/90dddf5158c1a224/my-dependency.rkyv
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Sending fresh GET request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/ is fully authenticated
TRACE cached request https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/my-dependency/ is storable because this is a shared cache and its response has a 'private' cache-control directive
TRACE Received package metadata for: my-dependency
DEBUG Searching for a compatible version of my-dependency (==0.1.0)
TRACE Using preference my-dependency 0.1.0
DEBUG Selecting: my-dependency==0.1.0 [preference] (my-dependency-0.1.0-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/90dddf5158c1a224/my-dependency/my-dependency-0.1.0-py3-none-any.msgpack
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Sending fresh HEAD request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e is unauthenticated, checking cache
TRACE No credentials in cache for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Attempting unauthenticated request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://my.gitlab.domain
TRACE Retrying request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e with credentials from cache Credentials { username: Username(Some("gitlab+deploy-token-328")), password: Some("[MASKED]") }
TRACE cached request https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e is storable because its response has a heuristically cacheable status code 200
TRACE Getting metadata for my-dependency-0.1.0-py3-none-any.whl by range request
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e is unauthenticated, checking cache
TRACE No credentials in cache for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Attempting unauthenticated request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://my.gitlab.domain
TRACE Retrying request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl#sha256=5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e with credentials from cache Credentials { username: Username(Some("gitlab+deploy-token-328")), password: Some("[MASKED]") }
TRACE Received built distribution metadata for: my-dependency==0.1.0
DEBUG Adding transitive dependency for my-dependency==0.1.0: num2words>=0.5.13
TRACE Fetching metadata for num2words from https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/90dddf5158c1a224/num2words.rkyv
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/
TRACE Sending fresh GET request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/ is fully authenticated
TRACE cached request https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/num2words/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: num2words
TRACE Using preference num2words 0.5.13
DEBUG Searching for a compatible version of num2words (>=0.5.13)
TRACE Using preference num2words 0.5.13
DEBUG Selecting: num2words==0.5.13 [preference] (num2words-0.5.13-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/90dddf5158c1a224/num2words/num2words-0.5.13-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: num2words==0.5.13
DEBUG Adding transitive dependency for num2words==0.5.13: docopt>=0.6.2
TRACE Fetching metadata for docopt from https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/90dddf5158c1a224/docopt.rkyv
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/
TRACE Sending fresh GET request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/ is fully authenticated
TRACE cached request https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/docopt/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: docopt
TRACE Using preference docopt 0.6.2
DEBUG Searching for a compatible version of docopt (>=0.6.2)
TRACE Using preference docopt 0.6.2
DEBUG Selecting: docopt==0.6.2 [preference] (docopt-0.6.2.tar.gz)
TRACE Checking lock for `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2` at `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2/.lock`
DEBUG Acquired lock for `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2`
TRACE No cache entry exists for /root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2/revision.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz
TRACE Handling request for https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz
TRACE Request for https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz
TRACE cached request https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz is storable because its response has a 'public' cache-control directive
DEBUG Downloading source distribution: docopt==0.6.2
DEBUG No static `pyproject.toml` available for: docopt==0.6.2 (MissingPyprojectToml)
DEBUG No static `PKG-INFO` available for: docopt==0.6.2 (PkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG No static `egg-info` available for: docopt==0.6.2 (MissingRequiresTxt)
DEBUG Preparing metadata for: docopt==0.6.2
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8.20
DEBUG Adding direct dependency: setuptools>=40.8.0
TRACE Fetching metadata for setuptools from https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/90dddf5158c1a224/setuptools.rkyv
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/
TRACE Sending fresh GET request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/ is fully authenticated
TRACE cached request https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/ is storable because its response has a 'public' cache-control directive
WARN Skipping file for setuptools: setuptools-0.6b1-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b1-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6b2-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b2-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6b3-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b3-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6b4-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b4-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c1-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c1-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c10-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c10-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c10-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c10-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c10-py2.6.egg
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.6.exe
WARN Skipping file for setuptools: setuptools-0.6c11-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c11-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.6.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.7.egg
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.6.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.7.exe
WARN Skipping file for setuptools: setuptools-0.6c2-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c2-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c3-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c3-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c3-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c4-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c4-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c4-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c4-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c4.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c4.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c4.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c5-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c5-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c5-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c5-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c5.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c5.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c5.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c6-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c6-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c6-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c6-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c6.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c6.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c6.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c7-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c7-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c7-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c7-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c7.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c7.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c7.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c8-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c8-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c8-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c8-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c8.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c8.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c8.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c9-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c9-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c9-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c9-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c9-py2.6.egg
WARN Skipping file for setuptools: setuptools-0.6c9.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c9.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c9.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-18.3.1-py3.4.egg
TRACE Received package metadata for: setuptools
TRACE Selecting candidate for setuptools with range >=40.8.0 with 581 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "75.2.0" version
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
TRACE Selecting candidate for setuptools with range >=40.8.0 with 581 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "75.2.0" version
DEBUG Selecting: setuptools==75.2.0 [compatible] (setuptools-75.2.0-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/90dddf5158c1a224/setuptools/setuptools-75.2.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: setuptools==75.2.0
DEBUG Tried 1 versions: setuptools 1
DEBUG my-dependency specific environment resolution took 0.131s
TRACE Resolution: <matches all marker environments>
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 75.2.0
DEBUG Installing in setuptools==75.2.0 in /root/.cache/uv/builds-v0/.tmpEwQiZL
DEBUG Identified uncached distribution: setuptools==75.2.0
DEBUG Downloading and building requirement for build: setuptools==75.2.0
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/90dddf5158c1a224/setuptools/setuptools-75.2.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl
TRACE cached request https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
DEBUG Installing build requirement: setuptools==75.2.0
TRACE Extracting file name=PackageName("setuptools")
TRACE Extracted 554 files name=PackageName("setuptools")
TRACE No entrypoints name=PackageName("setuptools")
TRACE No data name=PackageName("setuptools")
TRACE Writing extra metadata name=PackageName("setuptools")
TRACE Writing record name=PackageName("setuptools")
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG running egg_info
DEBUG writing docopt.egg-info/PKG-INFO
DEBUG writing dependency_links to docopt.egg-info/dependency_links.txt
DEBUG writing top-level names to docopt.egg-info/top_level.txt
DEBUG reading manifest file 'docopt.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE-MIT'
DEBUG writing manifest file 'docopt.egg-info/SOURCES.txt'
DEBUG Calling `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel()`
DEBUG running dist_info
DEBUG creating /root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt.egg-info
DEBUG writing /root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt.egg-info/PKG-INFO
DEBUG writing dependency_links to /root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt.egg-info/dependency_links.txt
DEBUG writing top-level names to /root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt.egg-info/top_level.txt
DEBUG writing manifest file '/root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt.egg-info/SOURCES.txt'
DEBUG reading manifest file '/root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE-MIT'
DEBUG writing manifest file '/root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt.egg-info/SOURCES.txt'
DEBUG creating '/root/.cache/uv/builds-v0/.tmpEwQiZL/metadata_directory/docopt-0.6.2.dist-info'
DEBUG The [wheel] section is deprecated. Use [bdist_wheel] instead.
DEBUG /root/.cache/uv/builds-v0/.tmpEwQiZL/lib/python3.8/site-packages/setuptools/_distutils/cmd.py:111: SetuptoolsDeprecationWarning: bdist_wheel.universal is deprecated
DEBUG !!
DEBUG 
DEBUG         ********************************************************************************
DEBUG         With Python 2.7 end-of-life, support for building universal wheels
DEBUG         (i.e., wheels that support both Python 2 and Python 3)
DEBUG         is being obviated.
DEBUG         Please discontinue using this option, or if you still need it,
DEBUG         file an issue with pypa/setuptools describing your use case.
DEBUG 
DEBUG         By 2025-Aug-30, you need to update your project and remove deprecated calls
DEBUG         or your builds will no longer be supported.
DEBUG         ********************************************************************************
DEBUG 
DEBUG !!
DEBUG   self.finalize_options()
DEBUG Prepared metadata for: docopt==0.6.2
DEBUG Released lock at `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2/.lock`
TRACE Received source distribution metadata for: docopt==0.6.2
DEBUG Tried 4 versions: docopt 1, num2words 1, my-dependency 1, my-project 1
DEBUG my-dependency universal resolution took 1.572s
TRACE Resolution: <matches all marker environments>
TRACE Resolution edge: my-project -> my-dependency
TRACE Resolution edge:     0.1.0 -> 0.1.0
TRACE Resolution edge: num2words -> docopt
TRACE Resolution edge:     0.5.13 -> 0.6.2
TRACE Resolution edge: ROOT -> my-project
TRACE Resolution edge:     0a0.dev0 -> 0.1.0
TRACE Resolution edge: my-dependency -> num2words
TRACE Resolution edge:     0.1.0 -> 0.5.13
Resolved 4 packages in 1.57s
TRACE Caching credentials for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: docopt==0.6.2
DEBUG Identified uncached distribution: num2words==0.5.13
DEBUG Identified uncached distribution: my-dependency==0.1.0
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/90dddf5158c1a224/my-dependency/my-dependency-0.1.0-py3-none-any.http
TRACE Checking lock for `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2` at `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2/.lock`
DEBUG No cache entry for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl
DEBUG Acquired lock for `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2`
TRACE Sending fresh GET request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl
TRACE Handling request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/90dddf5158c1a224/num2words/num2words-0.5.13-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl
TRACE cached request https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz
DEBUG Building: docopt==0.6.2
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8.20
DEBUG Adding direct dependency: setuptools>=40.8.0
TRACE Fetching metadata for setuptools from https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/
TRACE cached request https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE Selecting candidate for setuptools with range >=40.8.0 with 581 remote versions
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "75.2.0" version
TRACE Selecting candidate for setuptools with range >=40.8.0 with 581 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "75.2.0" version
DEBUG Selecting: setuptools==75.2.0 [compatible] (setuptools-75.2.0-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/31/2d/90165d51ecd38f9a02c6832198c13a4e48652485e2ccf863ebb942c531b6/setuptools-75.2.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==75.2.0
DEBUG Tried 1 versions: setuptools 1
DEBUG my-dependency specific environment resolution took 0.003s
TRACE Resolution: <matches all marker environments>
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 75.2.0
DEBUG Installing in setuptools==75.2.0 in /root/.cache/uv/builds-v0/.tmpDWhyUD
DEBUG Requirement already cached: setuptools==75.2.0
DEBUG Installing build requirement: setuptools==75.2.0
TRACE Extracting file name=PackageName("setuptools")
TRACE Extracted 554 files name=PackageName("setuptools")
TRACE No entrypoints name=PackageName("setuptools")
TRACE No data name=PackageName("setuptools")
TRACE Writing extra metadata name=PackageName("setuptools")
TRACE Writing record name=PackageName("setuptools")
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
TRACE Request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://my.gitlab.domain
TRACE Retrying request for https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl with credentials from cache Credentials { username: Username(Some("gitlab+deploy-token-328")), password: Some("[MASKED]") }
TRACE cached request https://files.pythonhosted.org/packages/8f/f0/ca1228af2bcbce2fdf2b23d58643c84253b88a3c1cd9dba391ca683c4b21/num2words-0.5.13-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://my.gitlab.domain/api/v4/projects/7777/packages/pypi/files/5e6332492f1f1ec089ce9fbb515c7cdbfc02cc507c23b8c4bc76215c144f759e/my-dependency-0.1.0-py3-none-any.whl is storable because this is a shared cache and its response has a 'private' cache-control directive
DEBUG running egg_info
DEBUG writing docopt.egg-info/PKG-INFO
DEBUG writing dependency_links to docopt.egg-info/dependency_links.txt
DEBUG writing top-level names to docopt.egg-info/top_level.txt
DEBUG reading manifest file 'docopt.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE-MIT'
DEBUG writing manifest file 'docopt.egg-info/SOURCES.txt'
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2/r9g19hdqaPnDMCS9_SDSD/.tmpQlY4IB", {}, None)`
DEBUG running bdist_wheel
DEBUG The [wheel] section is deprecated. Use [bdist_wheel] instead.
DEBUG running build
DEBUG running build_py
DEBUG creating build/lib
DEBUG copying docopt.py -> build/lib
DEBUG installing to build/bdist.linux-x86_64/wheel
DEBUG running install
DEBUG running install_lib
DEBUG creating build/bdist.linux-x86_64/wheel
DEBUG copying build/lib/docopt.py -> build/bdist.linux-x86_64/wheel/.
DEBUG running install_egg_info
DEBUG running egg_info
DEBUG writing docopt.egg-info/PKG-INFO
DEBUG writing dependency_links to docopt.egg-info/dependency_links.txt
DEBUG writing top-level names to docopt.egg-info/top_level.txt
DEBUG reading manifest file 'docopt.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE-MIT'
DEBUG writing manifest file 'docopt.egg-info/SOURCES.txt'
DEBUG Copying docopt.egg-info to build/bdist.linux-x86_64/wheel/./docopt-0.6.2-py3.8.egg-info
DEBUG running install_scripts
DEBUG creating build/bdist.linux-x86_64/wheel/docopt-0.6.2.dist-info/WHEEL
DEBUG creating '/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2/r9g19hdqaPnDMCS9_SDSD/.tmpQlY4IB/.tmp-iz4iadzf/docopt-0.6.2-py2.py3-none-any.whl' and adding 'build/bdist.linux-x86_64/wheel' to it
DEBUG adding 'docopt.py'
DEBUG adding 'docopt-0.6.2.dist-info/LICENSE-MIT'
DEBUG adding 'docopt-0.6.2.dist-info/METADATA'
DEBUG adding 'docopt-0.6.2.dist-info/WHEEL'
DEBUG adding 'docopt-0.6.2.dist-info/top_level.txt'
DEBUG adding 'docopt-0.6.2.dist-info/RECORD'
DEBUG removing build/bdist.linux-x86_64/wheel
DEBUG /root/.cache/uv/builds-v0/.tmpDWhyUD/lib/python3.8/site-packages/setuptools/_distutils/cmd.py:111: SetuptoolsDeprecationWarning: bdist_wheel.universal is deprecated
DEBUG !!
DEBUG 
DEBUG         ********************************************************************************
DEBUG         With Python 2.7 end-of-life, support for building universal wheels
DEBUG         (i.e., wheels that support both Python 2 and Python 3)
DEBUG         is being obviated.
DEBUG         Please discontinue using this option, or if you still need it,
DEBUG         file an issue with pypa/setuptools describing your use case.
DEBUG 
DEBUG         By 2025-Aug-30, you need to update your project and remove deprecated calls
DEBUG         or your builds will no longer be supported.
DEBUG         ********************************************************************************
DEBUG 
DEBUG !!
DEBUG   self.finalize_options()
DEBUG Finished building: docopt==0.6.2
DEBUG Released lock at `/root/.cache/uv/sdists-v5/index/90dddf5158c1a224/docopt/0.6.2/.lock`
Prepared 3 packages in 571ms
TRACE Extracting file name=PackageName("my-dependency")
TRACE Extracting file name=PackageName("num2words")
TRACE Extracting file name=PackageName("docopt")
DEBUG Failed to hardlink `/builds/my/project/path/.venv/lib/python3.8/site-packages/docopt.py` to `/root/.cache/uv/archive-v0/Af1xPG-BOzI7zCW1Kr3uH/docopt.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/builds/my/project/path/.venv/lib/python3.8/site-packages/my-dependency/__init__.py` to `/root/.cache/uv/archive-v0/8yEdFmS5K6VBjZc_iNs3r/my-dependency/__init__.py`, attempting to copy files as a fallback
DEBUG Failed to hardlink `/builds/my/project/path/.venv/lib/python3.8/site-packages/num2words/lang_FR_DZ.py` to `/root/.cache/uv/archive-v0/Ii1VgmrSMu_t3U0_B9hJS/num2words/lang_FR_DZ.py`, attempting to copy files as a fallback
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
TRACE Extracted 6 files name=PackageName("docopt")
TRACE No entrypoints name=PackageName("docopt")
TRACE No data name=PackageName("docopt")
TRACE Writing extra metadata name=PackageName("docopt")
TRACE Writing record name=PackageName("docopt")
TRACE Extracted 18 files name=PackageName("my-dependency")
TRACE No entrypoints name=PackageName("my-dependency")
TRACE No data name=PackageName("my-dependency")
TRACE Writing extra metadata name=PackageName("my-dependency")
TRACE Writing record name=PackageName("my-dependency")
TRACE Extracted 62 files name=PackageName("num2words")
TRACE No entrypoints name=PackageName("num2words")
TRACE Installing data name=PackageName("num2words")
TRACE Writing extra metadata name=PackageName("num2words")
TRACE Writing record name=PackageName("num2words")
Installed 3 packages in 5ms
 + docopt==0.6.2
 + num2words==0.5.13
 + my-dependency==0.1.0
DEBUG Using Python 3.8.20 interpreter at: /builds/my/project/path/.venv/bin/python
DEBUG Running `python my_script.py`
Hello world
DEBUG Command exited with code: 0
Cleaning up project directory and file based variables
00:01
Job succeeded
```

---

_Comment by @tomschelsen on 2024-10-25 16:03_

Alright nevermind that was a Gitlab permission issue. Really sorry for the inconvenience, and thanks for your time. Closing it

---

_Closed by @tomschelsen on 2024-10-25 16:03_

---

_Comment by @ihelmer07 on 2025-02-07 01:30_

@tomschelsen - what was the gitlab permission you adjusted to make it work?

---

_Comment by @tomschelsen on 2025-02-07 09:49_

In a project Settings > CI/CD > Job Tokens, you can set from which other projects/groups you accept Job Tokens from. 

---

_Referenced in [astral-sh/uv#13815](../../astral-sh/uv/issues/13815.md) on 2025-06-03 14:22_

---

```yaml
number: 17619
title: Cache control headers ignored for 404 response from package registry
type: issue
state: open
author: Henri-J-Norden
labels:
  - bug
assignees: []
created_at: 2026-01-20T00:52:22Z
updated_at: 2026-01-20T01:10:45Z
url: https://github.com/astral-sh/uv/issues/17619
synced_at: 2026-01-20T01:38:40Z
```

# Cache control headers ignored for 404 response from package registry

---

_@Henri-J-Norden_

### Summary

Hello,

I have a private package registry, but would like to avoid replicating public dependencies from PyPi to the private registry or proxying requests for unknown packages through the private registry to PyPi. This can be configured easily by just setting the `UV_INDEX` env var - by default, `uv` automatically falls back to PyPi when a package is not found in the private registry. 

However, when running the same package multiple times in a row with `uvx`, there is a noticeable pause during every execution for dependency resolution. It is especially noticeable for larger packages with many dependencies.

This seems to be caused by `uv` never caching negative responses from the private registry. Essentially the control header is ignored, when the private registry responds with HTTP404 for any of the dependencies (that are always expected to be on PyPi). 

This is surprising behavior, as I expected `uv` to cache the 404 response (until max-age has elapsed) and immediately fall back to the default index (and use a cached response from the default index, if any). If negative caching is not supported by design (even behind a cli flag or env var), then I feel like `uv` should at least print a warning when it ignores the cache control header (instead of dropping it silently). The [documentation](https://docs.astral.sh/uv/concepts/indexes/#customizing-cache-control-headers) could also be improved, since it's not clear what should happen in more complex cases.

---

# Example
For the following example, I have built (`uv build --sdist`) and published (`uv publish`) a single package to the private registry: `private`.

### private/pyproject.toml
```toml
[project]
name = "private"
version = "0.1.0"
dependencies = ["example_pypi_package"]  # public package, only on PyPi
```

The private registry sets the `Cache-Control` header to `public, max-age=600` in every response, if the request is not POST, PUT or PATCH.
```shell
$ curl -X GET -I http://localhost:8000/packages/pypi/simple/example_pypi_package/
HTTP/1.1 404 Not Found
Server: Werkzeug/3.1.5 Python/3.13.11
Date: Mon, 19 Jan 2026 23:56:28 GMT
Content-Type: application/json
Vary: Accept, Cookie
Allow: GET, HEAD, OPTIONS
Cache-Control: public, max-age=600
X-Frame-Options: DENY
Content-Length: 48
X-Content-Type-Options: nosniff
Referrer-Policy: same-origin
Cross-Origin-Opener-Policy: same-origin
djdt-request-id: e83c7f15fd594fc4aa266192e191c427
Server-Timing: TimerPanel_utime;dur=11.525999999996372;desc="User CPU time", TimerPanel_stime;dur=2.85199999999719;desc="System CPU time", TimerPanel_total;dur=14.377999999993563;desc="Total CPU time", TimerPanel_total_time;dur=25.12750199821312;desc="Elapsed time", SQLPanel_sql_time;dur=1.8284719990333542;desc="SQL 1 queries", CachePanel_total_time;dur=0;desc="Cache 0 Calls"
Connection: close
```

The private registry is part of a proprietary program, so I am unable to share the source code.

## Log snippets
With `UV_INDEX=http://__token__:***@localhost:8000/packages/pypi/simple/`, I executed `uvx -vvv private` twice in a row - the log below is from the second execution.

Notice how `private` is loaded from the cache:
```shell
TRACE Response from http://localhost:8000/packages/pypi/simple/private/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: http://localhost:8000/packages/pypi/simple/private/
```

... but the `example-pypi-package` response from the private registry is not cached:
```shell
DEBUG No cache entry for: http://localhost:8000/packages/pypi/simple/example-pypi-package/
DEBUG Sending fresh GET request for: http://localhost:8000/packages/pypi/simple/example-pypi-package/
TRACE Handling request for http://__token__:****@localhost:8000/packages/pypi/simple/example-pypi-package/ with authentication policy auto
TRACE Request for http://__token__:****@localhost:8000/packages/pypi/simple/example-pypi-package/ already contains username and password
TRACE checkout waiting for idle connection: ("http", localhost:8000)
DEBUG starting new connection: http://localhost:8000/
TRACE Http::connect; scheme=Some("http"), host=Some("localhost"), port=Some(Port(8000))
DEBUG connecting to [::1]:8000
DEBUG connected to [::1]:8000
TRACE http1 handshake complete, spawning background dispatcher task
TRACE waiting for connection to be ready
TRACE connection is ready
TRACE checkout dropped for ("http", localhost:8000)
TRACE Received response for fresh request with status 404 Not Found for: http://localhost:8000/packages/pypi/simple/example-pypi-package/
TRACE Considering retry of response HTTP 404 Not Found for http://localhost:8000/packages/pypi/simple/example-pypi-package/
TRACE Cannot retry nested reqwest error
TRACE Fetching metadata for example-pypi-package from https://pypi.org/simple/example-pypi-package/
TRACE Response from https://pypi.org/simple/example-pypi-package/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/example-pypi-package/
```

## Full log
<details>

```shell
$ uvx -V
uvx 0.9.26 (ee4f00362 2026-01-15)

$ uvx -vvv private
DEBUG uv 0.9.26 (ee4f00362 2026-01-15)
TRACE Checking lock for `/home/henri/.cache/uv` at `/home/henri/.cache/uv/.lock`
DEBUG Acquired shared lock for `/home/henri/.cache/uv`
DEBUG Using request timeout of 30s
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/henri/.local/share/uv/python`
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to determine libc
TRACE stdout output from `ld.so --version`: "ld.so (GNU libc) stable release version 2.42.\nCopyright (C) 2025 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.42 in stdout of ld.so version
DEBUG Found managed installation `cpython-3.13.11-linux-x86_64-gnu`
TRACE Found cached interpreter info for Python 3.13.11, skipping query of: /home/henri/.local/share/uv/python/cpython-3.13.11-linux-x86_64-gnu/bin/python3.13
DEBUG Found `cpython-3.13.11-linux-x86_64-gnu` at `/home/henri/.local/share/uv/python/cpython-3.13.11-linux-x86_64-gnu/bin/python3.13` (managed installations)
TRACE Checking lock for `/home/henri/.local/share/uv/tools` at `/home/henri/.local/share/uv/tools/.lock`
DEBUG Acquired exclusive lock for `/home/henri/.local/share/uv/tools`
DEBUG Checking for Python environment at: `/home/henri/.local/share/uv/tools/private`
DEBUG Released lock at `/home/henri/.local/share/uv/tools/.lock`
DEBUG Assessing Python executable as base candidate: /home/henri/.local/share/uv/python/cpython-3.13.11-linux-x86_64-gnu/bin/python3.13
DEBUG Caching via base interpreter: `/home/henri/.local/share/uv/python/cpython-3.13.11-linux-x86_64-gnu/bin/python3.13`
TRACE Read credentials for index http://__token__:****@localhost:8000/packages/pypi/simple/
TRACE Caching credentials for http://__token__:****@localhost:8000/packages/pypi
TRACE Caching credentials for http://__token__:****@localhost:8000/packages/pypi/simple/
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.11
DEBUG Solving with target Python version: >=3.13.11
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: private*
INFO add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: private. remaining choices: 
TRACE Fetching metadata for private from http://__token__:****@localhost:8000/packages/pypi/simple/private/
TRACE Response from http://localhost:8000/packages/pypi/simple/private/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: http://localhost:8000/packages/pypi/simple/private/
TRACE Received package metadata for: private
TRACE Selecting candidate for private with range * with 1 remote versions
DEBUG Searching for a compatible version of private (*)
TRACE Selecting candidate for private with range * with 1 remote versions
TRACE Found candidate for package private with range * after 1 steps: 0.1.0 version
TRACE Returning candidate for package private with range * after 1 steps
TRACE Found candidate for package private with range * after 1 steps: 0.1.0 version
TRACE Returning candidate for package private with range * after 1 steps
DEBUG Selecting: private==0.1.0 [compatible] (private-0.1.0.tar.gz)
TRACE Checking lock for `/home/henri/.cache/uv/sdists-v9/index/c1deb61519d057b2/private/0.1.0` at `/home/henri/.cache/uv/sdists-v9/index/c1deb61519d057b2/private/0.1.0/.lock`
DEBUG Acquired exclusive lock for `/home/henri/.cache/uv/sdists-v9/index/c1deb61519d057b2/private/0.1.0`
TRACE Response from http://localhost:8000/packages/pypi/download/private/private-0.1.0.tar.gz is storable because it has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 86400s
DEBUG Found fresh response for: http://localhost:8000/packages/pypi/download/private/private-0.1.0.tar.gz
DEBUG Found static `pyproject.toml` for: private==0.1.0
DEBUG Released lock at `/home/henri/.cache/uv/sdists-v9/index/c1deb61519d057b2/private/0.1.0/.lock`
TRACE Received source distribution metadata for: private==0.1.0
DEBUG Adding transitive dependency for private==0.1.0: example-pypi-package*
INFO add_decision: Id::<PubGrubPackage>(1) @ 0.1.0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0, private==0.1.0
TRACE Chose package for decision: example-pypi-package. remaining choices: 
TRACE Fetching metadata for example-pypi-package from http://__token__:****@localhost:8000/packages/pypi/simple/example-pypi-package/
TRACE No cache entry exists for /home/henri/.cache/uv/simple-v18/index/c1deb61519d057b2/example-pypi-package.rkyv
DEBUG No cache entry for: http://localhost:8000/packages/pypi/simple/example-pypi-package/
DEBUG Sending fresh GET request for: http://localhost:8000/packages/pypi/simple/example-pypi-package/
TRACE Handling request for http://__token__:****@localhost:8000/packages/pypi/simple/example-pypi-package/ with authentication policy auto
TRACE Request for http://__token__:****@localhost:8000/packages/pypi/simple/example-pypi-package/ already contains username and password
TRACE checkout waiting for idle connection: ("http", localhost:8000)
DEBUG starting new connection: http://localhost:8000/
TRACE Http::connect; scheme=Some("http"), host=Some("localhost"), port=Some(Port(8000))
DEBUG connecting to [::1]:8000
DEBUG connected to [::1]:8000
TRACE http1 handshake complete, spawning background dispatcher task
TRACE waiting for connection to be ready
TRACE connection is ready
TRACE checkout dropped for ("http", localhost:8000)
TRACE Received response for fresh request with status 404 Not Found for: http://localhost:8000/packages/pypi/simple/example-pypi-package/
TRACE Considering retry of response HTTP 404 Not Found for http://localhost:8000/packages/pypi/simple/example-pypi-package/
TRACE Cannot retry nested reqwest error
TRACE Fetching metadata for example-pypi-package from https://pypi.org/simple/example-pypi-package/
TRACE Response from https://pypi.org/simple/example-pypi-package/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/example-pypi-package/
TRACE Received package metadata for: example-pypi-package
TRACE Selecting candidate for example-pypi-package with range * with 1 remote versions
DEBUG Searching for a compatible version of example-pypi-package (*)
TRACE Selecting candidate for example-pypi-package with range * with 1 remote versions
TRACE Found candidate for package example-pypi-package with range * after 1 steps: 0.1.0 version
TRACE Found candidate for package example-pypi-package with range * after 1 steps: 0.1.0 version
TRACE Returning candidate for package example-pypi-package with range * after 1 steps
TRACE Returning candidate for package example-pypi-package with range * after 1 steps
DEBUG Selecting: example-pypi-package==0.1.0 [compatible] (example_pypi_package-0.1.0-py3-none-any.whl)
TRACE Response from https://files.pythonhosted.org/packages/5c/1b/413af2998cc5930ad77331da8de618bc2e83feee034a6b36dc5122804b4f/example_pypi_package-0.1.0-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5c/1b/413af2998cc5930ad77331da8de618bc2e83feee034a6b36dc5122804b4f/example_pypi_package-0.1.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: example-pypi-package==0.1.0
INFO add_decision: Id::<PubGrubPackage>(2) @ 0.1.0 without checking dependencies
TRACE Assigned packages: root==0a0.dev0, private==0.1.0, example-pypi-package==0.1.0
DEBUG Tried 2 versions: example-pypi-package 1, private 1
DEBUG marker environment resolution took 0.054s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.13.11", version: "3.13.11" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.18.6-2-cachyos", platform_system: "Linux", platform_version: "#1 SMP PREEMPT_DYNAMIC Sun, 18 Jan 2026 01:31:44 +0000", python_full_version: StringVersion { string: "3.13.11", version: "3.13.11" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> private
TRACE Resolution edge:     0a0.dev0 -> 0.1.0
TRACE Resolution edge: private -> example-pypi-package
TRACE Resolution edge:     0.1.0 -> 0.1.0
Resolved 2 packages in 54ms
DEBUG Checking for Python environment at: `/home/henri/.cache/uv/archive-v0/2vKDRN-FsuGZddQQnMP_i`
TRACE Found cached interpreter info for Python 3.13.11, skipping query of: /home/henri/.cache/uv/archive-v0/2vKDRN-FsuGZddQQnMP_i/bin/python3
DEBUG Looking at `.dist-info` at: /home/henri/.cache/uv/archive-v0/2vKDRN-FsuGZddQQnMP_i/lib/python3.13/site-packages/example_pypi_package-0.1.0.dist-info
DEBUG Looking at `.dist-info` at: /home/henri/.cache/uv/archive-v0/2vKDRN-FsuGZddQQnMP_i/lib/python3.13/site-packages/private-0.1.0.dist-info
DEBUG Looking at `.dist-info` at: /home/henri/.cache/uv/archive-v0/2vKDRN-FsuGZddQQnMP_i/lib/python3.13/site-packages/private-0.1.0.dist-info
Package `private` does not provide any executables.
DEBUG Released lock at `/home/henri/.cache/uv/.lock`
```

</details>

### Platform

Linux 6.18.6-2-cachyos x86_64 GNU/Linux

### Version

uv 0.9.26 (ee4f00362 2026-01-15)

### Python version

Python 3.13.11

---

_Label `bug` added by @Henri-J-Norden on 2026-01-20 00:52_

---

_Renamed from "Cache control headers ignored for 404 response from package repo" to "Cache control headers ignored for 404 response from package registry" by @Henri-J-Norden on 2026-01-20 01:09_

---

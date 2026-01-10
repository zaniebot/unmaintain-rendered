---
number: 12061
title: "`uv add` hangs on `setuptools.build_meta:__legacy__.get_requires_for_build_wheel`"
type: issue
state: open
author: rllin
labels:
  - external
assignees: []
created_at: 2025-03-08T00:40:22Z
updated_at: 2025-07-21T12:12:07Z
url: https://github.com/astral-sh/uv/issues/12061
synced_at: 2026-01-10T01:25:14Z
---

# `uv add` hangs on `setuptools.build_meta:__legacy__.get_requires_for_build_wheel`

---

_Issue opened by @rllin on 2025-03-08 00:40_

### Summary

## repro
```bash
uv init
uv add apache-beam
uv add datasets
```

## trace
I can't seem to get more logs out of this
```
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/17/5151b6ac2ac9b6276d46c33369ff814b0901872b2a0871771252f02e9192/multiprocess-0.70.9.tar.gz
   Building multiprocess==0.70.9
DEBUG Building: multiprocess==0.70.9
TRACE Preview is disabled, not checking for direct build
DEBUG Using base executable for virtual environment: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.8
DEBUG Solving with target Python version: >=3.12.8
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: setuptools>=40.8.0
INFO add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
TRACE Fetching metadata for setuptools from https://pypi.org/simple/setuptools/
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: setuptools. remaining choices:
TRACE Cached request https://pypi.org/simple/setuptools/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE Selecting candidate for setuptools with range >=40.8.0 with 589 remote versions
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
TRACE Selecting candidate for setuptools with range >=40.8.0 with 589 remote versions
TRACE Found candidate for package setuptools with range >=40.8.0 after 1 steps: 75.8.2 version
TRACE Returning candidate for package setuptools with range >=40.8.0 after 1 steps
TRACE Found candidate for package setuptools with range >=40.8.0 after 1 steps: 75.8.2 version
TRACE Returning candidate for package setuptools with range >=40.8.0 after 1 steps
DEBUG Selecting: setuptools==75.8.2 [compatible] (setuptools-75.8.2-py3-none-any.whl)
TRACE Cached request https://files.pythonhosted.org/packages/a9/38/7d7362e031bd6dc121e5081d8cb6aa6f6fedf2b67bf889962134c6da4705/setuptools-75.8.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a9/38/7d7362e031bd6dc121e5081d8cb6aa6f6fedf2b67bf889962134c6da4705/setuptools-75.8.2-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==75.8.2
INFO add_decision: Id::<PubGrubPackage>(1) @ 75.8.2 without checking dependencies
TRACE Assigned packages: root==0a0.dev0, setuptools==75.8.2
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.001s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.8", version: "3.12.8" }, os_name: "posix", platform_machine: "arm64", platform_python_implementation: "CPython", platform_release: "24.3.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.3.0: Thu Jan  2 20:24:22 PST 2025; root:xnu-11215.81.4~3/RELEASE_ARM64_T6041", python_full_version: StringVersion { string: "3.12.8", version: "3.12.8" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 75.8.2
DEBUG Installing in setuptools==75.8.2 in /Users/randall/.cache/uv/builds-v0/.tmpWAr3l3
DEBUG Registry requirement already cached: setuptools==75.8.2
DEBUG Installing build requirement: setuptools==75.8.2
TRACE Extracting file name=PackageName("setuptools")
TRACE Cloning /Users/randall/.cache/uv/archive-v0/7RlhyIIdWf5xI6NlX_nsg/setuptools-75.8.2.dist-info to /Users/randall/.cache/uv/builds-v0/.tmpWAr3l3/lib/python3.12/site-packages/setuptools-75.8.2.dist-info
TRACE Cloning /Users/randall/.cache/uv/archive-v0/7RlhyIIdWf5xI6NlX_nsg/distutils-precedence.pth to /Users/randall/.cache/uv/builds-v0/.tmpWAr3l3/lib/python3.12/site-packages/distutils-precedence.pth
TRACE Cloning /Users/randall/.cache/uv/archive-v0/7RlhyIIdWf5xI6NlX_nsg/setuptools to /Users/randall/.cache/uv/builds-v0/.tmpWAr3l3/lib/python3.12/site-packages/setuptools
TRACE Cloning /Users/randall/.cache/uv/archive-v0/7RlhyIIdWf5xI6NlX_nsg/pkg_resources to /Users/randall/.cache/uv/builds-v0/.tmpWAr3l3/lib/python3.12/site-packages/pkg_resources
TRACE Cloning /Users/randall/.cache/uv/archive-v0/7RlhyIIdWf5xI6NlX_nsg/_distutils_hack to /Users/randall/.cache/uv/builds-v0/.tmpWAr3l3/lib/python3.12/site-packages/_distutils_hack
TRACE Extracted 5 files name=PackageName("setuptools")
TRACE No entrypoints name=PackageName("setuptools")
TRACE No data name=PackageName("setuptools")
TRACE Writing installer metadata name=PackageName("setuptools")
TRACE Writing record name=PackageName("setuptools")
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
```

### Platform

macos

### Version

0.6.5

### Python version

3.12.8

---

_Label `bug` added by @rllin on 2025-03-08 00:40_

---

_Comment by @zanieb on 2025-03-08 00:55_

Historically, this is usually a bug in setuptools

---

_Comment by @zanieb on 2025-03-08 00:56_

Like

- https://github.com/astral-sh/uv/issues/11238
- https://github.com/pypa/setuptools/issues/332



---

_Comment by @rllin on 2025-03-08 01:00_

@zanieb thanks for quick response!

seems like
``` uv run pip install --use-pep517 --no-cache --force-reinstall "datasets"```
this works fine, am i maybe misunderstanding?

---

_Comment by @zanieb on 2025-03-08 01:19_

The error seems to be on "multiprocess==0.70.9"? What happens if you install that in isolation?

---

_Comment by @zanieb on 2025-03-08 01:21_

`uv add` could resolve to a different version than `uv pip install` or `pip install` because it's universal resolution rather than single-platform _and_ `pip` won't into account previously installed packages so it doesn't care that you also require `apache-beam` there.

ref https://docs.astral.sh/uv/concepts/resolution/#platform-specific-resolution

---

_Comment by @rllin on 2025-03-08 01:48_

`multiprocess==0.70.9` alone also hangs similarly!

---

_Comment by @zanieb on 2025-03-08 14:25_

It seems like a problem with that package, you could add a constraint that avoids it? Like

```toml
[tool.uv]
constraint-dependencies = ["multiprocess>0.70.9"]
```

---

_Comment by @wchen99998 on 2025-07-18 16:23_

still running into this issue, can't install apache_beam at all

---

_Label `bug` removed by @konstin on 2025-07-21 12:12_

---

_Label `external` added by @konstin on 2025-07-21 12:12_

---

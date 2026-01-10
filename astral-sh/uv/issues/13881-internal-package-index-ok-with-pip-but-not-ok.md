---
number: 13881
title: internal package index OK with pip but NOT OK with UV
type: issue
state: closed
author: rv2931
labels:
  - question
assignees: []
created_at: 2025-06-06T11:00:17Z
updated_at: 2025-06-06T11:25:18Z
url: https://github.com/astral-sh/uv/issues/13881
synced_at: 2026-01-10T01:25:39Z
---

# internal package index OK with pip but NOT OK with UV

---

_Issue opened by @rv2931 on 2025-06-06 11:00_

### Question

Hello
I have configured PIP to use our internal Nexus Package index
Si I created a pip conf file:
```
# .venv/pip.conf
[global]
index = https://user:password@mynexus.repository.local/repository/python-group/pypi
index-url = https://user:password@mynexus.repository.local/repository/python-group/simple
trusted-host = mynexus.repository.local
```

With this configuration, `pip install pandas` works well and install correctly pandas package, the pnexus proxy downloading it from official internet and caching it. So everything works fine as expected

When configuring uv with equivalent

```
[[tool.uv.index]]
name= "nexus"
url = "https://user:password@mynexus.repository.local/repository/python-group/simple"
default = true
```

Doing the command `uv add pandas`gives me the error :
```
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of pandas and your project depends on pandas, we can conclude that your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

With verbose;
```
DEBUG uv 0.7.11
DEBUG Found project root: `/myproject`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/myproject`
DEBUG Reading Python requests from version file at `/myproject/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.11`
DEBUG Released lock at `/tmp/uv-b7dc2882d908d0fe.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-nexus @ file:///myproject
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.11.11
DEBUG Solving with target Python version: >=3.11
DEBUG Adding direct dependency: test-nexus*
DEBUG Searching for a compatible version of test-nexus @ file:///myproject (*)
DEBUG Adding direct dependency: pandas*
DEBUG Found stale response for: https://mynexus.repository.local/repository/python-group/pypi/simple/pandas/
DEBUG Sending revalidation request for: https://mynexus.repository.local/repository/python-group/pypi/simple/pandas/
DEBUG Found modified response for: https://mynexus.repository.local/repository/python-group/pypi/simple/pandas/
DEBUG Unexpected fragment (expected `#sha256=...` or similar) on URL: browse/browse:python-group
WARN Skipping file for pandas:
WARN Skipping file for pandas:
WARN Skipping file for pandas: ..
DEBUG Searching for a compatible version of pandas (*)
DEBUG No compatible version found for: pandas
DEBUG Recording unit propagation conflict of pandas from incompatibility of (test-nexus)
DEBUG Searching for a compatible version of test-nexus @ file:///myproject (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: myproject
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of pandas and your project depends on pandas, we can conclude that your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

It is quite suprising as the pandas 2.3.0 package is available in the nexus package repository as it was previously downloaded by pip command that was working

Do I have forget something to configure internal index package repository with UV ?

Thank you in advance
RV

### Platform

Windows 10 + WSL Ubuntu

### Version

uv 0.7.11

---

_Label `question` added by @rv2931 on 2025-06-06 11:00_

---

_Comment by @konstin on 2025-06-06 11:05_

> DEBUG Unexpected fragment (expected `#sha256=...` or similar) on URL: browse/browse:python-group

uv can't parse the page at https://mynexus.repository.local/repository/python-group/pypi/simple/pandas/, is it possible for you to share the html, or at least a redacted URL around a `browse/browse:python-group` fragment in it?

---

_Comment by @rv2931 on 2025-06-06 11:19_

https://mynexus.repository.local/repository/python-group/pypi/simple/pandas/ give me an HTTP 404 error

in the nexus python-group, I have a list of folders containing
- python-group/simple: that looks to be the pypi root repository. this folder contains all index files for all packages in the cache
- python-group/ : seems to be the root folder of packages. So I can find one folder per package/version/archive.zip

I tried to set uv url= https://mynexus.repository.local/repository/python-group/pypi/ instead of https://mynexus.repository.local/repository/python-group/pypi/simple and https://mynexus.repository.local/repository/python-group
but nothing works
The error is the same `Unexpected fragment (expected `#sha256=...` or similar) on URL: browse/browse:python-group`

---

_Comment by @rv2931 on 2025-06-06 11:23_

Oh, my mistake
it's working
I think there was a `/pypi` in the path that souldn't be there

---

_Comment by @rv2931 on 2025-06-06 11:25_

SO yes, it was a bad path... Why I didn't use the same as pip ? I don't know
another bad trick of my chair or my keyboard
Thank you for answering

---

_Closed by @rv2931 on 2025-06-06 11:25_

---

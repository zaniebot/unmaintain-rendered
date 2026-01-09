---
number: 10832
title: no-manylinux support does not work when cache is enabled
type: issue
state: closed
author: pythonweb2
labels:
  - bug
assignees: []
created_at: 2025-01-21T23:05:39Z
updated_at: 2025-01-23T19:17:35Z
url: https://github.com/astral-sh/uv/issues/10832
synced_at: 2026-01-07T13:12:18-06:00
---

# no-manylinux support does not work when cache is enabled

---

_Issue opened by @pythonweb2 on 2025-01-21 23:05_

### Summary

Reference: https://docs.astral.sh/uv/pip/compatibility/#manylinux_compatible-enforcement

I have noticed that when the cache is not disabled, no-manylinux being installed won't make a difference. For example:

```bash
# uv venv
# uv pip install no-manylinux
# uv pip install cryptography
Resolved 3 packages in 144ms
Installed 3 packages in 23ms
 + cffi==1.17.1
 + cryptography==44.0.0
 + pycparser==2.22
# uv pip install --no-cache cryptography
Resolved 3 packages in 460ms
   Built cffi==1.17.1
Building cryptography==44.0.0
⠧ Preparing packages... (2/3)
```

It seems like regardless of whether or not the cache is disabled, this should work and build the package from source.

### Platform

Ubuntu 20.04

### Version

uv 0.5.22

### Python version

Python 3.8

---

_Label `bug` added by @pythonweb2 on 2025-01-21 23:05_

---

_Comment by @zanieb on 2025-01-21 23:08_

Can you share `--verbose` logs? Perhaps with `RUST_LOG=uv=trace` set?

---

_Comment by @pythonweb2 on 2025-01-21 23:09_

Verbose logs for the first command (with cache)

```
root@9fffee1db09a:~/test2# uv pip list
Package      Version
------------ -------
no-manylinux 3.0.0
root@9fffee1db09a:~/test2# RUST_LOG=uv=trace uv pip install --verbose cryptography
DEBUG uv 0.5.22
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.13.1, skipping probing: .venv/bin/python3
DEBUG Found `cpython-3.13.1-linux-x86_64-gnu` at `/root/test2/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.13.1 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: cryptography
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.13.1
TRACE assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: cryptography*
TRACE assigned packages: root==0a0.dev0
TRACE Chose package for decision: cryptography. remaining choices: 
TRACE Fetching metadata for cryptography from https://pypi.org/simple/cryptography/
TRACE cached request https://pypi.org/simple/cryptography/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
TRACE cached request https://pypi.org/simple/cryptography/ has a cached response that does not allow staleness
TRACE request https://pypi.org/simple/cryptography/ does not have a fresh cache because its age is 880 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.org/simple/cryptography/
DEBUG Sending revalidation request for: https://pypi.org/simple/cryptography/
TRACE Handling request for https://pypi.org/simple/cryptography/
TRACE Request for https://pypi.org/simple/cryptography/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/cryptography/
TRACE Attempting unauthenticated request for https://pypi.org/simple/cryptography/
TRACE not modified because old and new etag values ([34, 109, 57, 77, 69, 49, 56, 90, 83, 68, 48, 54, 81, 99, 107, 43, 114, 81, 49, 55, 101, 102, 65, 34]) match
DEBUG Found not-modified response for: https://pypi.org/simple/cryptography/
TRACE Received package metadata for: cryptography
TRACE Selecting candidate for cryptography with range * with 134 remote versions
DEBUG Searching for a compatible version of cryptography (*)
TRACE Selecting candidate for cryptography with range * with 134 remote versions
TRACE Found candidate for package cryptography with range * after 1 steps: 44.0.0 version
TRACE Found candidate for package cryptography with range * after 1 steps: 44.0.0 version
TRACE Returning candidate for package cryptography with range * after 1 steps
TRACE Returning candidate for package cryptography with range * after 1 steps
DEBUG Selecting: cryptography==44.0.0 [compatible] (cryptography-44.0.0-cp39-abi3-manylinux_2_28_x86_64.whl)
TRACE cached request https://files.pythonhosted.org/packages/ef/82/72403624f197af0db6bac4e58153bc9ac0e6020e57234115db9596eee85d/cryptography-44.0.0-cp39-abi3-manylinux_2_28_x86_64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ef/82/72403624f197af0db6bac4e58153bc9ac0e6020e57234115db9596eee85d/cryptography-44.0.0-cp39-abi3-manylinux_2_28_x86_64.whl.metadata
TRACE Received built distribution metadata for: cryptography==44.0.0
DEBUG Adding transitive dependency for cryptography==44.0.0: cffi{platform_python_implementation != 'PyPy'}>=1.12
TRACE assigned packages: root==0a0.dev0, cryptography==44.0.0
TRACE Chose package for decision: cffi{platform_python_implementation != 'PyPy'}. remaining choices: 
TRACE Fetching metadata for cffi from https://pypi.org/simple/cffi/
TRACE cached request https://pypi.org/simple/cffi/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
TRACE cached request https://pypi.org/simple/cffi/ has a cached response that does not allow staleness
TRACE request https://pypi.org/simple/cffi/ does not have a fresh cache because its age is 879 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.org/simple/cffi/
DEBUG Sending revalidation request for: https://pypi.org/simple/cffi/
TRACE Handling request for https://pypi.org/simple/cffi/
TRACE Request for https://pypi.org/simple/cffi/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/cffi/
TRACE Attempting unauthenticated request for https://pypi.org/simple/cffi/
TRACE not modified because old and new etag values ([34, 52, 79, 70, 72, 55, 117, 108, 50, 78, 81, 122, 70, 67, 48, 113, 54, 55, 120, 86, 112, 87, 103, 34]) match
DEBUG Found not-modified response for: https://pypi.org/simple/cffi/
TRACE Received package metadata for: cffi
DEBUG Searching for a compatible version of cffi{platform_python_implementation != 'PyPy'} (>=1.12)
TRACE Selecting candidate for cffi with range >=1.12 with 78 remote versions
TRACE Found candidate for package cffi with range >=1.12 after 1 steps: 1.17.1 version
TRACE Returning candidate for package cffi with range >=1.12 after 1 steps
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: cffi==1.17.1
DEBUG Adding transitive dependency for cffi==1.17.1: cffi{platform_python_implementation != 'PyPy'}==1.17.1
TRACE assigned packages: root==0a0.dev0, cryptography==44.0.0
TRACE Selecting candidate for cffi with range ==1.17.1 with 78 remote versions
TRACE Found candidate for package cffi with range ==1.17.1 after 1 steps: 1.17.1 version
TRACE Returning candidate for package cffi with range ==1.17.1 after 1 steps
TRACE Chose package for decision: cffi. remaining choices: cffi{platform_python_implementation != 'PyPy'}
DEBUG Searching for a compatible version of cffi (==1.17.1)
TRACE Selecting candidate for cffi with range ==1.17.1 with 78 remote versions
TRACE Found candidate for package cffi with range ==1.17.1 after 1 steps: 1.17.1 version
TRACE Returning candidate for package cffi with range ==1.17.1 after 1 steps
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE cached request https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Received built distribution metadata for: cffi==1.17.1
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
TRACE assigned packages: root==0a0.dev0, cryptography==44.0.0, cffi==1.17.1
TRACE Chose package for decision: cffi{platform_python_implementation != 'PyPy'}. remaining choices: pycparser
DEBUG Searching for a compatible version of cffi{platform_python_implementation != 'PyPy'} (==1.17.1)
TRACE Fetching metadata for pycparser from https://pypi.org/simple/pycparser/
TRACE Selecting candidate for cffi with range ==1.17.1 with 78 remote versions
TRACE Found candidate for package cffi with range ==1.17.1 after 1 steps: 1.17.1 version
TRACE Returning candidate for package cffi with range ==1.17.1 after 1 steps
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
TRACE assigned packages: root==0a0.dev0, cryptography==44.0.0, cffi==1.17.1, cffi{platform_python_implementation != 'PyPy'}==1.17.1
TRACE Chose package for decision: pycparser. remaining choices: 
TRACE cached request https://pypi.org/simple/pycparser/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
TRACE cached request https://pypi.org/simple/pycparser/ has a cached response that does not allow staleness
TRACE request https://pypi.org/simple/pycparser/ does not have a fresh cache because its age is 879 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.org/simple/pycparser/
DEBUG Sending revalidation request for: https://pypi.org/simple/pycparser/
TRACE Handling request for https://pypi.org/simple/pycparser/
TRACE Request for https://pypi.org/simple/pycparser/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pycparser/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pycparser/
TRACE not modified because old and new etag values ([34, 48, 99, 74, 102, 97, 100, 47, 80, 101, 80, 66, 114, 76, 51, 43, 101, 83, 103, 53, 71, 99, 65, 34]) match
DEBUG Found not-modified response for: https://pypi.org/simple/pycparser/
TRACE Received package metadata for: pycparser
TRACE Selecting candidate for pycparser with range * with 22 remote versions
DEBUG Searching for a compatible version of pycparser (*)
TRACE Selecting candidate for pycparser with range * with 22 remote versions
TRACE Found candidate for package pycparser with range * after 1 steps: 2.22 version
TRACE Found candidate for package pycparser with range * after 1 steps: 2.22 version
TRACE Returning candidate for package pycparser with range * after 1 steps
DEBUG Selecting: pycparser==2.22 [compatible] (pycparser-2.22-py3-none-any.whl)
TRACE Returning candidate for package pycparser with range * after 1 steps
TRACE cached request https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: pycparser==2.22
TRACE assigned packages: root==0a0.dev0, cryptography==44.0.0, cffi==1.17.1, cffi{platform_python_implementation != 'PyPy'}==1.17.1, pycparser==2.22
DEBUG Tried 3 versions: cffi 1, cryptography 1, pycparser 1
DEBUG marker environment resolution took 0.151s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.13.1", version: "3.13.1" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "5.15.153.1-microsoft-standard-WSL2", platform_system: "Linux", platform_version: "#1 SMP Fri Mar 29 23:14:13 UTC 2024", python_full_version: StringVersion { string: "3.13.1", version: "3.13.1" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> cryptography
TRACE Resolution edge:     0a0.dev0 -> 44.0.0
TRACE Resolution edge: cffi -> pycparser
TRACE Resolution edge:     1.17.1 -> 2.22
TRACE Resolution edge: cryptography -> cffi
TRACE Resolution edge:     44.0.0 -> 1.17.1 ; platform_python_implementation != 'PyPy'
Resolved 3 packages in 155ms
DEBUG Requirement already cached: cryptography==44.0.0
DEBUG Requirement already cached: cffi==1.17.1
DEBUG Requirement already cached: pycparser==2.22
DEBUG Unnecessary package: no-manylinux==3.0.0
TRACE Extracting file name=PackageName("cryptography")
TRACE Extracting file name=PackageName("cffi")
TRACE Extracting file name=PackageName("pycparser")
TRACE Extracted 29 files name=PackageName("cffi")
TRACE No entrypoints name=PackageName("cffi")
TRACE No data name=PackageName("cffi")
TRACE Writing installer metadata name=PackageName("cffi")
TRACE Writing record name=PackageName("cffi")
TRACE Extracted 23 files name=PackageName("pycparser")
TRACE No entrypoints name=PackageName("pycparser")
TRACE No data name=PackageName("pycparser")
TRACE Writing installer metadata name=PackageName("pycparser")
TRACE Writing record name=PackageName("pycparser")
TRACE Extracted 111 files name=PackageName("cryptography")
TRACE No entrypoints name=PackageName("cryptography")
TRACE No data name=PackageName("cryptography")
TRACE Writing installer metadata name=PackageName("cryptography")
TRACE Writing record name=PackageName("cryptography")
Installed 3 packages in 31ms
 + cffi==1.17.1
 + cryptography==44.0.0
 + pycparser==2.22
DEBUG Released lock at `/root/test2/.venv/.lock`
```

---

_Comment by @pythonweb2 on 2025-01-21 23:12_

And here are the logs for the second command, they are longer so I attached a file. 

[scratch_82.txt](https://github.com/user-attachments/files/18497513/scratch_82.txt)

---

_Comment by @charliermarsh on 2025-01-21 23:12_

This is returned as part of the interpreter metadata, I'm guessing it gets cached and we don't really know to invalidate it? 

---

_Comment by @pythonweb2 on 2025-01-21 23:17_

I also tested this with python 3.8 using the pre-packaged python from ubuntu-20.04, and this was still an issue

---

_Comment by @charliermarsh on 2025-01-21 23:18_

Yeah, that shouldn't make a difference.

Does running with `--refresh` resolve it?

---

_Comment by @pythonweb2 on 2025-01-21 23:20_

Yes, that does look like it also resolves the issue, similar to `--no-cache`.

---

_Referenced in [astral-sh/uv#10361](../../astral-sh/uv/pulls/10361.md) on 2025-01-21 23:48_

---

_Comment by @charliermarsh on 2025-01-23 19:17_

I'm going to mark this as wontfix. It's an incorrect behavior, but no-manylinux is very niche and I'd prefer not to make changes to our caching strategies on its behalf. `--refresh` is the recommended workaround.

---

_Closed by @charliermarsh on 2025-01-23 19:17_

---

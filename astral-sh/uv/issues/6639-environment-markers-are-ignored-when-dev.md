---
number: 6639
title: Environment markers are ignored when dev dependencies are specified in the root workspace package
type: issue
state: closed
author: Gr1N
labels: []
assignees: []
created_at: 2024-08-26T08:53:16Z
updated_at: 2024-08-26T12:22:55Z
url: https://github.com/astral-sh/uv/issues/6639
synced_at: 2026-01-10T01:24:03Z
---

# Environment markers are ignored when dev dependencies are specified in the root workspace package

---

_Issue opened by @Gr1N on 2024-08-26 08:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hey! During the migration from Rye to the latest uv (0.3.3) release, I found an issue with environment markers being ignored in the dev dependencies specification in the root package.

You can reproduce it by creating a new package with workspaces:
```
$ tree .
.
├── README.md
├── packages
│   └── uvplay
│       ├── README.md
│       ├── pyproject.toml
│       └── src
│           └── uvplay
│               └── __init__.py
├── pyproject.toml
└── uv.lock

5 directories, 6 files

$ cat pyproject.toml
[tool.uv]
managed = true

dev-dependencies = ["requests; platform_system == 'Windows'"]

[tool.uv.workspace]
members = ["packages/*"]

$ cat packages/uvplay/pyproject.toml
[project]
name = "uvplay"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[tool.uv]
managed = true

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

The just run sync (in my case on MacOS):
```
$ uv --version
uv 0.3.3 (deea6025a 2024-08-23)

$ uv sync -vv
    0.002216s DEBUG uv uv 0.3.3
    0.004477s DEBUG uv_workspace::workspace Found project root: `/Users/ng/Temp/uvplay`
    0.004981s DEBUG uv_workspace::workspace Adding discovered workspace member: `/Users/ng/Temp/uvplay/packages/uvplay`
    0.008706s DEBUG uv::commands::project The virtual environment's Python version satisfies `Python >=3.11`
 uv_client::linehaul::linehaul
    0.012727s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
    0.015905s DEBUG uv::commands::project::lock Starting clean resolution
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=uvplay @ file:///Users/ng/Temp/uvplay/packages/uvplay
    0.021169s DEBUG uv_fs Acquired lock for `/Users/ng/Library/Caches/uv/built-wheels-v3/editable/c881729a04e450cf`
    0.022592s   2ms DEBUG uv_distribution::source No static `PKG-INFO` available for: uvplay @ file:///Users/ng/Temp/uvplay/packages/uvplay (MissingPkgInfo)
    0.023725s   3ms DEBUG uv_distribution::source Found static `pyproject.toml` for: uvplay @ file:///Users/ng/Temp/uvplay/packages/uvplay
    0.024198s   4ms DEBUG uv_workspace::workspace Found workspace root: `/Users/ng/Temp/uvplay`
    0.024422s   4ms DEBUG uv_workspace::workspace Adding current workspace member: `/Users/ng/Temp/uvplay/packages/uvplay`
 uv_resolver::resolver::solve
    0.026390s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.11.1
    0.026455s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.11
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.028737s   2ms DEBUG uv_resolver::resolver Adding direct dependency: requests{platform_system == 'Windows'}*
    0.029031s   2ms DEBUG uv_resolver::resolver Adding direct dependency: uvplay*
 uv_resolver::resolver::process_request request=Versions requests
   uv_resolver::resolver::choose_version package=uvplay
      0.029474s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of uvplay @ file:///Users/ng/Temp/uvplay/packages/uvplay (*)
   uv_resolver::resolver::get_dependencies_forking package=uvplay, version=0.1.0
     uv_resolver::resolver::get_dependencies package=uvplay, version=0.1.0
   uv_resolver::resolver::choose_version package=requests{platform_system == 'Windows'}
   uv_client::registry_client::simple_api package=requests
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/requests.rkyv
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/simple-v12/pypi/requests.rkyv"
        0.032530s   2ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/requests/
        0.032655s   2ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/requests/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/requests/"
        0.159459s 128ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/requests/
       uv_client::cached_client::refresh_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/requests.rkyv
   uv_resolver::version_map::from_metadata
      0.163409s 133ms DEBUG uv_resolver::resolver Searching for a compatible version of requests{platform_system == 'Windows'} (*)
      0.163891s 134ms DEBUG uv_resolver::resolver Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies_forking package=requests{platform_system == 'Windows'}, version=2.32.3
     uv_resolver::resolver::get_dependencies package=requests{platform_system == 'Windows'}, version=2.32.3
    0.164099s 137ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: requests==2.32.3
    0.164107s 137ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: requests{platform_system == 'Windows'}==2.32.3
   uv_resolver::resolver::choose_version package=requests{platform_system == 'Windows'}
 uv_resolver::resolver::process_request request=Prefetch requests ==2.32.3
      0.164198s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of requests{platform_system == 'Windows'} (==2.32.3)
      0.164209s   0ms DEBUG uv_resolver::resolver Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=requests==2.32.3
     uv_client::registry_client::wheel_metadata built_dist=requests==2.32.3
   uv_resolver::resolver::get_dependencies_forking package=requests{platform_system == 'Windows'}, version=2.32.3
     uv_resolver::resolver::get_dependencies package=requests{platform_system == 'Windows'}, version=2.32.3
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/wheels-v1/pypi/requests/requests-2.32.3-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/wheels-v1/pypi/requests/requests-2.32.3-py3-none-any.msgpack"
            0.165027s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/f9/9b/335f9764261e915ed497fcdeb11df5dfd6f7bf257d4a6a2a686d80da4d54/requests-2.32.3-py3-none-any.whl.metadata
    0.166057s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
    0.166073s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
    0.166080s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
    0.166085s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
   uv_resolver::resolver::choose_version package=requests
      0.166137s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of requests (==2.32.3)
      0.166144s   0ms DEBUG uv_resolver::resolver Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies_forking package=requests, version=2.32.3
     uv_resolver::resolver::get_dependencies package=requests, version=2.32.3
    0.166177s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
    0.166183s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
    0.166187s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
    0.166261s 139ms DEBUG uv_resolver::resolver Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
 uv_resolver::resolver::process_request request=Versions certifi
   uv_resolver::resolver::choose_version package=certifi
   uv_client::registry_client::simple_api package=certifi
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/certifi.rkyv
 uv_resolver::resolver::process_request request=Versions charset-normalizer
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/simple-v12/pypi/certifi.rkyv"
   uv_client::registry_client::simple_api package=charset-normalizer
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/charset-normalizer.rkyv
 uv_resolver::resolver::process_request request=Versions idna
   uv_client::registry_client::simple_api package=idna
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/idna.rkyv
 uv_resolver::resolver::process_request request=Versions urllib3
   uv_client::registry_client::simple_api package=urllib3
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/urllib3.rkyv
 uv_resolver::resolver::process_request request=Prefetch urllib3 >=1.21.1, <3
 uv_resolver::resolver::process_request request=Prefetch idna >=2.5, <4
 uv_resolver::resolver::process_request request=Prefetch charset-normalizer >=2, <4
 uv_resolver::resolver::process_request request=Prefetch certifi >=2017.4.17
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/simple-v12/pypi/charset-normalizer.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/simple-v12/pypi/idna.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/simple-v12/pypi/urllib3.rkyv"
        0.166970s   0ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/certifi/
        0.166981s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/certifi/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/certifi/"
        0.167230s   0ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/idna/
        0.167249s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/idna/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/idna/"
        0.167978s   1ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/urllib3/
        0.167989s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/urllib3/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/urllib3/"
        0.168451s   2ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/charset-normalizer/
        0.168461s   2ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/charset-normalizer/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/charset-normalizer/"
        0.201930s  35ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/certifi/
       uv_client::cached_client::refresh_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/certifi.rkyv
   uv_resolver::version_map::from_metadata
      0.202802s  36ms DEBUG uv_resolver::resolver Searching for a compatible version of certifi (>=2017.4.17)
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=certifi==2024.7.4
     uv_client::registry_client::wheel_metadata built_dist=certifi==2024.7.4
      0.202844s  36ms DEBUG uv_resolver::resolver Selecting: certifi==2024.7.4 [compatible] (certifi-2024.7.4-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies_forking package=certifi, version=2024.7.4
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/wheels-v1/pypi/certifi/certifi-2024.7.4-py3-none-any.msgpack
     uv_resolver::resolver::get_dependencies package=certifi, version=2024.7.4
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/wheels-v1/pypi/certifi/certifi-2024.7.4-py3-none-any.msgpack"
            0.203312s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/1c/d5/c84e1a17bf61d4df64ca866a1c9a913874b4e9bdc131ec689a0ad013fb36/certifi-2024.7.4-py3-none-any.whl.metadata
   uv_resolver::resolver::choose_version package=charset-normalizer
        0.232808s  66ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/idna/
       uv_client::cached_client::refresh_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/idna.rkyv
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=idna==3.8
     uv_client::registry_client::wheel_metadata built_dist=idna==3.8
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/wheels-v1/pypi/idna/idna-3.8-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/wheels-v1/pypi/idna/idna-3.8-py3-none-any.msgpack"
            0.234819s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/22/7e/d71db821f177828df9dea8c42ac46473366f191be53080e552e628aad991/idna-3.8-py3-none-any.whl.metadata
        0.264182s  97ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/charset-normalizer/
       uv_client::cached_client::refresh_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/charset-normalizer.rkyv
        0.265490s  98ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/urllib3/
       uv_client::cached_client::refresh_cache file=/Users/ng/Library/Caches/uv/simple-v12/pypi/urllib3.rkyv
   uv_resolver::version_map::from_metadata
      0.269982s  66ms DEBUG uv_resolver::resolver Searching for a compatible version of charset-normalizer (>=2, <4)
      0.270978s  67ms DEBUG uv_resolver::resolver Selecting: charset-normalizer==3.3.2 [compatible] (charset_normalizer-3.3.2-cp311-cp311-macosx_10_9_universal2.whl)
   uv_resolver::resolver::get_dependencies_forking package=charset-normalizer, version=3.3.2
     uv_resolver::resolver::get_dependencies package=charset-normalizer, version=3.3.2
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=urllib3==2.2.2
     uv_client::registry_client::wheel_metadata built_dist=urllib3==2.2.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/wheels-v1/pypi/urllib3/urllib3-2.2.2-py3-none-any.msgpack
 uv_resolver::resolver::process_request request=Metadata charset-normalizer==3.3.2
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=charset-normalizer==3.3.2
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/wheels-v1/pypi/urllib3/urllib3-2.2.2-py3-none-any.msgpack"
     uv_client::registry_client::wheel_metadata built_dist=charset-normalizer==3.3.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/ng/Library/Caches/uv/wheels-v1/pypi/charset-normalizer/charset_normalizer-3.3.2-cp311-cp311-macosx_10_9_universal2.msgpack
 uv_client::cached_client::from_path_sync path="/Users/ng/Library/Caches/uv/wheels-v1/pypi/charset-normalizer/charset_normalizer-3.3.2-cp311-cp311-macosx_10_9_universal2.msgpack"
            0.272503s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/ca/1c/89ffc63a9605b583d5df2be791a27bc1a42b7c32bab68d3c8f2f73a98cd4/urllib3-2.2.2-py3-none-any.whl.metadata
            0.272726s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/68/77/02839016f6fbbf808e8b38601df6e0e66c17bbab76dff4613f7511413597/charset_normalizer-3.3.2-cp311-cp311-macosx_10_9_universal2.whl.metadata
   uv_resolver::resolver::choose_version package=idna
      0.272860s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of idna (>=2.5, <4)
      0.272883s   0ms DEBUG uv_resolver::resolver Selecting: idna==3.8 [compatible] (idna-3.8-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies_forking package=idna, version=3.8
     uv_resolver::resolver::get_dependencies package=idna, version=3.8
   uv_resolver::resolver::choose_version package=urllib3
      0.272958s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of urllib3 (>=1.21.1, <3)
      0.272966s   0ms DEBUG uv_resolver::resolver Selecting: urllib3==2.2.2 [compatible] (urllib3-2.2.2-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies_forking package=urllib3, version=2.2.2
     uv_resolver::resolver::get_dependencies package=urllib3, version=2.2.2
    0.273281s 246ms DEBUG uv_resolver::resolver::batch_prefetch Tried 6 versions: certifi 1, charset-normalizer 1, idna 1, requests 1, urllib3 1, uvplay 1
    0.273298s 246ms DEBUG uv_resolver::resolver Split universal resolution took 0.247s
Resolved 6 packages in 264ms
 uv_client::linehaul::linehaul
    0.279666s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
    0.282825s DEBUG uv_installer::plan Requirement already installed: certifi==2024.7.4
    0.282846s DEBUG uv_installer::plan Requirement already installed: charset-normalizer==3.3.2
    0.282852s DEBUG uv_installer::plan Requirement already installed: idna==3.8
    0.282858s DEBUG uv_installer::plan Requirement already installed: requests==2.32.3
    0.282864s DEBUG uv_installer::plan Requirement already installed: urllib3==2.2.2
    0.283062s DEBUG uv_installer::plan Requirement already installed: uvplay==0.1.0 (from file:///Users/ng/Temp/uvplay/packages/uvplay)
Audited 6 packages in 1ms

$ uv pip list
Package            Version  Editable project location
------------------ -------- -------------------------------------
certifi            2024.7.4
charset-normalizer 3.3.2
idna               3.8
requests           2.32.3
urllib3            2.2.2
uvplay             0.1.0    /Users/ng/Temp/uvplay/packages/uvplay
```

Without a workspace setup, everything would be working as expected.

---

_Comment by @charliermarsh on 2024-08-26 12:15_

Sorry about that -- I fixed it last night in #6620. Should go out today.

---

_Closed by @charliermarsh on 2024-08-26 12:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-26 12:15_

---

_Comment by @Gr1N on 2024-08-26 12:22_

Thanks!

---

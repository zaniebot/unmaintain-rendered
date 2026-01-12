```yaml
number: 4465
title: Index Strategy ignored for distributions packages
type: issue
state: closed
author: MattiasDC
labels: []
assignees: []
created_at: 2024-06-24T10:11:37Z
updated_at: 2024-06-24T12:03:40Z
url: https://github.com/astral-sh/uv/issues/4465
synced_at: 2026-01-12T15:58:50Z
```

# Index Strategy ignored for distributions packages

---

_@MattiasDC_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
It seems that uv is ignoring index strategies for installing distribution packages. E.g. when executing the next command: `UV_INDEX_STRATEGY=unsafe-best-match uv pip install pdoc==0.11.0 --no-cache -vvv`

Uv decides to use version `46.1.3` (minimal version is `40.8.0`) which is the latest version on `custom.pypi.index`, however on the `https://pypi.org` index there is already version `70`. Relevant part of the debug log:
```
 uv_resolver::resolver::solve 
      0.058848s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.5
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.059061s   0ms DEBUG uv_resolver::resolver Adding direct dependency: pdoc3*
   uv_resolver::resolver::choose_version package=pdoc3
     uv_resolver::resolver::package_wait package_name=pdoc3
 uv_resolver::resolver::process_request request=Versions pdoc3
   uv_client::registry_client::simple_api package=pdoc3
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpUKz5XB/simple-v7/5ee1fc4f1a888b63/pdoc3.rkyv
 uv_resolver::resolver::process_request request=Prefetch pdoc3 *
 uv_client::cached_client::from_path_sync path="/tmp/.tmpUKz5XB/simple-v7/5ee1fc4f1a888b63/pdoc3.rkyv"
          0.060462s   1ms DEBUG uv_client::cached_client No cache entry for: https://custom.pypi.index/pypi/PyPi/simple/pdoc3/
       uv_client::cached_client::fresh_request url="https://custom.pypi.index/pypi/PyPi/simple/pdoc3/"
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpUKz5XB/simple-v7/pypi/pdoc3.rkyv
 uv_client::cached_client::from_path_sync path="/tmp/.tmpUKz5XB/simple-v7/pypi/pdoc3.rkyv"
          0.217432s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/pdoc3/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/pdoc3/"
       uv_client::cached_client::new_cache file=/tmp/.tmpUKz5XB/simple-v7/pypi/pdoc3.rkyv
       uv_client::registry_client::parse_simple_api package=pdoc3
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pdoc3==0.11.0
     uv_client::cached_client::get_serde 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpUKz5XB/built-wheels-v3/pypi/pdoc3/0.11.0/revision.http
        0.234654s 175ms DEBUG uv_resolver::resolver Searching for a compatible version of pdoc3 (*)
        0.234711s 175ms DEBUG uv_resolver::resolver Selecting: pdoc3==0.11.0 (pdoc3-0.11.0.tar.gz)
 uv_client::cached_client::from_path_sync path="/tmp/.tmpUKz5XB/built-wheels-v3/pypi/pdoc3/0.11.0/revision.http"
   uv_resolver::resolver::get_dependencies package=pdoc3, version=0.11.0
     uv_resolver::resolver::distributions_wait version_id=pdoc3-0.11.0
            0.234939s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/e1/4e/741d6dbd64934c4769c055dd6952e1c6d117a1a25236f633e68ea2d375c8/pdoc3-0.11.0.tar.gz
         uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/e1/4e/741d6dbd64934c4769c055dd6952e1c6d117a1a25236f633e68ea2d375c8/pdoc3-0.11.0.tar.gz"
         uv_client::cached_client::new_cache file=/tmp/.tmpUKz5XB/built-wheels-v3/pypi/pdoc3/0.11.0/revision.http
         uv_distribution::source::download source_dist=pdoc3==0.11.0
              0.280857s   0ms DEBUG uv_distribution::source Downloading source distribution: pdoc3==0.11.0
           uv_distribution::source::download_source_dist filename="pdoc3-0.11.0.tar.gz", source_dist=pdoc3==0.11.0
     uv_distribution::source::build_metadata dist=pdoc3==0.11.0
          0.307510s   0ms DEBUG uv_distribution::source Preparing metadata for: pdoc3==0.11.0
          0.307617s   0ms DEBUG uv_distribution::source No static `PKG-INFO` available for: pdoc3==0.11.0 (DynamicPkgInfo(UnsupportedMetadataVersion("2.1")))
          0.307663s   0ms DEBUG uv_distribution::source No static `pyproject.toml` available for: pdoc3==0.11.0 (MissingPyprojectToml)
       uv_dispatch::setup_build version_id="pdoc3==0.11.0", subdirectory=None
         uv_resolver::resolver::solve 
              0.308204s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.5
           uv_resolver::resolver::choose_version package=root
           uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
                0.308264s   0ms DEBUG uv_resolver::resolver Adding direct dependency: setuptools>=40.8.0
           uv_resolver::resolver::choose_version package=setuptools
             uv_resolver::resolver::package_wait package_name=setuptools
         uv_resolver::resolver::process_request request=Versions setuptools
           uv_client::registry_client::simple_api package=setuptools
             uv_client::cached_client::get_cacheable 
               uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpUKz5XB/simple-v7/5ee1fc4f1a888b63/setuptools.rkyv
         uv_resolver::resolver::process_request request=Prefetch setuptools >=40.8.0
 uv_client::cached_client::from_path_sync path="/tmp/.tmpUKz5XB/simple-v7/5ee1fc4f1a888b63/setuptools.rkyv"
                  0.308474s   0ms DEBUG uv_client::cached_client No cache entry for: https://custom.pypi.index/pypi/PyPi/simple/setuptools/
               uv_client::cached_client::fresh_request url="https://custom.pypi.index/pypi/PyPi/simple/setuptools/"
               uv_client::cached_client::new_cache file=/tmp/.tmpUKz5XB/simple-v7/5ee1fc4f1a888b63/setuptools.rkyv
               uv_client::registry_client::parse_simple_api package=setuptools
                 uv_client::html::parse url=https://custom.pypi.index/pypi/PyPi/simple/setuptools/
             uv_client::cached_client::get_cacheable 
               uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpUKz5XB/simple-v7/pypi/setuptools.rkyv
 uv_client::cached_client::from_path_sync path="/tmp/.tmpUKz5XB/simple-v7/pypi/setuptools.rkyv"
                  0.333120s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/setuptools/
               uv_client::cached_client::fresh_request url="https://pypi.org/simple/setuptools/"
               uv_client::cached_client::new_cache file=/tmp/.tmpUKz5XB/simple-v7/pypi/setuptools.rkyv
               uv_client::registry_client::parse_simple_api package=setuptools
           uv_resolver::version_map::from_metadata 
           uv_resolver::version_map::from_metadata 
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==46.1.3
             uv_client::registry_client::wheel_metadata built_dist=setuptools==46.1.3
               uv_client::cached_client::get_serde 
                 uv_client::cached_client::get_cacheable 
                   uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpUKz5XB/wheels-v1/index/5ee1fc4f1a888b63/setuptools/setuptools-46.1.3-py3-none-any.msgpack
                0.360786s  52ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (>=40.8.0)
                0.360808s  52ms DEBUG uv_resolver::resolver Selecting: setuptools==46.1.3 (setuptools-46.1.3-py3-none-any.whl)
           uv_resolver::resolver::get_dependencies package=setuptools, version=46.1.3
```



---

_Renamed from "Index Strategy ignored for backend distributions" to "Index Strategy ignored for distributions packages" by @MattiasDC on 2024-06-24 10:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-24 10:58_

---

_Comment by @charliermarsh on 2024-06-24 11:59_

Thank you, fixed in the next release.

---

_Closed by @charliermarsh on 2024-06-24 12:03_

---

_Closed by @charliermarsh on 2024-06-24 12:03_

---

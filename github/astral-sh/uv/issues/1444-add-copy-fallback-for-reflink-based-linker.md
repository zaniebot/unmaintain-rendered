---
number: 1444
title: Add copy fallback for reflink-based linker
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - macos
assignees: []
created_at: 2024-02-16T06:46:05Z
updated_at: 2024-02-22T05:01:31Z
url: https://github.com/astral-sh/uv/issues/1444
synced_at: 2026-01-07T13:12:16-06:00
---

# Add copy fallback for reflink-based linker

---

_Issue opened by @charliermarsh on 2024-02-16 06:46_

See: https://twitter.com/mpr0010/status/1758299951254388875.

---

_Label `bug` added by @charliermarsh on 2024-02-16 06:46_

---

_Comment by @humanzz on 2024-02-16 16:55_

Same issue here, and it's on a mac.

I have my python package in case-sensitive volume e.g. `/Volumnes/casesensitive` and when I try to `uv pip install -e '.[dev,testing]'`, I get an errors along the lines of
```
error: Failed to build editables
  Caused by: Failed to build editable: file:///Volumes/casesensitive/MyTestPackage
  Caused by: Failed to install requirements from build-system.requires (install)
  Caused by: Failed to install build dependencies
  Caused by: Failed to install: setuptools-69.1.0-py3-none-any.whl (setuptools==69.1.0)
  Caused by: Failed to reflink /Volumes/casesensitive/uvc/archive-v0/Zgihw_1YQkIZZNdz81Rtf/setuptools-69.1.0.dist-info to /private/var/folders/43/29_b2rv50rj6c9t29s0t5w5whc959c/T/.tmpYrzRHM/.venv/lib/python3.10/site-packages/setuptools-69.1.0.dist-info
  Caused by: Cross-device link (os error 18)
```

---

_Referenced in [astral-sh/uv#1513](../../astral-sh/uv/issues/1513.md) on 2024-02-16 17:05_

---

_Label `macos` added by @zanieb on 2024-02-16 17:51_

---

_Comment by @charliermarsh on 2024-02-16 18:53_

Thanks! This one should be straightforward to fix. (In the interim, I might suggest setting `UV_CACHE_DIR` to something in the same volume as your projects, it will also speed up installation.)

---

_Comment by @humanzz on 2024-02-16 19:02_

No worries, I'm still testing this out. I had tried `UV_CACHE_DIR` to be on the same volume as my projects.
In some attempts it worked, in others it didn't, but when it didn't, it was likely - as can be seen in the earlier logs, there's a `/private/var...` path that popped up, that I don't think I have control over - that I was trying to force no cache.

---

_Comment by @humanzz on 2024-02-18 00:14_

I looked again into it, and tried out with the newer `0.1.4` version.

Setting the cache dir to be on the same volume
1. Succeeds if I install normally `uv pip install MyTestPackage[dev,testing]`
2. Fails if I install in editable mode  `uv pip install -e '.[dev,testing]'` and the failure shows `/private/var/...`.

and verbose output in case it helps

```
UV_CACHE_DIR=.uv_cache uv pip install --verbose --reinstall -e '.[sm,dev,testing]'
 uv::requirements::from_source source=-e .[sm,dev,testing]
    0.003566s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /Volumes/casesensitive/MyTestPackage/.venv
    0.004943s DEBUG uv_interpreter::interpreter Using cached markers for: /Volumes/casesensitive/MyTestPackage/.venv/bin/python
    0.004958s DEBUG uv::commands::pip_install Using Python 3.10.13 environment at /Volumes/casesensitive/MyTestPackage/.venv/bin/python
 uv_client::flat_index::from_entries
 uv_installer::downloader::build_editables
      0.177786s   0ms DEBUG uv_distribution::source Building (editable) file:///Volumes/casesensitive/MyTestPackage
   uv_dispatch::setup_build package_id="file:///Volumes/casesensitive/MyTestPackage", subdirectory=None
     uv_resolver::resolver::solve
          0.182888s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.10.13
       uv_resolver::resolver::choose_version package=root
       uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
            0.183596s   0ms DEBUG uv_resolver::resolver Adding direct dependency: setuptools*
       uv_resolver::resolver::choose_version package=setuptools
         uv_resolver::resolver::package_wait package_name=setuptools
     uv_resolver::resolver::process_request request=Versions setuptools
       uv_client::registry_client::simple_api package=setuptools
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Volumes/casesensitive/MyTestPackage/.uv_cache/simple-v1/pypi/setuptools.rkyv
     uv_resolver::resolver::process_request request=Prefetch setuptools *
              0.184517s   0ms  WARN uv_client::cached_client Broken cache entry at /Volumes/casesensitive/MyTestPackage/.uv_cache/simple-v1/pypi/setuptools.rkyv, removing: failed to open file `/Volumes/casesensitive/MyTestPackage/.uv_cache/simple-v1/pypi/setuptools.rkyv`
              0.184975s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/setuptools/
           uv_client::cached_client::fresh_request url="https://pypi.org/simple/setuptools/"
           uv_client::cached_client::new_cache file=/Volumes/casesensitive/MyTestPackage/.uv_cache/simple-v1/pypi/setuptools.rkyv
           uv_client::registry_client::parse_simple_api package=setuptools
 uv_resolver::version_map::from_metadata
       uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==69.1.0
         uv_client::registry_client::wheel_metadata built_dist=setuptools==69.1.0
           uv_client::cached_client::get_serde
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.msgpack
            0.326924s 143ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (*)
            0.326935s 143ms DEBUG uv_resolver::resolver Selecting: setuptools==69.1.0 (setuptools-69.1.0-py3-none-any.whl)
       uv_resolver::resolver::get_dependencies package=setuptools, version=69.1.0
         uv_resolver::resolver::distributions_wait package_id=setuptools-69.1.0
                  0.327113s   0ms  WARN uv_client::cached_client Broken cache entry at /Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.msgpack, removing: failed to open file `/Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.msgpack`
                  0.327154s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/bb/0a/203797141ec9727344c7649f6d5f6cf71b89a6c28f8f55d4f18de7a1d352/setuptools-69.1.0-py3-none-any.whl
               uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/bb/0a/203797141ec9727344c7649f6d5f6cf71b89a6c28f8f55d4f18de7a1d352/setuptools-69.1.0-py3-none-any.whl"
               uv_client::cached_client::new_cache file=/Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.msgpack
               uv_client::registry_client::read_metadata_range_request wheel=setuptools-69.1.0-py3-none-any.whl
     uv_dispatch::install resolution="setuptools==69.1.0", venv="/private/var/folders/43/29_b2rv50rj6c9t29s0t5w5whc959c/T/.tmpNfA2Od/.venv"
          0.634726s   0ms DEBUG uv_dispatch Installing in setuptools==69.1.0 in /private/var/folders/43/29_b2rv50rj6c9t29s0t5w5whc959c/T/.tmpNfA2Od/.venv
          0.635592s   0ms DEBUG uv_installer::plan Identified uncached requirement: setuptools ==69.1.0
          0.635645s   0ms DEBUG uv_dispatch Downloading and building requirement for build: setuptools==69.1.0
       uv_installer::downloader::download total=1
         uv_installer::downloader::get_wheel name=setuptools==69.1.0, size=Some(819310), url="https://files.pythonhosted.org/packages/bb/0a/203797141ec9727344c7649f6d5f6cf71b89a6c28f8f55d4f18de7a1d352/setuptools-69.1.0-py3-none-any.whl"
           uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("setuptools"), version: "69.1.0", python_tag: ["py3"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: None, filename: "setuptools-69.1.0-py3-none-any.whl", hashes: Hashes { md5: None, sha256: Some("c054629b81b946d63a9c6e732bc8b2513a7c3ea645f11d0139a2191d735c60c6") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8" }])), size: Some(819310), upload_time_utc_ms: Some(1707700511837), url: AbsoluteUrl("https://files.pythonhosted.org/packages/bb/0a/203797141ec9727344c7649f6d5f6cf71b89a6c28f8f55d4f18de7a1d352/setuptools-69.1.0-py3-none-any.whl"), yanked: Some(Bool(false)) }, index: Pypi }))
             uv_client::cached_client::get_serde
               uv_client::cached_client::get_cacheable
                 uv_client::cached_client::read_and_parse_cache file=/Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.http
                    0.635820s   0ms  WARN uv_client::cached_client Broken cache entry at /Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.http, removing: failed to open file `/Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.http`
                    0.635848s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/bb/0a/203797141ec9727344c7649f6d5f6cf71b89a6c28f8f55d4f18de7a1d352/setuptools-69.1.0-py3-none-any.whl
                 uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/bb/0a/203797141ec9727344c7649f6d5f6cf71b89a6c28f8f55d4f18de7a1d352/setuptools-69.1.0-py3-none-any.whl"
                 uv_client::cached_client::new_cache file=/Volumes/casesensitive/MyTestPackage/.uv_cache/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.http
                 uv_distribution::distribution_database::download wheel=setuptools==69.1.0
          0.770841s 136ms DEBUG uv_dispatch Installing build requirement: setuptools==69.1.0
       uv_installer::installer::install num_wheels=1
error: Failed to build editables
  Caused by: Failed to build editable: file:///Volumes/casesensitive/MyTestPackage
  Caused by: Failed to install requirements from build-system.requires (install)
  Caused by: Failed to install build dependencies
  Caused by: Failed to install: setuptools-69.1.0-py3-none-any.whl (setuptools==69.1.0)
  Caused by: Failed to reflink /Volumes/casesensitive/MyTestPackage/.uv_cache/archive-v0/Ha3UGLq7qAPgqh6uXovPQ/setuptools-69.1.0.dist-info to /private/var/folders/43/29_b2rv50rj6c9t29s0t5w5whc959c/T/.tmpNfA2Od/.venv/lib/python3.10/site-packages/setuptools-69.1.0.dist-info
  Caused by: Cross-device link (os error 18)
```

---

_Comment by @charliermarsh on 2024-02-18 00:18_

And in (2), it fails with the cross-device link error seen above?

---

_Comment by @humanzz on 2024-02-18 00:29_

Yes. I edited the message to add more verbose output

---

_Comment by @zanieb on 2024-02-18 07:29_

Hm so in `pip install` we create a temporary directory in the current virtual environment

https://github.com/astral-sh/uv/blob/b5617198f35c89a33f073c677ed9164bb661509c/crates/uv/src/commands/pip_install.rs#L188

https://github.com/astral-sh/uv/blob/b5617198f35c89a33f073c677ed9164bb661509c/crates/uv/src/commands/pip_install.rs#L360

but eventually we get to

https://github.com/astral-sh/uv/blob/12fea1d058083a4f8a032ca8295a7d95a776292e/crates/uv-build/src/lib.rs#L286

via 

https://github.com/astral-sh/uv/blob/2586f655bbf650a9797c8f88b6d9066eefe7a3dc/crates/uv-dispatch/src/lib.rs#L274

https://github.com/astral-sh/uv/blob/340cb67a8b2a708996bd87369d2cadf48ef9df63/crates/uv-distribution/src/source/mod.rs#L954

where we create a temporary directory to actually perform the build but it is not scoped to the current virtual environment or our cache (hence your issue)

---

_Referenced in [astral-sh/uv#1628](../../astral-sh/uv/pulls/1628.md) on 2024-02-18 07:38_

---

_Comment by @humanzz on 2024-02-19 10:56_

I've just tried the `0.1.5` release and it finally works :)

I haven't said it earlier in my comments here, and on other issues, but I have to say it, **Thank you for your awesome work... this is great ⚡ 🚀** 


---

_Comment by @ifplusor on 2024-02-19 11:20_

> I've just tried the `0.1.5` release and it finally works :)
> 
> I haven't said it earlier in my comments here, and on other issues, but I have to say it now, Thank you for your awesome work... this is great ⚡ 🚀

I run `uv` in an additional APFS (Case-sensitive) volume, but it does not work.

---

_Comment by @zanieb on 2024-02-19 17:36_

Thank you!

@ifplusor we still need to add a fallback copy as described in this issue, but until then you should be able to set `UV_CACHE_DIR` to avoid copying across volumes.

---

_Assigned to @snowsignal by @zanieb on 2024-02-19 18:13_

---

_Referenced in [astral-sh/uv#1773](../../astral-sh/uv/pulls/1773.md) on 2024-02-20 18:08_

---

_Closed by @charliermarsh on 2024-02-22 02:57_

---

_Comment by @charliermarsh on 2024-02-22 05:01_

Should be fixed in v0.1.7 (out now).

---

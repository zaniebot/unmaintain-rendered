---
number: 10671
title: cache clean package not working as expected
type: issue
state: closed
author: davidszotten
labels:
  - question
  - cache
assignees: []
created_at: 2025-01-16T09:29:31Z
updated_at: 2025-01-16T17:44:09Z
url: https://github.com/astral-sh/uv/issues/10671
synced_at: 2026-01-10T01:24:56Z
---

# cache clean package not working as expected

---

_Issue opened by @davidszotten on 2025-01-16 09:29_

using `--no-cache` seems to skip some stuff that isn't removed with `cache clean <package>`?

```
$ uv --version
uv 0.5.20 (1c17662b3 2025-01-15)
```

```
$ uv cache clean uwsgi
Removed 704 files (3.6MiB)
```

```
$ uvx -vvv  --python=3.13.1 uwsgi
    0.000262s DEBUG uv uv 0.5.20 (1c17662b3 2025-01-15)
    0.004181s DEBUG uv_python::discovery Searching for Python 3.13.1 in managed installations or search path
    0.005040s DEBUG uv_python::discovery Searching for managed installations at `.local/share/uv/python`
    0.005852s DEBUG uv_python::discovery Found managed installation `cpython-3.13.1-macos-aarch64-none`
    0.006923s DEBUG uv_python::discovery Found `cpython-3.13.1-macos-aarch64-none` at `/Users/david/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13` (managed installations)
    0.010402s DEBUG uv_fs Acquired lock for `.local/share/uv/tools`
    0.010721s DEBUG uv_fs Released lock at `/Users/david/.local/share/uv/tools/.lock`
    0.011324s DEBUG uv::commands::project::environment Caching via interpreter: `/Users/david/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13`
 uv_client::linehaul::linehaul
    0.014239s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_resolver::resolver::solve
    0.020398s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.13.1
    0.020548s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.13.1
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.023693s   3ms DEBUG uv_resolver::resolver Adding direct dependency: uwsgi*
 uv_resolver::resolver::process_request request=Versions uwsgi
   uv_client::registry_client::simple_api package=uwsgi
     uv_client::cached_client::get_cacheable_with_retry
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=/Users/david/Library/Caches/uv/simple-v15/pypi/uwsgi.rkyv
 uv_client::cached_client::from_path_sync path="/Users/david/Library/Caches/uv/simple-v15/pypi/uwsgi.rkyv"
          0.026557s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/uwsgi/
         uv_client::cached_client::fresh_request url="https://pypi.org/simple/uwsgi/"
 uv_resolver::resolver::process_request request=Prefetch uwsgi *
         uv_client::cached_client::new_cache file=/Users/david/Library/Caches/uv/simple-v15/pypi/uwsgi.rkyv
         uv_client::registry_client::parse_simple_api package=uwsgi
   uv_resolver::version_map::from_metadata
    0.063074s  42ms DEBUG uv_resolver::resolver Searching for a compatible version of uwsgi (*)
    0.063427s  43ms DEBUG uv_resolver::resolver Selecting: uwsgi==2.0.28 [compatible] (uwsgi-2.0.28.tar.gz)
   uv_resolver::resolver::get_dependencies_forking package=uwsgi, version=2.0.28
     uv_resolver::resolver::get_dependencies package=uwsgi, version=2.0.28
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=uwsgi==2.0.28
    0.064928s DEBUG uv_fs Acquired lock for `/Users/david/Library/Caches/uv/sdists-v6/pypi/uwsgi/2.0.28`
     uv_client::cached_client::get_serde_with_retry
       uv_client::cached_client::get_cacheable_with_retry
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/david/Library/Caches/uv/sdists-v6/pypi/uwsgi/2.0.28/revision.http
 uv_client::cached_client::from_path_sync path="/Users/david/Library/Caches/uv/sdists-v6/pypi/uwsgi/2.0.28/revision.http"
            0.065403s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/24/c2/d58480aadc9a1f420dd96fc43cf0dcd8cb5ededb95cab53743529c23b6cd/uwsgi-2.0.28.tar.gz
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/24/c2/d58480aadc9a1f420dd96fc43cf0dcd8cb5ededb95cab53743529c23b6cd/uwsgi-2.0.28.tar.gz"
           uv_client::cached_client::new_cache file=/Users/david/Library/Caches/uv/sdists-v6/pypi/uwsgi/2.0.28/revision.http
           uv_distribution::source::download source_dist=uwsgi==2.0.28
              0.197876s   0ms DEBUG uv_distribution::source Downloading source distribution: uwsgi==2.0.28
             uv_distribution::source::download_source_dist source_dist=uwsgi==2.0.28
      0.335440s 271ms DEBUG uv_distribution::source No static `pyproject.toml` available for: uwsgi==2.0.28 (MissingPyprojectToml)
      0.335929s 272ms DEBUG uv_distribution::source No static `PKG-INFO` available for: uwsgi==2.0.28 (PkgInfo(UnsupportedMetadataVersion("1.0")))
      0.336076s 272ms DEBUG uv_distribution::source No static `egg-info` available for: uwsgi==2.0.28 (MissingEggInfo)
     uv_distribution::source::build_metadata dist=uwsgi==2.0.28
        0.336098s   0ms DEBUG uv_distribution::source Preparing metadata for: uwsgi==2.0.28
       uv_dispatch::setup_build version_id="uwsgi==2.0.28", subdirectory=None
          0.336796s   0ms DEBUG uv_virtualenv::virtualenv Assessing Python executable as base candidate: /Users/david/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13
          0.336811s   0ms DEBUG uv_virtualenv::virtualenv Using base executable for virtual environment: /Users/david/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13
          0.336828s   0ms DEBUG uv_virtualenv::virtualenv Ignoring empty directory
          0.338647s   2ms DEBUG uv_build_frontend Resolving build requirements
 uv_resolver::resolver::solve
    0.338861s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.13.1
    0.338868s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.13.1
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.339115s   0ms DEBUG uv_resolver::resolver Adding direct dependency: setuptools>=40.8.0
         uv_resolver::resolver::process_request request=Versions setuptools
           uv_client::registry_client::simple_api package=setuptools
             uv_client::cached_client::get_cacheable_with_retry
               uv_client::cached_client::get_cacheable
                 uv_client::cached_client::read_and_parse_cache file=/Users/david/Library/Caches/uv/simple-v15/pypi/setuptools.rkyv
         uv_resolver::resolver::process_request request=Prefetch setuptools >=40.8.0
 uv_client::cached_client::from_path_sync path="/Users/david/Library/Caches/uv/simple-v15/pypi/setuptools.rkyv"
                  0.340305s   1ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/setuptools/
           uv_resolver::version_map::from_metadata
    0.340639s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (>=40.8.0)
    0.340880s   2ms DEBUG uv_resolver::resolver Selecting: setuptools==75.8.0 [compatible] (setuptools-75.8.0-py3-none-any.whl)
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==75.8.0
   uv_resolver::resolver::get_dependencies_forking package=setuptools, version=75.8.0
     uv_resolver::resolver::get_dependencies package=setuptools, version=75.8.0
             uv_client::registry_client::wheel_metadata built_dist=setuptools==75.8.0
               uv_client::cached_client::get_serde_with_retry
                 uv_client::cached_client::get_cacheable_with_retry
                   uv_client::cached_client::get_cacheable
                     uv_client::cached_client::read_and_parse_cache file=/Users/david/Library/Caches/uv/wheels-v3/pypi/setuptools/setuptools-75.8.0-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/david/Library/Caches/uv/wheels-v3/pypi/setuptools/setuptools-75.8.0-py3-none-any.msgpack"
                      0.341839s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/69/8a/b9dc7678803429e4a3bc9ba462fa3dd9066824d3c607490235c6a796be5a/setuptools-75.8.0-py3-none-any.whl.metadata
    0.343722s   4ms DEBUG uv_resolver::resolver::batch_prefetch Tried 1 versions: setuptools 1
    0.343728s   4ms DEBUG uv_resolver::resolver marker environment resolution took 0.005s
         uv_dispatch::install build_stack=BuildStack({Digest(HashDigest { algorithm: Sha256, digest: "79ca1891ef2df14508ab0471ee8c0eb94bd2d51d03f32f90c4bbe557ab1e99d0" })}), resolution="setuptools==75.8.0", venv="/Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo"
            0.345366s   0ms DEBUG uv_dispatch Installing in setuptools==75.8.0 in /Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo
            0.347070s   1ms DEBUG uv_installer::plan Requirement already cached: setuptools==75.8.0
            0.347206s   1ms DEBUG uv_dispatch Installing build requirement: setuptools==75.8.0
           uv_installer::installer::install num_wheels=1
 uv_installer::installer::install num_wheels=1
   uv_install_wheel::linker::install_wheel wheel=setuptools-75.8.0-py3-none-any.whl
     uv_install_wheel::linker::link_wheel_files
          0.356686s  20ms DEBUG uv_build_frontend Creating PEP 517 build environment
          0.356689s  20ms DEBUG uv_build_frontend Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
         uv_build_frontend::run_python_script script="get_requires_for_build_wheel", version_id="uwsgi==2.0.28"
            0.585415s 228ms DEBUG uv_build_frontend /Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/lib/python3.13/site-packages/setuptools/_distutils/dist.py:270: UserWarning: Unknown distribution option: 'descriptions'
            0.585439s 228ms DEBUG uv_build_frontend   warnings.warn(msg)
            0.596816s 240ms DEBUG uv_build_frontend running egg_info
            0.597770s 240ms DEBUG uv_build_frontend creating uWSGI.egg-info
            0.597863s 241ms DEBUG uv_build_frontend writing uWSGI.egg-info/PKG-INFO
            0.598112s 241ms DEBUG uv_build_frontend writing dependency_links to uWSGI.egg-info/dependency_links.txt
            0.598237s 241ms DEBUG uv_build_frontend writing top-level names to uWSGI.egg-info/top_level.txt
            0.598330s 241ms DEBUG uv_build_frontend writing manifest file 'uWSGI.egg-info/SOURCES.txt'
            0.603013s 246ms DEBUG uv_build_frontend reading manifest file 'uWSGI.egg-info/SOURCES.txt'
            0.603096s 246ms DEBUG uv_build_frontend adding license file 'LICENSE'
            0.603393s 246ms DEBUG uv_build_frontend writing manifest file 'uWSGI.egg-info/SOURCES.txt'
        0.615098s 279ms DEBUG uv_build_frontend Calling `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel()`
       uv_build_frontend::run_python_script script="prepare_metadata_for_build_wheel", version_id="uwsgi==2.0.28"
          0.672395s  57ms DEBUG uv_build_frontend /Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/lib/python3.13/site-packages/setuptools/_distutils/dist.py:270: UserWarning: Unknown distribution option: 'descriptions'
          0.672404s  57ms DEBUG uv_build_frontend   warnings.warn(msg)
          0.677223s  62ms DEBUG uv_build_frontend running dist_info
          0.678643s  63ms DEBUG uv_build_frontend creating /Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI.egg-info
          0.678717s  63ms DEBUG uv_build_frontend writing /Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI.egg-info/PKG-INFO
          0.678962s  63ms DEBUG uv_build_frontend writing dependency_links to /Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI.egg-info/dependency_links.txt
          0.679212s  64ms DEBUG uv_build_frontend writing top-level names to /Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI.egg-info/top_level.txt
          0.679302s  64ms DEBUG uv_build_frontend writing manifest file '/Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI.egg-info/SOURCES.txt'
          0.681442s  66ms DEBUG uv_build_frontend reading manifest file '/Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI.egg-info/SOURCES.txt'
          0.681583s  66ms DEBUG uv_build_frontend adding license file 'LICENSE'
          0.681874s  66ms DEBUG uv_build_frontend writing manifest file '/Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI.egg-info/SOURCES.txt'
          0.681933s  66ms DEBUG uv_build_frontend creating '/Users/david/Library/Caches/uv/builds-v0/.tmpSxurSo/metadata_directory/uWSGI-2.0.28.dist-info'
        0.694635s 358ms DEBUG uv_distribution::source Prepared metadata for: uwsgi==2.0.28
      0.720970s 657ms DEBUG uv_fs Released lock at `/Users/david/Library/Caches/uv/sdists-v6/pypi/uwsgi/2.0.28/.lock`
    0.721023s 700ms DEBUG uv_resolver::resolver::batch_prefetch Tried 1 versions: uwsgi 1
    0.721038s 700ms DEBUG uv_resolver::resolver marker environment resolution took 0.700s
Resolved 1 package in 704ms
    0.722329s DEBUG uv::commands::tool::run Running `uwsgi`
    0.722413s DEBUG uv_tool Looking at `.dist-info` at: Library/Caches/uv/archive-v0/FHcSb4W6PFFrBfhTc5c29/lib/python3.13/site-packages/uWSGI-2.0.28.dist-info
dyld[30960]: Library not loaded: /install/lib/libpython3.13.dylib
  Referenced from: <9E38007F-64A0-3C50-B7CA-7AA5091E8932> /Users/david/Library/Caches/uv/archive-v0/FHcSb4W6PFFrBfhTc5c29/bin/uwsgi
  Reason: tried: '/install/lib/libpython3.13.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/install/lib/libpython3.13.dylib' (no such file), '/install/lib/libpython3.13.dylib' (no such file)
    0.724722s DEBUG uv::commands::tool::run Command exited with signal: Some(6)
```


but

```
uvx --no-cache --python=3.13.1 uwsgi
   Built uwsgi==2.0.28
Installed 1 package in 3ms
*** Starting uWSGI 2.0.28 (64bit) on [Thu Jan 16 09:26:45 2025] ***
compiled with version: Apple LLVM 15.0.0 (clang-1500.1.0.2.5) on 16 January 2025 09:26:42
os: Darwin-23.6.0 Darwin Kernel Version 23.6.0: Thu Sep 12 23:36:06 PDT 2024; root:xnu-10063.141.1.701.1~1/RELEASE_ARM64_T8122
nodename: david-Mac15,3
machine: arm64
clock source: unix
pcre jit disabled
detected number of CPU cores: 8
current working directory: /Users/david
detected binary path: /private/var/folders/3q/26_y3lv165q8vhq94rjp80qw0000gp/T/.tmp7FSN6j/archive-v0/c1EOPa7tN1pO9w7aunm1v/bin/uwsgi
*** WARNING: you are running uWSGI without its master process manager ***
your processes number limit is 2666
your memory page size is 16384 bytes
detected max file descriptor number: 1048575
lock engine: OSX spinlocks
thunder lock: disabled (you can enable it with --thunder-lock)
The -s/--socket option is missing and stdin is not a socket.
```

(version with `-vvv` was too large for a gh issue body apparently so available here https://gist.github.com/davidszotten/91e39420ec2e6db9c3be049da01c8fb2)

---

_Comment by @charliermarsh on 2025-01-16 15:12_

This is annoying but seems expected. In the first example, you're getting a cached _environment_ which isn't (can't be?) cleared with `uv cache clean`.

---

_Label `question` added by @charliermarsh on 2025-01-16 15:12_

---

_Label `cache` added by @charliermarsh on 2025-01-16 15:12_

---

_Comment by @davidszotten on 2025-01-16 15:41_

ah, I thought it was something along those lines. what are my options for removing the bad cached environment? (I'd prefer to avoid clearing my entire cache if possible)

---

_Comment by @charliermarsh on 2025-01-16 15:44_

Actually I'm like mildly confused -- my previous comment might be wrong. Let me look again.

---

_Comment by @charliermarsh on 2025-01-16 15:49_

Sorry, yes, it's a cached environment. You could run `uv cache prune`? That removes any cached environments. Or just remove the `environments-v1` subdirectory.

---

_Comment by @davidszotten on 2025-01-16 17:44_

Thanks!

(it seems reasonable to me that the cache wasn't invalidated; it should be pretty rare that we fix details around python installations like we did recently)

---

_Closed by @davidszotten on 2025-01-16 17:44_

---

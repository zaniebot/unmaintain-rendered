```yaml
number: 1588
title: uv hangs when using tox-uv
type: issue
state: closed
author: davidszotten
labels:
  - bug
assignees: []
created_at: 2024-02-17T13:15:59Z
updated_at: 2024-02-17T21:33:37Z
url: https://github.com/astral-sh/uv/issues/1588
synced_at: 2026-01-10T05:40:31Z
```

# uv hangs when using tox-uv

---

_Issue opened by @davidszotten on 2024-02-17 13:15_

See more details at https://github.com/tox-dev/tox-uv/issues/11

any advice on how to debug this?

hacking tox-uv to add a `-v` to the `uv invocation i get

```
py: 584 W install_package> /Users/david/environments/uv-bugs/bin/uv pip install -v --reinstall --no-deps my_package@/Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz [tox/tox_env/api.py:427]
 uv::requirements::from_source source=my_package@/Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
    0.000707s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /Users/david/dev/uv-bugs/.tox/py/.venv
    0.001068s DEBUG uv_interpreter::interpreter Using cached markers for: /Users/david/dev/uv-bugs/.tox/py/.venv/bin/python
    0.001074s DEBUG uv::commands::pip_install Using Python 3.12.0 environment at /Users/david/dev/uv-bugs/.tox/py/.venv/bin/python
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.203508s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.0
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.203564s   0ms DEBUG uv_resolver::resolver Adding direct dependency: my-package*
   uv_resolver::resolver::choose_version package=my-package
        0.203677s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz (*)
 uv_resolver::resolver::process_request request=Metadata my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
     uv_distribution::source::build_source_dist_metadata
          0.205756s   0ms DEBUG uv_distribution::source Preparing metadata for: my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
       uv_dispatch::setup_build package_id="my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz", subdirectory=None
            0.205869s   0ms DEBUG uv_build Unpacking for build: /Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
         uv_resolver::resolver::solve
              0.208806s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.0
           uv_resolver::resolver::choose_version package=root
           uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
                0.208837s   0ms DEBUG uv_resolver::resolver Adding direct dependency: poetry-core*
           uv_resolver::resolver::choose_version package=poetry-core
             uv_resolver::resolver::package_wait package_name=poetry-core
         uv_resolver::resolver::process_request request=Versions poetry-core
           uv_client::registry_client::simple_api package=poetry-core
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/david/Library/Caches/uv/simple-v0/pypi/poetry-core.rkyv
         uv_resolver::resolver::process_request request=Prefetch poetry-core *
                  0.209834s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/poetry-core/
 uv_resolver::version_map::from_metadata
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=poetry-core==1.9.0
             uv_client::registry_client::wheel_metadata built_dist=poetry-core==1.9.0
               uv_client::cached_client::get_serde
                 uv_client::cached_client::get_cacheable
                   uv_client::cached_client::read_and_parse_cache file=/Users/david/Library/Caches/uv/wheels-v0/pypi/poetry-core/poetry_core-1.9.0-py3-none-any.msgpack
                0.210159s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of poetry-core (*)
                0.210165s   1ms DEBUG uv_resolver::resolver Selecting: poetry-core==1.9.0 (poetry_core-1.9.0-py3-none-any.whl)
           uv_resolver::resolver::get_dependencies package=poetry-core, version=1.9.0
         uv_dispatch::install resolution="poetry-core==1.9.0", venv="/private/var/folders/jb/193kg8hd62n5mx9mz29k7fl00000gn/T/.tmp8WfoYo/.venv"
              0.210333s   0ms DEBUG uv_dispatch Installing in poetry-core==1.9.0 in /private/var/folders/jb/193kg8hd62n5mx9mz29k7fl00000gn/T/.tmp8WfoYo/.venv
              0.210698s   0ms DEBUG uv_installer::plan Requirement already cached: poetry-core==1.9.0
              0.210709s   0ms DEBUG uv_dispatch Installing build requirement: poetry-core==1.9.0
           uv_installer::installer::install num_wheels=1
            0.214843s   9ms DEBUG uv_build Calling `poetry.core.masonry.api.get_requires_for_build_wheel()`
         uv_build::run_python_script script="get_requires_for_build_wheel", python_version=3.12.0
          0.372848s 167ms DEBUG uv_build Calling `poetry.core.masonry.api.prepare_metadata_for_build_wheel()`
       uv_build::run_python_script script="prepare_metadata_for_build_wheel", python_version=3.12.0
          0.738287s 532ms DEBUG uv_distribution::source Prepared metadata for: my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
   uv_resolver::resolver::get_dependencies package=my-package, version=0.1.0
Resolved 1 package in 557ms
    0.761061s DEBUG uv_installer::plan Identified uncached requirement: my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
    0.761283s DEBUG uv_installer::plan Unnecessary package: httpcore==1.0.3
    0.761289s DEBUG uv_installer::plan Unnecessary package: h11==0.14.0
    0.761292s DEBUG uv_installer::plan Unnecessary package: httpx==0.26.0
    0.761293s DEBUG uv_installer::plan Unnecessary package: certifi==2024.2.2
    0.761295s DEBUG uv_installer::plan Unnecessary package: sniffio==1.3.0
    0.761315s DEBUG uv_installer::plan Unnecessary package: anyio==4.2.0
    0.761316s DEBUG uv_installer::plan Unnecessary package: idna==3.6
 uv_installer::downloader::download total=1
   uv_installer::downloader::get_wheel name=my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz, size=None, url=""
     uv_distribution::distribution_database::get_or_build_wheel dist=Source(Path(PathSourceDist { name: PackageName("my-package"), url: VerbatimUrl { url: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz", query: None, fragment: None }, given: Some("/Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz") }, path: "/Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz", editable: false }))
       uv_distribution::source::build_source_dist
            0.762486s   0ms DEBUG uv_distribution::source Building: my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
         uv_dispatch::setup_build package_id="my-package @ file:///Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz", subdirectory=None
              0.763308s   0ms DEBUG uv_build Unpacking for build: /Users/david/dev/uv-bugs/.tox/.tmp/package/5/my_package-0.1.0.tar.gz
           uv_resolver::resolver::solve
                0.767418s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.0
             uv_resolver::resolver::choose_version package=root
             uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
                  0.767464s   0ms DEBUG uv_resolver::resolver Adding direct dependency: poetry-core*
             uv_resolver::resolver::choose_version package=poetry-core
               uv_resolver::resolver::package_wait package_name=poetry-core
                  0.767493s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of poetry-core (*)
                  0.767505s   0ms DEBUG uv_resolver::resolver Selecting: poetry-core==1.9.0 (poetry_core-1.9.0-py3-none-any.whl)
             uv_resolver::resolver::get_dependencies package=poetry-core, version=1.9.0
               uv_resolver::resolver::distributions_wait package_id=poetry-core-1.9.0
           uv_resolver::resolver::process_request request=Prefetch poetry-core *

[hangs here]
```

---

_Comment by @charliermarsh on 2024-02-17 18:23_

This could be the same as https://github.com/astral-sh/uv/issues/1355.

---

_Label `bug` added by @zanieb on 2024-02-17 19:36_

---

_Comment by @davidszotten on 2024-02-17 21:33_

yes, #1607 seems to fix this. thanks!

---

_Closed by @davidszotten on 2024-02-17 21:33_

---

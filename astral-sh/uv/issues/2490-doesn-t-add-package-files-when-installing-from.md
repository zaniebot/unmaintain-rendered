---
number: 2490
title: "Doesn't add package files when installing from sdist on PyPy"
type: issue
state: closed
author: edgarrmondragon
labels:
  - bug
  - great writeup
  - installer
assignees: []
created_at: 2024-03-16T17:46:28Z
updated_at: 2024-03-22T19:36:08Z
url: https://github.com/astral-sh/uv/issues/2490
synced_at: 2026-01-10T01:23:18Z
---

# Doesn't add package files when installing from sdist on PyPy

---

_Issue opened by @edgarrmondragon on 2024-03-16 17:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## System

* `uv` 0.1.21 (e9c12c52f 2024-03-14)

## Description

uv doesn't add all package files when installing from an sdist, while virtualenv+pip do it correctly:

### uv

```console
$ uv venv venv --python pypy3.10
Using Python 3.10.13 interpreter at: /Users/edgarramirez/.pyenv/versions/pypy3.10-7.3.15/bin/pypy3.10
Creating virtualenv at: venv
Activate with: source venv/bin/activate

$ source venv/bin/activate

$ (venv) uv -vv pip install deptry==0.14.0 --no-binary 'deptry'
```

<details><summary>Output</summary>
<p>

```
uv::requirements::from_source source=deptry==0.14.0
    0.000287s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/edgarramirez/Code/contrib/deptry/venv
    0.000675s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.10.13, skipping probing: /Users/edgarramirez/Code/contrib/deptry/venv/bin/python
    0.000687s DEBUG uv::commands::pip_install Using Python 3.10.13 environment at /Users/edgarramirez/Code/contrib/deptry/venv/bin/python
    0.001129s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.005233s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.10.13
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.005291s   0ms DEBUG uv_resolver::resolver Adding direct dependency: deptry==0.14.0
   uv_resolver::resolver::choose_version package=deptry
     uv_resolver::resolver::package_wait package_name=deptry
 uv_resolver::resolver::process_request request=Versions deptry
   uv_client::registry_client::simple_api package=deptry
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/deptry.rkyv
 uv_resolver::resolver::process_request request=Prefetch deptry ==0.14.0
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/deptry.rkyv"
          0.006364s   0ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/deptry/
          0.006375s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/deptry/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/deptry/"
            0.006391s   0ms DEBUG uv_auth::middleware No credentials found for: https://pypi.org/simple/deptry/
          0.514199s 508ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/deptry/
       uv_client::cached_client::refresh_cache file=/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/deptry.rkyv
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=deptry==0.14.0
     uv_client::registry_client::wheel_metadata built_dist=deptry==0.14.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/deptry/deptry-0.14.0-cp38-abi3-macosx_10_12_x86_64.msgpack
        0.518327s 513ms DEBUG uv_resolver::resolver Searching for a compatible version of deptry (==0.14.0)
        0.518344s 513ms DEBUG uv_resolver::resolver Selecting: deptry==0.14.0 (deptry-0.14.0-cp38-abi3-macosx_10_12_x86_64.whl)
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/deptry/deptry-0.14.0-cp38-abi3-macosx_10_12_x86_64.msgpack"
   uv_resolver::resolver::get_dependencies package=deptry, version=0.14.0
     uv_resolver::resolver::distributions_wait package_id=deptry-0.14.0
              0.518703s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/58/d5/f2ba32ecf0e6d4d152f188211cd7aa7098e41d7baa69032b03081dd97b45/deptry-0.14.0-cp38-abi3-macosx_10_12_x86_64.whl.metadata
        0.518788s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: chardet>=4.0.0
        0.518801s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: click>=8.0.0, <9
        0.518809s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: pathspec>=0.9.0
        0.518818s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: tomli>=2.0.1
   uv_resolver::resolver::choose_version package=chardet
     uv_resolver::resolver::package_wait package_name=chardet
 uv_resolver::resolver::process_request request=Versions chardet
   uv_client::registry_client::simple_api package=chardet
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/chardet.rkyv
 uv_resolver::resolver::process_request request=Versions click
   uv_client::registry_client::simple_api package=click
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/chardet.rkyv"
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/click.rkyv
 uv_resolver::resolver::process_request request=Versions pathspec
   uv_client::registry_client::simple_api package=pathspec
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/pathspec.rkyv
 uv_resolver::resolver::process_request request=Versions tomli
   uv_client::registry_client::simple_api package=tomli
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/tomli.rkyv
 uv_resolver::resolver::process_request request=Prefetch tomli >=2.0.1
 uv_resolver::resolver::process_request request=Prefetch pathspec >=0.9.0
 uv_resolver::resolver::process_request request=Prefetch click >=8.0.0, <9
 uv_resolver::resolver::process_request request=Prefetch chardet >=4.0.0
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/click.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/tomli.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/simple-v3/pypi/pathspec.rkyv"
          0.519406s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/chardet/
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=chardet==5.2.0
     uv_client::registry_client::wheel_metadata built_dist=chardet==5.2.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/chardet/chardet-5.2.0-py3-none-any.msgpack
        0.519577s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of chardet (>=4.0.0)
        0.519588s   0ms DEBUG uv_resolver::resolver Selecting: chardet==5.2.0 (chardet-5.2.0-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/chardet/chardet-5.2.0-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=chardet, version=5.2.0
     uv_resolver::resolver::distributions_wait package_id=chardet-5.2.0
              0.519875s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/38/6f/f5fbc992a329ee4e0f288c1fe0e2ad9485ed064cac731ed2fe47dcc38cbf/chardet-5.2.0-py3-none-any.whl.metadata
   uv_resolver::resolver::choose_version package=click
     uv_resolver::resolver::package_wait package_name=click
          0.520004s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/tomli/
   uv_resolver::version_map::from_metadata 
          0.520072s   1ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/pathspec/
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=tomli==2.0.1
     uv_client::registry_client::wheel_metadata built_dist=tomli==2.0.1
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/tomli/tomli-2.0.1-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/tomli/tomli-2.0.1-py3-none-any.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pathspec==0.12.1
     uv_client::registry_client::wheel_metadata built_dist=pathspec==0.12.1
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/pathspec/pathspec-0.12.1-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/pathspec/pathspec-0.12.1-py3-none-any.msgpack"
          0.520379s   1ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/click/
   uv_resolver::version_map::from_metadata 
              0.520542s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/97/75/10a9ebee3fd790d20926a90a2547f0bf78f371b2f13aa822c759680ca7b9/tomli-2.0.1-py3-none-any.whl.metadata
              0.520584s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=click==8.1.7
     uv_client::registry_client::wheel_metadata built_dist=click==8.1.7
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/click/click-8.1.7-py3-none-any.msgpack
        0.520693s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of click (>=8.0.0, <9)
        0.520704s   0ms DEBUG uv_resolver::resolver Selecting: click==8.1.7 (click-8.1.7-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/edgarramirez/Library/Caches/uv/wheels-v0/pypi/click/click-8.1.7-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=click, version=8.1.7
     uv_resolver::resolver::distributions_wait package_id=click-8.1.7
              0.520950s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata
   uv_resolver::resolver::choose_version package=pathspec
     uv_resolver::resolver::package_wait package_name=pathspec
        0.521011s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of pathspec (>=0.9.0)
        0.521021s   0ms DEBUG uv_resolver::resolver Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=pathspec, version=0.12.1
     uv_resolver::resolver::distributions_wait package_id=pathspec-0.12.1
   uv_resolver::resolver::choose_version package=tomli
     uv_resolver::resolver::package_wait package_name=tomli
        0.521069s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of tomli (>=2.0.1)
        0.521078s   0ms DEBUG uv_resolver::resolver Selecting: tomli==2.0.1 (tomli-2.0.1-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=tomli, version=2.0.1
     uv_resolver::resolver::distributions_wait package_id=tomli-2.0.1
Resolved 5 packages in 516ms
    0.521871s DEBUG uv_installer::plan Requirement already cached: chardet==5.2.0
    0.522039s DEBUG uv_installer::plan Requirement already cached: click==8.1.7
    0.525213s DEBUG uv_installer::plan Requirement already cached: deptry==0.14.0
    0.525461s DEBUG uv_installer::plan Requirement already cached: pathspec==0.12.1
    0.525615s DEBUG uv_installer::plan Requirement already cached: tomli==2.0.1
 uv_installer::installer::install num_wheels=5
Installed 5 packages in 8ms
 + chardet==5.2.0
 + click==8.1.7
 + deptry==0.14.0
 + pathspec==0.12.1
 + tomli==2.0.1
```

</p>
</details> 

```
$ (venv) tree venv/lib/pypy3.10/site-packages/deptry
venv/lib/pypy3.10/site-packages/deptry
└── rust.pypy310-pp73-darwin.so
```

> [!NOTE]  
> This leads to import errors. For example
> ```python
> Traceback (most recent call last):
>   File "/home/runner/work/citric/citric/.nox/deps-pypy3-10/bin/deptry", line 5, in <module>
>     from deptry.cli import deptry
> ModuleNotFoundError: No module named 'deptry.cli'
> ```

### virtualenv + pip

```console
$ virtualenv venv --python $(pyenv which pypy3.10)
created virtual environment PyPy3.10.13.final.0-64 in 249ms
  creator PyPy3Posix(dest=/Users/edgarramirez/Code/contrib/deptry/venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/Users/edgarramirez/Library/Application Support/virtualenv)
    added seed packages: pip==24.0, setuptools==69.1.0, wheel==0.42.0
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator

$ source venv/bin/activate

$ (venv) pip install deptry==0.14.0 --no-binary 'deptry' --no-cache
```

<details><summary>Output</summary>
<p>

```
Collecting deptry==0.14.0
  Downloading deptry-0.14.0.tar.gz (116 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 116.0/116.0 kB 391.3 kB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting chardet>=4.0.0 (from deptry==0.14.0)
  Downloading chardet-5.2.0-py3-none-any.whl.metadata (3.4 kB)
Collecting click<9,>=8.0.0 (from deptry==0.14.0)
  Downloading click-8.1.7-py3-none-any.whl.metadata (3.0 kB)
Collecting pathspec>=0.9.0 (from deptry==0.14.0)
  Downloading pathspec-0.12.1-py3-none-any.whl.metadata (21 kB)
Collecting tomli>=2.0.1 (from deptry==0.14.0)
  Downloading tomli-2.0.1-py3-none-any.whl.metadata (8.9 kB)
Downloading chardet-5.2.0-py3-none-any.whl (199 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 199.4/199.4 kB 3.3 MB/s eta 0:00:00
Downloading click-8.1.7-py3-none-any.whl (97 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.9/97.9 kB 84.1 MB/s eta 0:00:00
Downloading pathspec-0.12.1-py3-none-any.whl (31 kB)
Downloading tomli-2.0.1-py3-none-any.whl (12 kB)
Building wheels for collected packages: deptry
  Building wheel for deptry (pyproject.toml) ... done
  Created wheel for deptry: filename=deptry-0.14.0-pp310-pypy310_pp73-macosx_11_0_arm64.whl size=2118674 sha256=e159eed0504b5cd9c730a9bbac41de342265c81206e49a1b3d353bd180473c98
  Stored in directory: /private/var/folders/k7/l2cj2kv14x9dg7ljk_p20y7w0000gn/T/pip-ephem-wheel-cache-j5e_kjsv/wheels/0d/d4/c2/a1a72fd0512f7e13c795e7cda6795b0d3428626c1e90afa1ce
Successfully built deptry
Installing collected packages: tomli, pathspec, click, chardet, deptry
Successfully installed chardet-5.2.0 click-8.1.7 deptry-0.14.0 pathspec-0.12.1 tomli-2.0.1
```

</p>
</details> 

```console
$ tree venv/lib/pypy3.10/site-packages/deptry -L 1
venv/lib/pypy3.10/site-packages/deptry
├── __init__.py
├── __main__.py
├── __pycache__
├── cli.py
├── config.py
├── core.py
├── dependency.py
├── dependency_getter
├── dependency_specification_detector.py
├── deprecate
├── exceptions.py
├── imports
├── module.py
├── python_file_finder.py
├── reporters
├── rust.pyi
├── rust.pypy310-pp73-darwin.so
├── stdlibs.py
├── utils.py
└── violations
```


---

_Comment by @zanieb on 2024-03-16 17:48_

Thanks for the report!

Just a heads up that we don't officially support PyPy yet, you can track that at https://github.com/astral-sh/uv/issues/2096.

---

_Label `bug` added by @zanieb on 2024-03-16 17:48_

---

_Label `great writeup` added by @zanieb on 2024-03-16 17:48_

---

_Label `installer` added by @zanieb on 2024-03-16 17:48_

---

_Referenced in [astral-sh/uv#2096](../../astral-sh/uv/issues/2096.md) on 2024-03-16 17:52_

---

_Label `pypy` added by @AlexWaygood on 2024-03-16 17:52_

---

_Comment by @charliermarsh on 2024-03-16 18:04_

Not obvious to me why this would be.

---

_Comment by @edgarrmondragon on 2024-03-16 18:07_

> Thanks for the report!
> 
> Just a heads up that we don't officially support PyPy yet

Yup, it's on me for using an unsupported implementation! 😅

> you can track that at #2096

Tracking already. Just wanted to log since I encountered it this morning.

Thanks!

---

_Comment by @charliermarsh on 2024-03-16 18:08_

👍 Totally, thank you for the report!

---

_Comment by @charliermarsh on 2024-03-16 18:22_

I was able to reproduce (but not with CPython, so that's good at least).

---

_Referenced in [astral-sh/uv#2491](../../astral-sh/uv/issues/2491.md) on 2024-03-16 18:24_

---

_Referenced in [fpgmaas/deptry#613](../../fpgmaas/deptry/issues/613.md) on 2024-03-16 18:37_

---

_Comment by @charliermarsh on 2024-03-16 18:51_

@konstin - I'm not sure what the issue is here. Building `python -m build` gives me a reasonable wheel, but our build is producing a wheel with just `rust.pypy310-pp73-darwin.so` in it.


---

_Comment by @charliermarsh on 2024-03-16 18:51_

Interestingly, we build deptry-0.14.0-pp39-pypy39_pp73-macosx_11_0_arm64.whl, but build is giving me deptry-0.0.1-pp39-pypy39_pp73-macosx_11_0_arm64 (`0.14.0` vs. `0.0.1`).

---

_Comment by @charliermarsh on 2024-03-16 18:53_

When I run with `pypa/build`, I also see more maturin logging:

```
Found sdist root: /Users/crmarsh/workspace/deptry
Rewriting Cargo.toml at /Users/crmarsh/workspace/deptry/Cargo.toml
```

---

_Assigned to @konstin by @charliermarsh on 2024-03-18 01:24_

---

_Referenced in [astral-sh/uv#2615](../../astral-sh/uv/pulls/2615.md) on 2024-03-22 15:42_

---

_Label `pypy` removed by @konstin on 2024-03-22 15:43_

---

_Comment by @konstin on 2024-03-22 15:45_

On my machine (ubuntu) this failing on any python interpreter, i'm surprised this seems to be pypy only on mac, do the mac users know any details about what happens on cpython? Otherwise the fix should be https://github.com/astral-sh/uv/pull/2615, our cache gitignore leaks into the build

---

_Comment by @edgarrmondragon on 2024-03-22 16:51_

> do the mac users know any details about what happens on cpython?

@konstin You're right, I see the same behavior on cpython! Repeating the steps in the issue description for cpython leads to the same empty installation:

```console
$ tree venv/lib/python3.10/site-packages/deptry
venv/lib/python3.10/site-packages/deptry
└── rust.abi3.so
```

I just didn't notice it because there were deptry wheels for cpython.

---

_Comment by @charliermarsh on 2024-03-22 17:01_

Ah yeah, my guess is it affects all builds, we just weren’t required to build wheels in all cases.

---

_Closed by @zanieb on 2024-03-22 19:36_

---

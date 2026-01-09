---
number: 1562
title: uv fails to resolve dependencies on tensorrt==8.6.1.post1
type: issue
state: closed
author: Di-Is
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-02-17T02:39:22Z
updated_at: 2024-02-17T14:18:41Z
url: https://github.com/astral-sh/uv/issues/1562
synced_at: 2026-01-07T13:12:16-06:00
---

# uv fails to resolve dependencies on tensorrt==8.6.1.post1

---

_Issue opened by @Di-Is on 2024-02-17 02:39_

uv fails to resolve dependencies on tensorrt==8.6.1.post1.

#### executed command
```bash
uv pip compile -v requirements.in
```

#### requirements.in
```txt
tensorrt==8.6.1.post1
```

#### environment 
uv version: 0.1.3
platform: Ubuntu22.04 (amd64)

#### Error log
```log
error: Failed to download and build: tensorrt==8.6.1.post1
  Caused by: Failed to build: tensorrt==8.6.1.post1
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/tmp/.tmptfY9gE/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/tmp/.tmptfY9gE/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/tmp/.tmptfY9gE/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 103, in <module>
  File "<string>", line 96, in parent_command_line
  File "/usr/local/lib/python3.11/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/subprocess.py", line 548, in run
    with Popen(*popenargs, **kwargs) as process:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/usr/local/lib/python3.11/subprocess.py", line 1950, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'ps'
---
root@d97baf896efc:/conf# uv pip compile -v requirements.in
 uv::requirements::from_source source=requirements.in
    0.001766s DEBUG uv_interpreter::interpreter Resolved python3 to /usr/local/bin/python3
    0.001802s DEBUG uv_interpreter::interpreter Using cached markers for: /usr/local/bin/python3
    0.001805s DEBUG uv::commands::pip_compile Using Python 3.11.7 interpreter at /usr/local/bin/python3 for builds
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.002439s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.7
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.002465s   0ms DEBUG uv_resolver::resolver Adding direct dependency: tensorrt==8.6.1.post1
   uv_resolver::resolver::choose_version package=tensorrt
     uv_resolver::resolver::package_wait package_name=tensorrt
 uv_resolver::resolver::process_request request=Versions tensorrt
   uv_client::registry_client::simple_api package=tensorrt
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/root/.cache/uv/simple-v0/pypi/tensorrt.rkyv
 uv_resolver::resolver::process_request request=Prefetch tensorrt ==8.6.1.post1
          0.003904s   1ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/tensorrt/
 uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=tensorrt==8.6.1.post1
     uv_client::cached_client::get_serde 
       uv_client::cached_client::get_cacheable 
         uv_client::cached_client::read_and_parse_cache file=/root/.cache/uv/built-wheels-v0/pypi/tensorrt/8.6.1.post1/manifest.msgpack
        0.004128s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of tensorrt (==8.6.1.post1)
        0.004133s   1ms DEBUG uv_resolver::resolver Selecting: tensorrt==8.6.1.post1 (tensorrt-8.6.1.post1.tar.gz)
   uv_resolver::resolver::get_dependencies package=tensorrt, version=8.6.1.post1
     uv_resolver::resolver::distributions_wait package_id=tensorrt-8.6.1.post1
            0.004349s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/46/08/da496b4f20890b6f717f159bbdda98e9248b560e712aa22cd9bb1daafe15/tensorrt-8.6.1.post1.tar.gz
     uv_distribution::source::build_source_dist_metadata 
          0.004646s   0ms DEBUG uv_distribution::source Preparing metadata for: tensorrt==8.6.1.post1
       uv_dispatch::setup_build package_id="tensorrt==8.6.1.post1", subdirectory=None
         uv_resolver::resolver::solve 
              0.005012s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.7
           uv_resolver::resolver::choose_version package=root
           uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
                0.005027s   0ms DEBUG uv_resolver::resolver Adding direct dependency: setuptools>=40.8.0
                0.005030s   0ms DEBUG uv_resolver::resolver Adding direct dependency: wheel*
           uv_resolver::resolver::choose_version package=setuptools
             uv_resolver::resolver::package_wait package_name=setuptools
         uv_resolver::resolver::process_request request=Versions setuptools
           uv_client::registry_client::simple_api package=setuptools
             uv_client::cached_client::get_cacheable 
               uv_client::cached_client::read_and_parse_cache file=/root/.cache/uv/simple-v0/pypi/setuptools.rkyv
         uv_resolver::resolver::process_request request=Versions wheel
           uv_client::registry_client::simple_api package=wheel
             uv_client::cached_client::get_cacheable 
               uv_client::cached_client::read_and_parse_cache file=/root/.cache/uv/simple-v0/pypi/wheel.rkyv
         uv_resolver::resolver::process_request request=Prefetch wheel *
         uv_resolver::resolver::process_request request=Prefetch setuptools >=40.8.0
                  0.005882s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/wheel/
 uv_resolver::version_map::from_metadata 
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=wheel==0.42.0
             uv_client::registry_client::wheel_metadata built_dist=wheel==0.42.0
               uv_client::cached_client::get_serde 
                 uv_client::cached_client::get_cacheable 
                   uv_client::cached_client::read_and_parse_cache file=/root/.cache/uv/wheels-v0/pypi/wheel/wheel-0.42.0-py3-none-any.msgpack
                      0.006339s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/c7/c3/55076fc728723ef927521abaa1955213d094933dc36d4a2008d5101e1af5/wheel-0.42.0-py3-none-any.whl
                  0.007274s   2ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/setuptools/
 uv_resolver::version_map::from_metadata 
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==69.1.0
             uv_client::registry_client::wheel_metadata built_dist=setuptools==69.1.0
               uv_client::cached_client::get_serde 
                 uv_client::cached_client::get_cacheable 
                   uv_client::cached_client::read_and_parse_cache file=/root/.cache/uv/wheels-v0/pypi/setuptools/setuptools-69.1.0-py3-none-any.msgpack
                0.007641s   2ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (>=40.8.0)
                0.007645s   2ms DEBUG uv_resolver::resolver Selecting: setuptools==69.1.0 (setuptools-69.1.0-py3-none-any.whl)
           uv_resolver::resolver::get_dependencies package=setuptools, version=69.1.0
             uv_resolver::resolver::distributions_wait package_id=setuptools-69.1.0
                      0.007982s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/bb/0a/203797141ec9727344c7649f6d5f6cf71b89a6c28f8f55d4f18de7a1d352/setuptools-69.1.0-py3-none-any.whl
           uv_resolver::resolver::choose_version package=wheel
             uv_resolver::resolver::package_wait package_name=wheel
                0.008038s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of wheel (*)
                0.008041s   0ms DEBUG uv_resolver::resolver Selecting: wheel==0.42.0 (wheel-0.42.0-py3-none-any.whl)
           uv_resolver::resolver::get_dependencies package=wheel, version=0.42.0
             uv_resolver::resolver::distributions_wait package_id=wheel-0.42.0
         uv_dispatch::install resolution="setuptools==69.1.0, wheel==0.42.0", venv="/tmp/.tmpRr6p9C/.venv"
              0.008074s   0ms DEBUG uv_dispatch Installing in setuptools==69.1.0, wheel==0.42.0 in /tmp/.tmpRr6p9C/.venv
              0.008117s   0ms DEBUG uv_installer::plan Requirement already cached: setuptools==69.1.0
              0.008130s   0ms DEBUG uv_installer::plan Requirement already cached: wheel==0.42.0
              0.008135s   0ms DEBUG uv_dispatch Installing build requirements: setuptools==69.1.0, wheel==0.42.0
           uv_installer::installer::install num_wheels=2
          0.020606s  15ms DEBUG uv_build Calling `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel()`
       uv_build::run_python_script script="prepare_metadata_for_build_wheel", python_version=3.11.7
error: Failed to download and build: tensorrt==8.6.1.post1
  Caused by: Failed to build: tensorrt==8.6.1.post1
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/tmp/.tmpRr6p9C/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/tmp/.tmpRr6p9C/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/tmp/.tmpRr6p9C/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 103, in <module>
  File "<string>", line 96, in parent_command_line
  File "/usr/local/lib/python3.11/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/subprocess.py", line 548, in run
    with Popen(*popenargs, **kwargs) as process:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/usr/local/lib/python3.11/subprocess.py", line 1950, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'ps'
---
```


---

_Label `bug` added by @charliermarsh on 2024-02-17 02:53_

---

_Comment by @zanieb on 2024-02-17 04:02_

Hm this compiles for me on amd64 linux and arm64 macos

```bash
$ echo "tensorrt==8.6.1.post1" | uv pip compile -  
Resolved 1 package in 792ms
```

Are you missing `ps`?

```bash
$ which ps
/usr/bin/ps
```

---

_Label `needs-reproduction` added by @zanieb on 2024-02-17 04:02_

---

_Comment by @Di-Is on 2024-02-17 14:07_

@zanieb 

I checked and the ps command does not exist in my environment (docker python container).
I installed the ps command and successfully resolved the dependency.
I close the issue as it was specific to my environment.


---

_Closed by @Di-Is on 2024-02-17 14:07_

---

_Comment by @charliermarsh on 2024-02-17 14:18_

@Di-Is - Thanks for following up :)

---

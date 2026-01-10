```yaml
number: 7549
title: "`uv pip install` failed on Windows: `stream did not contain valid UTF-8`"
type: issue
state: closed
author: jfcherng
labels:
  - bug
  - windows
assignees: []
created_at: 2024-09-19T13:46:22Z
updated_at: 2024-10-15T02:31:58Z
url: https://github.com/astral-sh/uv/issues/7549
synced_at: 2026-01-10T04:45:10Z
```

# `uv pip install` failed on Windows: `stream did not contain valid UTF-8`

---

_Issue opened by @jfcherng on 2024-09-19 13:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This issue seems to only happen on Windows.

## The command you invoked

The command is `uv pip install peewee` 

Python is `cp312` if this matters.

> [jfcherng@jfcherng-G614JZ x]$ uv pip install peewee
⠼ peewee==3.17.6                                                                                                        error: Failed to download and build `peewee==3.17.6`
  Caused by: Failed to run `C:\Users\jfcherng\AppData\Local\uv\cache\builds-v0\.tmpIbfNAj\Scripts\python.exe`
  Caused by: stream did not contain valid UTF-8

It can be installed via `pip install peewee` without an issue however.

## The current uv platform.

Win11 x64 23H2 22631.4169

## The current uv version (`uv --version`).

`uv 0.4.12 (2545bca69 2024-09-18)`






---

_Comment by @skeapskeap on 2024-09-23 11:30_

I'm facing the same issue but with `httptools`
```
PS C:\Users\username\PycharmProjects\project> uv pip install httptools==0.2.0
Resolved 1 package in 531ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: httptools==0.2.0
  Caused by: Failed to run `C:\Users\username\AppData\Local\uv\cache\builds-v0\.tmpyGy0iZ\Scripts\python.exe`
  Caused by: stream did not contain valid UTF-8
```
Windows 10 Pro 22H2 19045.4894
**uv version** `uv 0.4.10 (690716484 2024-09-13)`

---

_Renamed from "Failed to install `peewee` on Windows" to "`uv pip install` failed on Windows: `stream did not contain valid UTF-8`" by @jfcherng on 2024-09-23 11:56_

---

_Comment by @skeapskeap on 2024-09-23 12:09_

In addition to my previous comment. It seems that the problem is actually with `httptools==0.2.0` itself because `pip` can't install it on my system as well.
At the same time the project works fine with `pip` because it resolves uvicorn[standard]==0.15.0 with other deps to httptools==0.6.1 while `uv sync` resolves it to `httptools==0.2.0`
This is how the error looks like when installing `httptools==0.2.0` with pip.
```
      ...
      parser.c
      httptools/parser/parser.c(212): fatal error C1083: ЌҐ г¤ Ґвбп ®вЄалвм д ©« ўЄ«озҐ­ЁҐ: longintrepr.h: No such file or directory,
      error: command 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2022\\BuildTools\\VC\\Tools\\MSVC\\14.38.33130\\bin\\HostX86\\x64\\cl.exe' failed with exit code 2
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for httptools
Failed to build httptools
ERROR: Could not build wheels for httptools, which is required to install pyproject.toml-based projects
```

---

_Comment by @charliermarsh on 2024-09-28 12:39_

This should be fixed by https://github.com/astral-sh/uv/pull/7757.

---

_Label `bug` added by @charliermarsh on 2024-09-28 12:39_

---

_Label `windows` added by @charliermarsh on 2024-09-28 12:39_

---

_Closed by @charliermarsh on 2024-09-28 12:43_

---

_Comment by @DeemanOne on 2024-10-03 19:53_

Hey, I have the same error with peewee, still after updating to 0.4.18: 
uv pip install yfinance
⠦ peewee==3.17.6                                                                                                                                                                             
  × Failed to download and build `peewee==3.17.6`
  ├─▶ Failed to run `C:\Users\Deeman\AppData\Local\uv\cache\builds-v0\.tmp8g782B\Scripts\python.exe`
  ╰─▶ stream did not contain valid UTF-8
Also am on Win11 and py312

---

_Comment by @Paillat-dev on 2024-10-03 19:56_

@DeemanOne Are you sure it is up to date ? What is the output of `uv --version`, and how did you install uv ?

---

_Comment by @jfcherng on 2024-10-04 07:27_

> @DeemanOne Are you sure it is up to date ? What is the output of `uv --version`, and how did you install uv ?

```
$ uv pip install -U peewee
⠹ peewee==3.17.6
  × Failed to download and build `peewee==3.17.6`
  ├─▶ Failed to run `C:\Users\jfcherng\AppData\Local\uv\cache\builds-v0\.tmpPVYdDy\Scripts\python.exe`
  ╰─▶ stream did not contain valid UTF-8
$ uv -V
uv 0.4.18 (7b55e9790 2024-10-01)
```

---

_Comment by @jfcherng on 2024-10-11 03:38_

Here's the full log with `uv 0.4.20`: [output.log](https://github.com/user-attachments/files/17337293/output.log)

Probably the same issue with https://github.com/astral-sh/uv/issues/8009

---

_Comment by @faulander on 2024-10-14 11:52_

still cannot install peewee with 0.4.20:
```
(backend) PS C:\HF\_PROJECTS\parkbuddies\backend> uv pip install -vv -n peewee
    0.001806s DEBUG uv uv 0.4.20
 uv_requirements::specification::from_source source=peewee
    0.004276s DEBUG uv_python::discovery Searching for default Python interpreter in system path or `py` launcher
    0.155613s DEBUG uv_python::discovery Found `cpython-3.12.5-windows-x86_64-none` at `C:\HF\_PROJECTS\parkbuddies\backend\.venv\Scripts\python.exe` (active virtual environment)
    0.156230s DEBUG uv::commands::pip::operations Using Python 3.12.5 environment at .venv
    0.156615s DEBUG uv_fs Acquired lock for `.venv`
    0.157012s DEBUG uv::commands::pip::install At least one requirement is not satisfied: peewee
 uv_client::linehaul::linehaul
    0.157893s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_resolver::resolver::solve
    0.159084s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.12.5
    0.159212s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.12.5
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.159871s   0ms DEBUG uv_resolver::resolver Adding direct dependency: peewee*
   uv_resolver::resolver::choose_version package=peewee
 uv_resolver::resolver::process_request request=Versions peewee
   uv_client::registry_client::simple_api package=peewee
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\simple-v13\pypi\peewee.rkyv
 uv_resolver::resolver::process_request request=Prefetch peewee *
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\simple-v13\\pypi\\peewee.rkyv"
        0.161155s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/peewee/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/peewee/"
       uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\simple-v13\pypi\peewee.rkyv
       uv_client::registry_client::parse_simple_api package=peewee
   uv_resolver::version_map::from_metadata
      0.259348s  99ms DEBUG uv_resolver::resolver Searching for a compatible version of peewee (*)
      0.259571s  99ms DEBUG uv_resolver::resolver Selecting: peewee==3.17.6 [compatible] (peewee-3.17.6.tar.gz)
   uv_resolver::resolver::get_dependencies_forking package=peewee, version=3.17.6
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=peewee==3.17.6
     uv_resolver::resolver::get_dependencies package=peewee, version=3.17.6
    0.260974s DEBUG uv_fs Acquired lock for `C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\sdists-v4\pypi\peewee\3.17.6`
     uv_client::cached_client::get_serde
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\sdists-v4\pypi\peewee\3.17.6\revision.http
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\sdists-v4\\pypi\\peewee\\3.17.6\\revision.http"
          0.262198s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/bd/be/e9c886b4601a19f4c34a1b75c5fe8b98a2115dd964251a76b24c977c369d/peewee-3.17.6.tar.gz
         uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/bd/be/e9c886b4601a19f4c34a1b75c5fe8b98a2115dd964251a76b24c977c369d/peewee-3.17.6.tar.gz"
         uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\sdists-v4\pypi\peewee\3.17.6\revision.http
         uv_distribution::source::download source_dist=peewee==3.17.6
            0.401122s   0ms DEBUG uv_distribution::source Downloading source distribution: peewee==3.17.6
           uv_distribution::source::download_source_dist filename="peewee-3.17.6.tar.gz", source_dist=peewee==3.17.6
      1.032571s 773ms DEBUG uv_distribution::source No static `pyproject.toml` available for: peewee==3.17.6 (PyprojectToml(FieldNotFound("project")))
      1.032954s 773ms DEBUG uv_distribution::source No static `PKG-INFO` available for: peewee==3.17.6 (PkgInfo(UnsupportedMetadataVersion("2.1")))
      1.033259s 773ms DEBUG uv_distribution::source No static `egg-info` available for: peewee==3.17.6 (MissingRequiresTxt)
     uv_distribution::source::build_metadata dist=peewee==3.17.6
        1.033622s   0ms DEBUG uv_distribution::source Preparing metadata for: peewee==3.17.6
       uv_dispatch::setup_build version_id="peewee==3.17.6", subdirectory=None
          1.034412s   0ms DEBUG uv_virtualenv::virtualenv Ignoring empty directory
          1.041387s   7ms DEBUG uv_build_frontend Resolving build requirements
 uv_resolver::resolver::solve
    1.042012s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.12.5
    1.042189s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.12.5
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    1.042835s   0ms DEBUG uv_resolver::resolver Adding direct dependency: setuptools*
    1.042966s   1ms DEBUG uv_resolver::resolver Adding direct dependency: wheel*
   uv_resolver::resolver::choose_version package=setuptools
         uv_resolver::resolver::process_request request=Versions setuptools
           uv_client::registry_client::simple_api package=setuptools
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\simple-v13\pypi\setuptools.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\simple-v13\\pypi\\setuptools.rkyv"
         uv_resolver::resolver::process_request request=Versions wheel
           uv_client::registry_client::simple_api package=wheel
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\simple-v13\pypi\wheel.rkyv
         uv_resolver::resolver::process_request request=Prefetch wheel *
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\simple-v13\\pypi\\wheel.rkyv"
         uv_resolver::resolver::process_request request=Prefetch setuptools *
                1.045705s   2ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/setuptools/
               uv_client::cached_client::fresh_request url="https://pypi.org/simple/setuptools/"
                1.046011s   1ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/wheel/
               uv_client::cached_client::fresh_request url="https://pypi.org/simple/wheel/"
               uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\simple-v13\pypi\setuptools.rkyv
               uv_client::registry_client::parse_simple_api package=setuptools
               uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\simple-v13\pypi\wheel.rkyv
               uv_client::registry_client::parse_simple_api package=wheel
           uv_resolver::version_map::from_metadata
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=wheel==0.44.0
             uv_client::registry_client::wheel_metadata built_dist=wheel==0.44.0
               uv_client::cached_client::get_serde
                 uv_client::cached_client::get_cacheable
                   uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\wheel\wheel-0.44.0-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\wheels-v2\\pypi\\wheel\\wheel-0.44.0-py3-none-any.msgpack"
                    1.145410s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl.metadata
                   uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl.metadata"
                   uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\wheel\wheel-0.44.0-py3-none-any.msgpack
                   uv_client::registry_client::parse_metadata21
                  1.179848s  93ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b1-py2.3.egg
                  1.180237s  93ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b1-py2.4.egg
                  1.180763s  94ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b2-py2.3.egg
                  1.181026s  94ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b2-py2.4.egg
                  1.181355s  94ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b3-py2.3.egg
                  1.181621s  94ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b3-py2.4.egg
                  1.181892s  95ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b4-py2.3.egg
                  1.182124s  95ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6b4-py2.4.egg
                  1.182336s  95ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c1-py2.3.egg
                  1.182505s  95ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c1-py2.4.egg
                  1.182658s  95ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10-1.src.rpm
                  1.182807s  96ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10-py2.3.egg
                  1.182950s  96ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10-py2.4.egg
                  1.183095s  96ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10-py2.5.egg
                  1.183243s  96ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10-py2.6.egg
                  1.183393s  96ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10.win32-py2.3.exe
                  1.183529s  96ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10.win32-py2.4.exe
                  1.183667s  96ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10.win32-py2.5.exe
                  1.183805s  97ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c10.win32-py2.6.exe
                  1.183951s  97ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11-1.src.rpm
                  1.184101s  97ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11-py2.3.egg
                  1.184237s  97ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11-py2.4.egg
                  1.184372s  97ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11-py2.5.egg
                  1.184543s  97ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11-py2.6.egg
                  1.184726s  97ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11-py2.7.egg
                  1.184888s  98ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11.win32-py2.3.exe
                  1.185047s  98ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11.win32-py2.4.exe
                  1.185199s  98ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11.win32-py2.5.exe
                  1.185356s  98ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11.win32-py2.6.exe
                  1.185512s  98ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c11.win32-py2.7.exe
                  1.185674s  98ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c2-py2.3.egg
                  1.185816s  99ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c2-py2.4.egg
                  1.186023s  99ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c3-py2.3.egg
                  1.186187s  99ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c3-py2.4.egg
                  1.186343s  99ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c3-py2.5.egg
                  1.186511s  99ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c4-1.src.rpm
                  1.186629s  99ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c4-py2.3.egg
                  1.186742s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c4-py2.4.egg
                  1.186854s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c4-py2.5.egg
                  1.186970s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c4.win32-py2.3.exe
                  1.187086s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c4.win32-py2.4.exe
                  1.187200s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c4.win32-py2.5.exe
                  1.187324s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c5-1.src.rpm
                  1.187464s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c5-py2.3.egg
                  1.187608s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c5-py2.4.egg
                  1.187735s 100ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c5-py2.5.egg
                  1.187863s 101ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c5.win32-py2.3.exe
                  1.187980s 101ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c5.win32-py2.4.exe
                  1.188097s 101ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c5.win32-py2.5.exe
                  1.188229s 101ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c6-1.src.rpm
                  1.188347s 101ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c6-py2.3.egg
                  1.188461s 101ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c6-py2.4.egg
                  1.188583s 101ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c6-py2.5.egg
                  1.188778s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c6.win32-py2.3.exe
                  1.188908s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c6.win32-py2.4.exe
                  1.189040s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c6.win32-py2.5.exe
                  1.189169s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c7-1.src.rpm
                  1.189290s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c7-py2.3.egg
                  1.189419s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c7-py2.4.egg
                  1.189562s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c7-py2.5.egg
                  1.189696s 102ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c7.win32-py2.3.exe
                  1.189829s 103ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c7.win32-py2.4.exe
                  1.189971s 103ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c7.win32-py2.5.exe
                  1.190113s 103ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c8-1.src.rpm
                  1.190248s 103ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c8-py2.3.egg
                  1.190387s 103ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c8-py2.4.egg
                  1.190521s 103ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c8-py2.5.egg
                  1.190653s 103ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c8.win32-py2.3.exe
                  1.190795s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c8.win32-py2.4.exe
                  1.190941s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c8.win32-py2.5.exe
                  1.191071s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9-1.src.rpm
                  1.191187s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9-py2.3.egg
                  1.191298s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9-py2.4.egg
                  1.191413s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9-py2.5.egg
                  1.191538s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9-py2.6.egg
                  1.191659s 104ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9.win32-py2.3.exe
                  1.191785s 105ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9.win32-py2.4.exe
                  1.191900s 105ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-0.6c9.win32-py2.5.exe
                  1.192543s 105ms  WARN uv_client::registry_client Skipping file for setuptools: setuptools-18.3.1-py3.4.egg
           uv_resolver::version_map::from_metadata 
      1.202683s 159ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (*)
      1.202841s 159ms DEBUG uv_resolver::resolver Selecting: setuptools==75.1.0 [compatible] (setuptools-75.1.0-py3-none-any.whl)
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==75.1.0
             uv_client::registry_client::wheel_metadata built_dist=setuptools==75.1.0
   uv_resolver::resolver::get_dependencies_forking package=setuptools, version=75.1.0
     uv_resolver::resolver::get_dependencies package=setuptools, version=75.1.0
               uv_client::cached_client::get_serde
                 uv_client::cached_client::get_cacheable
                   uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\setuptools\setuptools-75.1.0-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\wheels-v2\\pypi\\setuptools\\setuptools-75.1.0-py3-none-any.msgpack"
                    1.204146s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/ff/ae/f19306b5a221f6a436d8f2238d5b80925004093fa3edea59835b514d9057/setuptools-75.1.0-py3-none-any.whl.metadata
                   uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/ff/ae/f19306b5a221f6a436d8f2238d5b80925004093fa3edea59835b514d9057/setuptools-75.1.0-py3-none-any.whl.metadata"
                   uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\setuptools\setuptools-75.1.0-py3-none-any.msgpack
                   uv_client::registry_client::parse_metadata21
   uv_resolver::resolver::choose_version package=wheel
      1.239482s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of wheel (*)
      1.239839s   0ms DEBUG uv_resolver::resolver Selecting: wheel==0.44.0 [compatible] (wheel-0.44.0-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies_forking package=wheel, version=0.44.0
     uv_resolver::resolver::get_dependencies package=wheel, version=0.44.0
    1.240623s 198ms DEBUG uv_resolver::resolver::batch_prefetch Tried 2 versions: setuptools 1, wheel 1
    1.240770s 198ms DEBUG uv_resolver::resolver Split specific environment resolution took 0.198s
         uv_dispatch::install resolution="setuptools==75.1.0, wheel==0.44.0", venv="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\builds-v0\\.tmpouBUYP"
            1.241329s   0ms DEBUG uv_dispatch Installing in setuptools==75.1.0, wheel==0.44.0 in C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP
            1.241773s   0ms DEBUG uv_installer::plan Identified uncached distribution: setuptools==75.1.0
            1.242019s   0ms DEBUG uv_installer::plan Identified uncached distribution: wheel==0.44.0
            1.242160s   0ms DEBUG uv_dispatch Downloading and building requirements for build: setuptools==75.1.0, wheel==0.44.0
           uv_installer::preparer::prepare total=2
             uv_installer::preparer::get_wheel name=setuptools==75.1.0, size=Some(1248506), url="https://files.pythonhosted.org/packages/ff/ae/f19306b5a221f6a436d8f2238d5b80925004093fa3edea59835b514d9057/setuptools-75.1.0-py3-none-any.whl"
               uv_distribution::distribution_database::get_or_build_wheel dist=setuptools==75.1.0
                 uv_client::cached_client::get_serde
                   uv_client::cached_client::get_cacheable
                     uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\setuptools\setuptools-75.1.0-py3-none-any.http
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\wheels-v2\\pypi\\setuptools\\setuptools-75.1.0-py3-none-any.http"
             uv_installer::preparer::get_wheel name=wheel==0.44.0, size=Some(67059), url="https://files.pythonhosted.org/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl"
               uv_distribution::distribution_database::get_or_build_wheel dist=wheel==0.44.0
                 uv_client::cached_client::get_serde
                   uv_client::cached_client::get_cacheable
                     uv_client::cached_client::read_and_parse_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\wheel\wheel-0.44.0-py3-none-any.http
 uv_client::cached_client::from_path_sync path="C:\\Users\\hf\\AppData\\Local\\Temp\\.tmpyx8wj3\\wheels-v2\\pypi\\wheel\\wheel-0.44.0-py3-none-any.http"
                      1.244329s   1ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/ff/ae/f19306b5a221f6a436d8f2238d5b80925004093fa3edea59835b514d9057/setuptools-75.1.0-py3-none-any.whl
                     uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/ff/ae/f19306b5a221f6a436d8f2238d5b80925004093fa3edea59835b514d9057/setuptools-75.1.0-py3-none-any.whl"
                      1.244808s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl
                     uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl"
                     uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\setuptools\setuptools-75.1.0-py3-none-any.http
                     uv_distribution::distribution_database::wheel wheel=setuptools==75.1.0
                     uv_client::cached_client::new_cache file=C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\wheels-v2\pypi\wheel\wheel-0.44.0-py3-none-any.http
                     uv_distribution::distribution_database::wheel wheel=wheel==0.44.0
            2.046034s 804ms DEBUG uv_dispatch Installing build requirements: wheel==0.44.0, setuptools==75.1.0
           uv_installer::installer::install num_wheels=2
 uv_installer::installer::install num_wheels=2
   uv_install_wheel::linker::install_wheel wheel=wheel-0.44.0-py3-none-any.whl
 uv_install_wheel::linker::install_wheel wheel=setuptools-75.1.0-py3-none-any.whl
      2.047097s   0ms DEBUG uv_install_wheel::linker Extracting file, name=PackageName("wheel")
     uv_install_wheel::linker::link_wheel_files
    2.047359s   0ms DEBUG uv_install_wheel::linker Extracting file, name=PackageName("setuptools")
   uv_install_wheel::linker::link_wheel_files
      2.056727s  10ms DEBUG uv_install_wheel::linker Extracted 34 files, name=PackageName("wheel")
      2.059201s  12ms DEBUG uv_install_wheel::linker Writing entrypoints, name=PackageName("wheel")
      2.061081s  14ms DEBUG uv_install_wheel::linker No data, name=PackageName("wheel")
      2.061261s  14ms DEBUG uv_install_wheel::linker Writing extra metadata, name=PackageName("wheel")
      2.063712s  17ms DEBUG uv_install_wheel::linker Writing record, name=PackageName("wheel")
    2.199568s 152ms DEBUG uv_install_wheel::linker Extracted 554 files, name=PackageName("setuptools")
    2.200214s 153ms DEBUG uv_install_wheel::linker No entrypoints, name=PackageName("setuptools")
    2.200381s 153ms DEBUG uv_install_wheel::linker No data, name=PackageName("setuptools")
    2.200532s 153ms DEBUG uv_install_wheel::linker Writing extra metadata, name=PackageName("setuptools")
    2.202620s 155ms DEBUG uv_install_wheel::linker Writing record, name=PackageName("setuptools")
          2.203296s   1s  DEBUG uv_build_frontend Creating PEP 517 build environment
          2.203424s   1s  DEBUG uv_build_frontend Calling `setuptools.build_meta.get_requires_for_build_wheel()`
         uv_build_frontend::run_python_script script="get_requires_for_build_wheel", version_id="peewee==3.17.6"
            5.655588s   3s  DEBUG uv_build_frontend test_pw_sqlite3.c
            5.704216s   3s  DEBUG uv_build_frontend <string>:111: UserWarning: Could not find libsqlite3, SQLite extensions will not be built.
            5.791341s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.792062s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.792498s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.792699s   3s  DEBUG uv_build_frontend     self.flush()
            5.792918s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.793190s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.793313s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.793530s   3s  DEBUG uv_build_frontend Call stack:
            5.794589s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.794775s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.794924s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.795074s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.795221s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.795362s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.795508s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.795652s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.795795s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.795930s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.796067s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.796203s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.796339s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.796467s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.796610s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.796762s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.796909s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.797115s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.797345s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.797457s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 970, in run_command
            5.797602s   3s  DEBUG uv_build_frontend     log.info("running %s", command)
            5.797764s   3s  DEBUG uv_build_frontend Message: 'running %s'
            5.797934s   3s  DEBUG uv_build_frontend Arguments: ('egg_info',)
            5.799779s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.799925s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.800067s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.800187s   3s  DEBUG uv_build_frontend     self.flush()
            5.800292s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.800401s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.800504s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.800609s   3s  DEBUG uv_build_frontend Call stack:
            5.800881s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.801017s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.801144s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.801260s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.801383s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.801498s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.801653s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.801802s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.801976s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.802108s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.802246s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.802381s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.802514s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.802648s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.802779s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.802904s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.803030s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.803146s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.803277s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.803405s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 973, in run_command
            5.803547s   3s  DEBUG uv_build_frontend     cmd_obj.run()
            5.803677s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 304, in run
            5.804105s   3s  DEBUG uv_build_frontend     writer(self, ep.name, os.path.join(self.egg_info, ep.name))
            5.804275s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 648, in write_pkg_info
            5.804446s   3s  DEBUG uv_build_frontend     log.info("writing %s", filename)
            5.804591s   3s  DEBUG uv_build_frontend Message: 'writing %s'
            5.804720s   3s  DEBUG uv_build_frontend Arguments: ('peewee.egg-info\\PKG-INFO',)
            5.804882s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.805001s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.805120s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.805245s   3s  DEBUG uv_build_frontend     self.flush()
            5.805351s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.805462s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.805565s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.805668s   3s  DEBUG uv_build_frontend Call stack:
            5.805773s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.805886s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.806020s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.806143s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.806272s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.806386s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.806507s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.806617s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.806744s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.806876s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.807012s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.807144s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.807269s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.807380s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.807498s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.807607s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.807725s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.807845s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.807962s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.808080s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 973, in run_command
            5.808217s   3s  DEBUG uv_build_frontend     cmd_obj.run()
            5.808341s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 304, in run
            5.808464s   3s  DEBUG uv_build_frontend     writer(self, ep.name, os.path.join(self.egg_info, ep.name))
            5.808579s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 689, in overwrite_arg
            5.808699s   3s  DEBUG uv_build_frontend     write_arg(cmd, basename, filename, True)
            5.808811s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 697, in write_arg
            5.808923s   3s  DEBUG uv_build_frontend     cmd.write_or_delete_file(argname, filename, value, force)
            5.809030s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 268, in write_or_delete_file
            5.809143s   3s  DEBUG uv_build_frontend     self.write_file(what, filename, data)
            5.809248s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 282, in write_file
            5.809360s   3s  DEBUG uv_build_frontend     log.info("writing %s to %s", what, filename)
            5.809466s   3s  DEBUG uv_build_frontend Message: 'writing %s to %s'
            5.809571s   3s  DEBUG uv_build_frontend Arguments: ('dependency_links', 'peewee.egg-info\\dependency_links.txt')
            5.809678s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.809813s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.809962s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.810097s   3s  DEBUG uv_build_frontend     self.flush()
            5.810211s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.810333s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.810469s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.810607s   3s  DEBUG uv_build_frontend Call stack:
            5.810739s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.810873s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.811014s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.811164s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.811309s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.811439s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.811584s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.811720s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.811860s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.811992s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.812118s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.812235s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.812362s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.812490s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.812611s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.812723s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.812850s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.812962s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.813080s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.813199s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 973, in run_command
            5.813331s   3s  DEBUG uv_build_frontend     cmd_obj.run()
            5.813451s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 304, in run
            5.813583s   3s  DEBUG uv_build_frontend     writer(self, ep.name, os.path.join(self.egg_info, ep.name))
            5.813712s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 685, in write_toplevel_names
            5.813871s   3s  DEBUG uv_build_frontend     cmd.write_file("top-level names", filename, '\n'.join(sorted(pkgs)) + '\n')
            5.814049s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 282, in write_file
            5.814193s   3s  DEBUG uv_build_frontend     log.info("writing %s to %s", what, filename)
            5.814324s   3s  DEBUG uv_build_frontend Message: 'writing %s to %s'
            5.814451s   3s  DEBUG uv_build_frontend Arguments: ('top-level names', 'peewee.egg-info\\top_level.txt')
            5.839250s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.839485s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.839654s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.839823s   3s  DEBUG uv_build_frontend     self.flush()
            5.839971s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.840096s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.840217s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.840366s   3s  DEBUG uv_build_frontend Call stack:
            5.840540s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.840735s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.840937s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.841131s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.841329s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.841516s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.841714s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.841903s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.842089s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.842278s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.842442s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.842590s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.842733s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.842869s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.843011s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.843146s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.843311s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.843441s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.843570s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.843683s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 973, in run_command
            5.843809s   3s  DEBUG uv_build_frontend     cmd_obj.run()
            5.843937s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 311, in run
            5.844073s   3s  DEBUG uv_build_frontend     self.find_sources()
            5.844203s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 319, in find_sources
            5.844341s   3s  DEBUG uv_build_frontend     mm.run()
            5.844473s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 540, in run
            5.844608s   3s  DEBUG uv_build_frontend     self.add_defaults()
            5.844744s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 585, in add_defaults
            5.844977s   3s  DEBUG uv_build_frontend     self.read_manifest()
            5.845121s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\sdist.py", line 202, in read_manifest
            5.845251s   3s  DEBUG uv_build_frontend     log.info("reading manifest file '%s'", self.manifest)
            5.845381s   3s  DEBUG uv_build_frontend Message: "reading manifest file '%s'"
            5.845507s   3s  DEBUG uv_build_frontend Arguments: ('peewee.egg-info\\SOURCES.txt',)
            5.856318s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.856535s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.856658s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.856809s   3s  DEBUG uv_build_frontend     self.flush()
            5.857013s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.857157s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.857351s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.857478s   3s  DEBUG uv_build_frontend Call stack:
            5.857655s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.857782s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.857905s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.858026s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.858145s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.858255s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.858371s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.858503s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.858655s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.858814s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.858976s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.859125s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.859296s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.859406s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.859522s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.859727s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.859880s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.860101s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.860238s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.860355s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 973, in run_command
            5.860484s   3s  DEBUG uv_build_frontend     cmd_obj.run()
            5.860608s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 311, in run
            5.860737s   3s  DEBUG uv_build_frontend     self.find_sources()
            5.860878s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 319, in find_sources
            5.861013s   3s  DEBUG uv_build_frontend     mm.run()
            5.861133s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 542, in run
            5.861257s   3s  DEBUG uv_build_frontend     self.read_template()
            5.861372s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\command\sdist.py", line 341, in read_template
            5.861495s   3s  DEBUG uv_build_frontend     log.info("reading manifest template '%s'", self.template)
            5.861620s   3s  DEBUG uv_build_frontend Message: "reading manifest template '%s'"
            5.861756s   3s  DEBUG uv_build_frontend Arguments: ('MANIFEST.in',)
            5.861930s   3s  DEBUG uv_build_frontend warning: no files found matching 'tests.py'
            5.862056s   3s  DEBUG uv_build_frontend warning: no files found matching 'playhouse\pskel'
            5.862174s   3s  DEBUG uv_build_frontend warning: no files found matching 'playhouse\tests\README'
            5.897429s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.897675s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.897842s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.898064s   3s  DEBUG uv_build_frontend     self.flush()
            5.898222s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.898390s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.898592s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.898762s   3s  DEBUG uv_build_frontend Call stack:
            5.898911s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.899055s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.899198s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.899340s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.899521s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.899679s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.899925s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.900178s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.900387s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.900643s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.900888s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.901080s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.901216s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.901342s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.901468s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.901605s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.901739s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.901870s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.902006s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.902137s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 973, in run_command
            5.902271s   3s  DEBUG uv_build_frontend     cmd_obj.run()
            5.902397s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 311, in run
            5.902533s   3s  DEBUG uv_build_frontend     self.find_sources()
            5.902659s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 319, in find_sources
            5.902850s   3s  DEBUG uv_build_frontend     mm.run()
            5.903043s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 543, in run
            5.903207s   3s  DEBUG uv_build_frontend     self.add_license_files()
            5.903357s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 598, in add_license_files
            5.903500s   3s  DEBUG uv_build_frontend     log.info("adding license file '%s'", lf)
            5.903631s   3s  DEBUG uv_build_frontend Message: "adding license file '%s'"
            5.903762s   3s  DEBUG uv_build_frontend Arguments: ('LICENSE',)
            5.912072s   3s  DEBUG uv_build_frontend --- Logging error ---
            5.912304s   3s  DEBUG uv_build_frontend Traceback (most recent call last):
            5.912510s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1164, in emit
            5.912649s   3s  DEBUG uv_build_frontend     self.flush()
            5.912776s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Roaming\uv\python\cpython-3.12.5-windows-x86_64-none\Lib\logging\__init__.py", line 1144, in flush
            5.912913s   3s  DEBUG uv_build_frontend     self.stream.flush()
            5.913034s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
            5.913148s   3s  DEBUG uv_build_frontend Call stack:
            5.913940s   3s  DEBUG uv_build_frontend   File "<string>", line 14, in <module>
            5.914104s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 332, in get_requires_for_build_wheel
            5.914238s   3s  DEBUG uv_build_frontend     return self._get_build_requires(config_settings, requirements=[])
            5.914372s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 302, in _get_build_requires
            5.914548s   3s  DEBUG uv_build_frontend     self.run_setup()
            5.914709s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\build_meta.py", line 318, in run_setup
            5.914864s   3s  DEBUG uv_build_frontend     exec(code, locals())
            5.915012s   3s  DEBUG uv_build_frontend   File "<string>", line 201, in <module>
            5.915184s   3s  DEBUG uv_build_frontend   File "<string>", line 153, in _do_setup
            5.915330s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\__init__.py", line 117, in setup
            5.915523s   3s  DEBUG uv_build_frontend     return distutils.core.setup(**attrs)
            5.915683s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 183, in setup
            5.915872s   3s  DEBUG uv_build_frontend     return run_commands(dist)
            5.916118s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\core.py", line 199, in run_commands
            5.916320s   3s  DEBUG uv_build_frontend     dist.run_commands()
            5.917247s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 954, in run_commands
            5.917636s   3s  DEBUG uv_build_frontend     self.run_command(cmd)
            5.917854s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\dist.py", line 950, in run_command
            5.918028s   3s  DEBUG uv_build_frontend     super().run_command(command)
            5.918164s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\dist.py", line 973, in run_command
            5.918310s   3s  DEBUG uv_build_frontend     cmd_obj.run()
            5.918449s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 311, in run
            5.918597s   3s  DEBUG uv_build_frontend     self.find_sources()
            5.918781s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 319, in find_sources
            5.918934s   3s  DEBUG uv_build_frontend     mm.run()
            5.919080s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 548, in run
            5.919327s   3s  DEBUG uv_build_frontend     self.write_manifest()
            5.919527s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\command\egg_info.py", line 564, in write_manifest
            5.919784s   3s  DEBUG uv_build_frontend     self.execute(write_file, (self.manifest, files), msg)
            5.919943s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\cmd.py", line 337, in execute
            5.920156s   3s  DEBUG uv_build_frontend     util.execute(func, args, msg, dry_run=self.dry_run)
            5.920348s   3s  DEBUG uv_build_frontend   File "C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Lib\site-packages\setuptools\_distutils\util.py", line 324, in execute
            5.920524s   3s  DEBUG uv_build_frontend     log.info(msg)
            5.920752s   3s  DEBUG uv_build_frontend Message: "writing manifest file 'peewee.egg-info\\SOURCES.txt'"
            5.920908s   3s  DEBUG uv_build_frontend Arguments: ()
            5.921108s   3s  DEBUG uv_build_frontend Exception ignored in: <_io.TextIOWrapper name='<stdout>' mode='w' encoding='utf-8'>
            5.921231s   3s  DEBUG uv_build_frontend OSError: [Errno 22] Invalid argument
      6.060516s   5s  DEBUG uv_fs Released lock at `C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\sdists-v4\pypi\peewee\3.17.6\.lock`
  × Failed to download and build `peewee==3.17.6`
  ├─▶ Failed to run `C:\Users\hf\AppData\Local\Temp\.tmpyx8wj3\builds-v0\.tmpouBUYP\Scripts\python.exe`
  ╰─▶ stream did not contain valid UTF-8
```

---

_Comment by @jfcherng on 2024-10-15 02:31_

uv v0.4.21 works on my side. Thanks!

---

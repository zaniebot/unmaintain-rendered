```yaml
number: 3599
title: Incompatible cached wheels may continue to be used after upgrade
type: issue
state: open
author: wimglenn
labels:
  - needs-mre
assignees: []
created_at: 2024-05-15T02:56:29Z
updated_at: 2025-10-07T15:43:13Z
url: https://github.com/astral-sh/uv/issues/3599
synced_at: 2026-01-12T15:58:45Z
```

# Incompatible cached wheels may continue to be used after upgrade

---

_@wimglenn_

I saw an issue installing pydantic on macOS where it uv would use an incompatible wheel for pydantic_core (with cp312-cp312-macosx_10_12_x86_64 tag) resulting in this error message:

```
$ .venv/bin/python3 -c 'import pydantic_core'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/.../.venv/lib/python3.12/site-packages/pydantic_core/__init__.py", line 6, in <module>
    from ._pydantic_core import (
ImportError: dlopen(/.../.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so, 0x0002): tried: '/.../.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64')), '/System/Volumes/Preboot/Cryptexes/OS/.../.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so' (no such file), '/.../.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64'))
```

pip would select a different one (with cp312-cp312-macosx_11_0_arm64 tag) which worked.

I had recently upgraded to `uv 0.1.44 (d417daad7 2024-05-14)`, and a `uv cache clean` fixed the issue - on reinstall it used a compatible wheel.

Are the UV caches keyed off the uv version at all? Perhaps the uv installation should self-invalidate caches.

---

_Comment by @charliermarsh on 2024-05-15 02:59_

Unfortunately it's hard to help here without more information and a reliable reproduction. We intentionally don't key the cache on uv version -- instead, each of the buckets has its own version that we bump whenever the schema changes.

---

_Comment by @charliermarsh on 2024-05-15 03:02_

Do you have any idea how that wheel ended up in the cache in the first place? Either way we shouldn't be re-using it unless it appears to be compatible with the current environment. Any reproduction welcome! Otherwise I may have to close.

---

_Label `needs-mre` added by @zanieb on 2024-05-15 03:43_

---

_Comment by @wimglenn on 2024-05-15 04:29_

I think I can reproduce it by creating an alternate install using brew - do these installations share files at all?

```
$ ~/.cargo/bin/uv --version
uv 0.1.44 (d417daad7 2024-05-14)
$ arch -x86_64 brew install -q uv
==> Fetching dependencies for uv: libssh2 and libgit2
==> Fetching libssh2
==> Fetching libgit2
==> Fetching uv
==> Installing dependencies for uv: libssh2 and libgit2
==> Installing uv dependency: libssh2
🍺  /usr/local/Cellar/libssh2/1.11.0_1: 198 files, 1.1MB
==> Installing uv dependency: libgit2
🍺  /usr/local/Cellar/libgit2/1.7.2: 105 files, 4.5MB
==> Installing uv
==> Caveats
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/uv/0.1.44: 13 files, 28.4MB
==> Running `brew cleanup uv`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> uv
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
$ ~/.cargo/bin/uv --version
uv 0.1.44 (d417daad7 2024-05-14)
$ /usr/local/Cellar/uv/0.1.44/bin/uv --version                                  
uv 0.1.44
$ ~/.cargo/bin/uv venv     
Using Python 3.12.3 interpreter at: /usr/local/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ /usr/local/Cellar/uv/0.1.44/bin/uv pip install pydantic-core --force-reinstall
Resolved 2 packages in 315ms
Downloaded 2 packages in 211ms
Installed 2 packages in 10ms
 + pydantic-core==2.18.2
 + typing-extensions==4.11.0
$ tail -1 .venv/lib/python3.12/site-packages/pydantic_core-2.18.2.dist-info/WHEEL
Tag: cp312-cp312-macosx_10_12_x86_64
$ .venv/bin/python -c 'import pydantic_core; print(pydantic_core.__file__)'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/private/tmp/u/.venv/lib/python3.12/site-packages/pydantic_core/__init__.py", line 6, in <module>
    from ._pydantic_core import (
ImportError: dlopen(/private/tmp/u/.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so, 0x0002): tried: '/private/tmp/u/.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64')), '/System/Volumes/Preboot/Cryptexes/OS/private/tmp/u/.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so' (no such file), '/private/tmp/u/.venv/lib/python3.12/site-packages/pydantic_core/_pydantic_core.cpython-312-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e' or 'arm64'))
$ ~/.cargo/bin/uv pip install pydantic-core --force-reinstall                               
Resolved 2 packages in 8ms
Installed 2 packages in 1ms
 - pydantic-core==2.18.2
 + pydantic-core==2.18.2
 - typing-extensions==4.11.0
 + typing-extensions==4.11.0
$ tail -1 .venv/lib/python3.12/site-packages/pydantic_core-2.18.2.dist-info/WHEEL
Tag: cp312-cp312-macosx_10_12_x86_64
$ ~/.cargo/bin/uv cache clean && ~/.cargo/bin/uv pip install pydantic-core --force-reinstall
Clearing cache at: /Users/wim/Library/Caches/uv
Removed 28 files (10.5MiB)
Resolved 2 packages in 245ms
Downloaded 2 packages in 277ms
Installed 2 packages in 3ms
 - pydantic-core==2.18.2
 + pydantic-core==2.18.2
 - typing-extensions==4.11.0
 + typing-extensions==4.11.0
$ tail -1 .venv/lib/python3.12/site-packages/pydantic_core-2.18.2.dist-info/WHEEL
Tag: cp312-cp312-macosx_11_0_arm64
$ .venv/bin/python -c 'import pydantic_core; print(pydantic_core.__file__)'
/private/tmp/u/.venv/lib/python3.12/site-packages/pydantic_core/__init__.py
```

---

_Comment by @charliermarsh on 2024-05-15 04:40_

Think I need to look at this with fresh eyes in the morning... I might be interested to see the output of the first `/usr/local/Cellar/uv/0.1.44/bin/uv pip install pydantic-core --force-reinstall` with the `--verbose` flag though. I'm not really sure why the `cp312-cp312-macosx_10_12_x86_64` is being considered as compatible.

---

_Comment by @charliermarsh on 2024-05-15 04:43_

The core problem AFAICT is that for some reason `cp312-cp312-macosx_10_12_x86_64` is being considered compatible with your system, which is presumedly an ARM mac?

---

_Comment by @charliermarsh on 2024-05-15 04:44_

It would be one thing if you created the venv with `/usr/local/Cellar/uv/0.1.44/bin/uv`, which seems to be an x86 Python, but you're creating the venv with `~/.cargo/bin/uv` right?

---

_Comment by @wimglenn on 2024-05-15 04:59_

Yeah, it's a MacBook Air (Apple M1). 
Right. Here's some more verbosity:

```
$ rm -rf .venv
$ ~/.cargo/bin/uv venv
Using Python 3.12.3 interpreter at: /usr/local/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ ~/.cargo/bin/uv cache clean
Clearing cache at: /Users/wim/Library/Caches/uv
Removed 29 files (10.5MiB)
$ /usr/local/Cellar/uv/0.1.44/bin/uv pip install pydantic-core --force-reinstall -vv
 uv_requirements::specification::from_source source=pydantic-core
    0.002866s DEBUG uv_interpreter::virtualenv Found a virtualenv named .venv at: /private/tmp/u/.venv
    0.003150s DEBUG uv_interpreter::interpreter Probing interpreter info for: /private/tmp/u/.venv/bin/python
    0.067560s DEBUG uv_interpreter::interpreter Found Python 3.12.3 for: /private/tmp/u/.venv/bin/python
    0.068221s DEBUG uv::commands::pip::install Using Python 3.12.3 environment at .venv/bin/python
    0.068311s DEBUG uv_fs Trying to lock if free: .venv/.lock
 uv_client::linehaul::linehaul 
    0.070875s DEBUG uv_client::base_client Using registry request timeout of 30s
 uv_resolver::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.073008s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.3
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.073901s   0ms DEBUG uv_resolver::resolver Adding direct dependency: pydantic-core*
   uv_resolver::resolver::choose_version package=pydantic-core
     uv_resolver::resolver::package_wait package_name=pydantic-core
 uv_resolver::resolver::process_request request=Versions pydantic-core
   uv_client::registry_client::simple_api package=pydantic-core
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/wim/Library/Caches/uv/simple-v7/pypi/pydantic-core.rkyv
 uv_client::cached_client::from_path_sync path="/Users/wim/Library/Caches/uv/simple-v7/pypi/pydantic-core.rkyv"
 uv_resolver::resolver::process_request request=Prefetch pydantic-core *
          0.076620s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/pydantic-core/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/pydantic-core/"
       uv_client::cached_client::new_cache file=/Users/wim/Library/Caches/uv/simple-v7/pypi/pydantic-core.rkyv
       uv_client::registry_client::parse_simple_api package=pydantic-core
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pydantic-core==2.18.2
     uv_client::registry_client::wheel_metadata built_dist=pydantic-core==2.18.2
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.msgpack
 uv_client::cached_client::from_path_sync path="/Users/wim/Library/Caches/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.msgpack"
        0.269505s 194ms DEBUG uv_resolver::resolver Searching for a compatible version of pydantic-core (*)
        0.269612s 194ms DEBUG uv_resolver::resolver Selecting: pydantic-core==2.18.2 (pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.whl)
   uv_resolver::resolver::get_dependencies package=pydantic-core, version=2.18.2
     uv_resolver::resolver::distributions_wait version_id=pydantic-core-2.18.2
              0.270407s   1ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/15/b1/e6edfe46402a5b415fc3de86aa64fb10009b323907f8d513175bfb839aa9/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/15/b1/e6edfe46402a5b415fc3de86aa64fb10009b323907f8d513175bfb839aa9/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.msgpack
           uv_client::registry_client::parse_metadata21 
        0.341417s  71ms DEBUG uv_resolver::resolver Adding transitive dependency for pydantic-core==2.18.2: typing-extensions>=4.6.0, <4.7.0 | >4.7.0
   uv_resolver::resolver::choose_version package=typing-extensions
     uv_resolver::resolver::package_wait package_name=typing-extensions
 uv_resolver::resolver::process_request request=Versions typing-extensions
   uv_client::registry_client::simple_api package=typing-extensions
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/wim/Library/Caches/uv/simple-v7/pypi/typing-extensions.rkyv
 uv_resolver::resolver::process_request request=Prefetch typing-extensions >=4.6.0, <4.7.0 | >4.7.0
 uv_client::cached_client::from_path_sync path="/Users/wim/Library/Caches/uv/simple-v7/pypi/typing-extensions.rkyv"
          0.341832s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/typing-extensions/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/typing-extensions/"
       uv_client::cached_client::new_cache file=/Users/wim/Library/Caches/uv/simple-v7/pypi/typing-extensions.rkyv
       uv_client::registry_client::parse_simple_api package=typing-extensions
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=typing-extensions==4.11.0
     uv_client::registry_client::wheel_metadata built_dist=typing-extensions==4.11.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.msgpack
        0.361175s  19ms DEBUG uv_resolver::resolver Searching for a compatible version of typing-extensions (>=4.6.0, <4.7.0 | >4.7.0)
        0.361188s  19ms DEBUG uv_resolver::resolver Selecting: typing-extensions==4.11.0 (typing_extensions-4.11.0-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/wim/Library/Caches/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=typing-extensions, version=4.11.0
     uv_resolver::resolver::distributions_wait version_id=typing-extensions-4.11.0
              0.361284s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21 
      0.379275s 306ms DEBUG uv_resolver::resolver::batch_prefetch Tried 3 versions: pydantic-core 1, root 1, typing-extensions 1
Resolved 2 packages in 308ms
    0.380907s DEBUG uv_installer::plan Identified uncached requirement: pydantic-core==2.18.2
    0.381434s DEBUG uv_installer::plan Identified uncached requirement: typing-extensions==4.11.0
 uv_installer::downloader::download total=2
   uv_installer::downloader::get_wheel name=pydantic-core==2.18.2, size=Some(1857389), url="https://files.pythonhosted.org/packages/15/b1/e6edfe46402a5b415fc3de86aa64fb10009b323907f8d513175bfb839aa9/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.whl"
     uv_distribution::distribution_database::get_or_build_wheel dist=pydantic-core==2.18.2
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.http
 uv_client::cached_client::from_path_sync path="/Users/wim/Library/Caches/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.http"
   uv_installer::downloader::get_wheel name=typing-extensions==4.11.0, size=Some(34698), url="https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl"
     uv_distribution::distribution_database::get_or_build_wheel dist=typing-extensions==4.11.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.http
 uv_client::cached_client::from_path_sync path="/Users/wim/Library/Caches/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.http"
              0.385101s   1ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/15/b1/e6edfe46402a5b415fc3de86aa64fb10009b323907f8d513175bfb839aa9/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/15/b1/e6edfe46402a5b415fc3de86aa64fb10009b323907f8d513175bfb839aa9/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.whl"
              0.385193s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl"
           uv_client::cached_client::new_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.18.2-cp312-cp312-macosx_10_12_x86_64.http
           uv_distribution::distribution_database::wheel wheel=pydantic-core==2.18.2
           uv_client::cached_client::new_cache file=/Users/wim/Library/Caches/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.http
           uv_distribution::distribution_database::wheel wheel=typing-extensions==4.11.0
Downloaded 2 packages in 251ms
 uv_installer::installer::install num_wheels=2
Installed 2 packages in 9ms
 + pydantic-core==2.18.2
 + typing-extensions==4.11.0
$ tail -1 .venv/lib/python3.12/site-packages/pydantic_core-2.18.2.dist-info/WHEEL
Tag: cp312-cp312-macosx_10_12_x86_64
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-15 12:59_

---

_Comment by @charliermarsh on 2024-05-15 14:46_

I haven't been able to reproduce this yet. I installed an x86 Python and repeated the process above. I agree that something strange is happening above. In `/usr/local/bin/python3`, can you run: `import platform; platform.mac_ver()`?

---

_Comment by @wimglenn on 2024-05-16 02:04_

```
$ /usr/local/bin/python3 -c 'import platform; print(platform.mac_ver())'
('14.4.1', ('', '', ''), 'arm64')
```

---

_Comment by @wimglenn on 2024-05-25 02:00_

I've updated macOS and it shows `('14.5', ('', '', ''), 'arm64')` now. 
I still see this issue - today it was uv pip selecting `black-24.4.2-cp312-cp312-macosx_10_9_x86_64.whl` when py pip collected `black-24.4.2-cp312-cp312-macosx_11_0_arm64.whl`.

---

_Comment by @charliermarsh on 2024-05-25 02:42_

Unfortunately I was never able to reproduce this even after installing both an x86 and an ARM Python side-by-side on my machine.

---

_Comment by @erensahins on 2024-06-27 09:19_

I am experiencing the same error regarding installing pydantic on MacOS M1 chip:

```uv pip install pydantic==2.7.4```

```
ImportError: dlopen(.../.venv/lib/python3.11/site-packages/pydantic_core/_pydantic_core.cpython-311-darwin.so, 0x0002): tried: '../.venv/lib/python3.11/site-packages/pydantic_core/_pydantic_core.cpython-311-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64')), 
'/System/Volumes/Preboot/Cryptexes/OS/Users/<username>/<projectname>/.venv/lib/python3.11/site-packages/pydantic_core/_pydantic_core.cpython-311-darwin.so' (no such file), '.../.venv/lib/python3.11/site-packages/pydantic_core/_pydantic_core.cpython-311-darwin.so' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64'))
```

However, pip directly selects the correct architecture and installs properly.

my mac platform version:

```('13.5.1', ('', '', ''), 'arm64')```

Uninstalling pydantic and cleaning uv cache does not resolve the problem.

---

_Comment by @JuR-0 on 2025-06-27 15:28_

@erensahins @wimglenn did you fix your issue? I encountered the same problem today

---

_Comment by @P-Muench on 2025-10-07 15:42_

@erensahins @wimglenn I had the same issue bugging me for a while. Turns out I had an x86_64 version of homebrew installed on my Mac (running with Rosetta 2) which must have installed an x86_64 version of uv. After reinstalling homebrew  and uv (both as arm versions) the error went away.

You can verify which version of homebrew you are running via

`brew config`


---

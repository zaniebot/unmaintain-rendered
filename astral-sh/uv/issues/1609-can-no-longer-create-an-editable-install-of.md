```yaml
number: 1609
title: "Can no longer create an editable install of `typeshed-stats` on Windows after upgrading to `uv==0.1.3`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - duplicate
  - cache
assignees: []
created_at: 2024-02-17T19:23:15Z
updated_at: 2024-02-17T20:33:28Z
url: https://github.com/astral-sh/uv/issues/1609
synced_at: 2026-01-10T05:40:31Z
```

# Can no longer create an editable install of `typeshed-stats` on Windows after upgrading to `uv==0.1.3`

---

_Issue opened by @AlexWaygood on 2024-02-17 19:23_

I was able to create an editable install of [`typeshed-stats`](https://github.com/AlexWaygood/typeshed-stats) fine on Windows locally with `uv==0.1.1`, and I'm still able to on macOS. However, after upgrading from `uv==0.1.1` to `uv==0.1.3` on Windows, I now see this (using PowerShell):

```ps
PS C:\Users\alexw\coding\typeshed-stats> uv venv env
Using Python 3.12.1 interpreter at C:\Users\alexw\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: env
PS C:\Users\alexw\coding\typeshed-stats> env\scripts\activate
(env) PS C:\Users\alexw\coding\typeshed-stats> uv pip install -e ".[dev]"
error: Failed to build editables
  Caused by: Failed to build editable: file:///C:/Users/alexw/coding/typeshed-stats
  Caused by: Failed to install requirements from build-system.requires (resolve)
  Caused by: No solution found when resolving: hatchling, hatch-vcs
  Caused by: Reading from cache archive failed: check bytes error: check failed for tuple struct member 0: check failed for slice index 0: check failed for struct member files: check failed for struct member wheels: check failed for slice index 0: check failed for struct member file: check failed for struct member requires_python: invalid tag for enum: 156
```


---

_Label `bug` added by @AlexWaygood on 2024-02-17 19:23_

---

_Label `cache` added by @AlexWaygood on 2024-02-17 19:23_

---

_Comment by @AlexWaygood on 2024-02-17 19:25_

Hmmm... probably a duplicate of #1571, I guess?

---

_Comment by @AlexWaygood on 2024-02-17 19:27_

The output if I run `uv pip install -e ".[dev]" --verbose`:

<details>

```
 uv::requirements::from_source source=-e .[dev]
    0.003862s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: C:\Users\alexw\coding\typeshed-stats\env
    0.005343s DEBUG uv_interpreter::interpreter Using cached markers for: \\?\C:\Users\alexw\coding\typeshed-stats\env\Scripts\python.exe
    0.005479s DEBUG uv::commands::pip_install Using Python 3.12.1 environment at C:\Users\alexw\coding\typeshed-stats\env\Scripts\python.exe
 uv_client::flat_index::from_entries
 uv_installer::downloader::build_editables
      0.012075s   0ms DEBUG uv_distribution::source Building (editable) file:///C:/Users/alexw/coding/typeshed-stats
   uv_dispatch::setup_build package_id="file:///C:/Users/alexw/coding/typeshed-stats", subdirectory=None
     uv_resolver::resolver::solve
          0.028772s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.1
       uv_resolver::resolver::choose_version package=root
       uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
            0.029515s   0ms DEBUG uv_resolver::resolver Adding direct dependency: hatch-vcs*
            0.029749s   0ms DEBUG uv_resolver::resolver Adding direct dependency: hatchling*
       uv_resolver::resolver::choose_version package=hatch-vcs
         uv_resolver::resolver::package_wait package_name=hatch-vcs
     uv_resolver::resolver::process_request request=Versions hatch-vcs
       uv_client::registry_client::simple_api package=hatch-vcs
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\alexw\AppData\Local\uv\cache\simple-v0\pypi\hatch-vcs.rkyv
     uv_resolver::resolver::process_request request=Versions hatchling
       uv_client::registry_client::simple_api package=hatchling
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\alexw\AppData\Local\uv\cache\simple-v0\pypi\hatchling.rkyv
     uv_resolver::resolver::process_request request=Prefetch hatchling *
     uv_resolver::resolver::process_request request=Prefetch hatch-vcs *
              0.033306s   2ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/hatch-vcs/
              0.033573s   2ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/hatch-vcs/
           uv_client::cached_client::revalidation_request url="https://pypi.org/simple/hatch-vcs/"
              0.034468s   2ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/hatchling/
              0.034637s   2ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/hatchling/
           uv_client::cached_client::revalidation_request url="https://pypi.org/simple/hatchling/"
              0.134278s 103ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/hatch-vcs/
           uv_client::cached_client::refresh_cache file=\\?\C:\Users\alexw\AppData\Local\uv\cache\simple-v0\pypi\hatch-vcs.rkyv
error: Failed to build editables
  Caused by: Failed to build editable: file:///C:/Users/alexw/coding/typeshed-stats
  Caused by: Failed to install requirements from build-system.requires (resolve)
  Caused by: No solution found when resolving: hatchling, hatch-vcs
  Caused by: Reading from cache archive failed: check bytes error: check failed for tuple struct member 0: check failed for slice index 0: check failed for struct member files: check failed for struct member wheels: check failed for slice index 0: check failed for struct member file: check failed for struct member requires_python: invalid tag for enum: 156
```

</details>

---

_Comment by @zanieb on 2024-02-17 19:27_

Yeah this looks like a cache upgrade bug, if you `uv clean` it should be fixed

---

_Comment by @AlexWaygood on 2024-02-17 19:30_

> Yeah this looks like a cache upgrade bug, if you `uv clean` it should be fixed

Yup, can confirm that fixed things!

---

_Comment by @MichaReiser on 2024-02-17 19:33_

Is the uv version not a part of the cache key?

---

_Comment by @charliermarsh on 2024-02-17 19:42_

No, the cache buckets have their own versions that we can bump. I need to look back at the history to see if we changed something in the cache representation between versions. It’s possible that I was supposed to bump a version.

---

_Comment by @AlexWaygood on 2024-02-17 19:49_

Fun fact: I only ran into this because I was trying to repro #1612 locally 😄

---

_Comment by @charliermarsh on 2024-02-17 20:26_

Ok, I believe I've confirmed locally that this was caused by https://github.com/astral-sh/uv/pull/1556. I changed one of the cached structs, and that breaks the zero-copy representation. I should've bumped the cache version.

---

_Comment by @charliermarsh on 2024-02-17 20:27_

Gonna merge with https://github.com/astral-sh/uv/issues/1571.

---

_Closed by @charliermarsh on 2024-02-17 20:27_

---

_Label `duplicate` added by @zanieb on 2024-02-17 20:33_

---

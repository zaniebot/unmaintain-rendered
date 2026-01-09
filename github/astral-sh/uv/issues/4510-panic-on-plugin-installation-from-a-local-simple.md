---
number: 4510
title: Panic on plugin installation from a local simple repository
type: issue
state: closed
author: BenediktMaag
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-06-25T07:28:35Z
updated_at: 2024-06-26T07:38:17Z
url: https://github.com/astral-sh/uv/issues/4510
synced_at: 2026-01-07T13:12:17-06:00
---

# Panic on plugin installation from a local simple repository

---

_Issue opened by @BenediktMaag on 2024-06-25 07:28_

I have setup a simple repository (https://packaging.python.org/en/latest/specifications/simple-repository-api/ | PEP503).

Installing a plugin from the repository by providing a extra-index-url works with pip
❯ : pip install --extra-index-url C:\temp\simple pyplugin
Looking in indexes: https://pypi.org/simple, c:\temp\simple
Processing c:\temp\simple\pyplugin\pyplugin-0.2.0-cp312-none-win_amd64.whl
Installing collected packages: pyplugin
Successfully installed pyplugin-0.2.0

Auditing once installed works:
❯ : uv pip install --extra-index-url C:\temp\simple pyplugin
Audited 1 package in 0.87ms

Uninstalling with uv works
❯ : uv pip uninstall pyplugin
Uninstalled 1 package in 4ms
 - pyplugin==0.2.0

Installing if not installed panics on as an error is unwrapped:
❯ : uv pip install --extra-index-url C:\temp\simple pyplugin -vv
    0.000065s DEBUG uv uv 0.2.15
 uv_requirements::specification::from_source source=pyplugin
    0.004157s DEBUG uv_toolchain::discovery Searching for Python interpreter in system toolchains
    0.005540s DEBUG uv_toolchain::discovery Found cpython 3.12.0 at `C:\temp\test_install\.venv\Scripts\python.exe` (active virtual environment)
    0.006112s DEBUG uv::commands::pip::install Using Python 3.12.0 environment at .venv\Scripts\python.exe
    0.007138s DEBUG uv_fs Acquired lock for `.venv`
    0.007840s DEBUG uv::commands::pip::install At least one requirement is not satisfied: pyplugin
 uv_client::linehaul::linehaul
    0.008540s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_resolver::resolver::solve
   uv_resolver::resolver::solve_tracked
      0.012593s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.12.0
     uv_resolver::resolver::choose_version package=root
     uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
       uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
          0.013802s   0ms DEBUG uv_resolver::resolver Adding direct dependency: pyplugin*
 uv_resolver::resolver::process_request request=Versions pyplugin
   uv_client::registry_client::simple_api package=pyplugin
thread '     uv_resolver::resolver::choose_version package=pyplugin
main' panicked at D:\a\uv\uv\crates\uv-client\src\registry_client.rs:273:14:
called `Result::unwrap()` on an `Err` value: ()

A minimal setup can be found here, github doesnt allow .7z files (maturin init compiled as 0.1.0 and 0.2.0):
https://we.tl/t-WOJyEdOD1L

❯ : uv --version
uv 0.2.15 (bfc342da9 2024-06-24)
Windows 11

---

_Comment by @charliermarsh on 2024-06-25 12:28_

I think we require that you pass it as a `file://` URL, so we can distinguish it from an HTTP registry.

---

_Comment by @BenediktMaag on 2024-06-25 12:42_

Indeed it works when prefixing the url with file:://
However you might still considers prompting a more understandable error than:

❯ : uv pip install --extra-index-url C:\temp\simple pyplugin
⠙ Resolving dependencies...
thread 'main' panicked at D:\a\uv\uv\crates\uv-client\src\registry_client.rs:273:14:
called `Result::unwrap()` on an `Err` value: ()
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-25 12:52_

---

_Label `bug` added by @zanieb on 2024-06-25 13:38_

---

_Label `compatibility` added by @charliermarsh on 2024-06-25 14:59_

---

_Comment by @charliermarsh on 2024-06-25 17:13_

It looks like pip's logic is:

```python
if os.path.exists(location):  # Is a local path.
    url = path_to_url(location)
    path = location
elif location.startswith("file:"):  # A file: URL.
    url = location
    path = url_to_path(location)
elif is_url(location):
    url = location
```

---

_Referenced in [astral-sh/uv#4524](../../astral-sh/uv/pulls/4524.md) on 2024-06-25 17:23_

---

_Closed by @charliermarsh on 2024-06-25 17:57_

---

_Closed by @charliermarsh on 2024-06-25 17:57_

---

_Comment by @charliermarsh on 2024-06-25 18:15_

Next release supports this.

---

_Referenced in [astral-sh/uv#4527](../../astral-sh/uv/pulls/4527.md) on 2024-06-25 18:25_

---

_Comment by @BenediktMaag on 2024-06-26 07:38_

Thanks alot!

---

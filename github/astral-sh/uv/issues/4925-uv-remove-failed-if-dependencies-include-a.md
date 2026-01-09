---
number: 4925
title: "`uv remove` failed if dependencies include a package from extra index"
type: issue
state: closed
author: j178
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-09T11:57:08Z
updated_at: 2024-07-09T17:46:33Z
url: https://github.com/astral-sh/uv/issues/4925
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv remove` failed if dependencies include a package from extra index

---

_Issue opened by @j178 on 2024-07-09 11:57_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

A minimal reproducer:

the `pyproject.toml`:

```toml
[project]
name = "repro"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "httpx",
]

[tool.uv]
preview = true
extra-index-url = ["https://KPFIg-AgpUCzarPJTpQLlK2UvZ36V1uA:@pypi.fury.io/j178/"]
```

```sh
$ uv add a-private-package  # this is a private package on my fury.io index
Resolved 2 packages in 2.51s
   Built repro @ file:///private/tmp/repro
Prepared 1 package in 2.40s
Uninstalled 1 package in 1ms
Installed 1 package in 2ms
 - repro==0.1.0 (from file:///private/tmp/repro)
 + repro==0.1.0 (from file:///private/tmp/repro)

$ uv remove httpx
error: Because a-private-package was not found in the package registry and repro==0.1.0 depends on a-private-package, we can conclude that repro==0.1.0 cannot be used.
And because only repro==0.1.0 is available and you require repro, we can conclude that the requirements are unsatisfiable.

$ uv --version
uv 0.2.23 (Homebrew 2024-07-08)
```

<details>
<summary>The full trace log</summary>

```sh
$ uv remove httpx -vv
    0.000039s DEBUG uv uv 0.2.23 (Homebrew 2024-07-08)
    0.000380s DEBUG uv_distribution::workspace Found project root: `/private/tmp/repro`
    0.000540s DEBUG uv_distribution::workspace No workspace root found, using project root
    0.001249s DEBUG uv::commands::project Interpreter meets the requested Python: `Python >=3.12`
 uv_client::linehaul::linehaul
    0.001436s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=repro @ file:///private/tmp/repro
    0.003162s DEBUG uv_fs Acquired lock for `/Users/didi/Library/Caches/uv/built-wheels-v3/editable/8fbfad1f309882c8`
   uv_distribution::source::build_metadata dist=repro @ file:///private/tmp/repro
      0.004019s   0ms DEBUG uv_distribution::source Preparing metadata for: repro @ file:///private/tmp/repro
      0.004071s   0ms DEBUG uv_distribution::source No static `PKG-INFO` available for: repro @ file:///private/tmp/repro (MissingPkgInfo)
      0.004340s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: repro @ file:///private/tmp/repro
    0.005068s   2ms DEBUG uv_distribution::workspace No workspace root found, using project root
 uv_resolver::resolver::solve
   uv_resolver::resolver::solve_tracked
      0.005589s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.12.3
      0.005601s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.12
     uv_resolver::resolver::choose_version package=root
     uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
       uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
      0.005835s   0ms DEBUG uv_resolver::resolver Adding direct dependency: repro*
     uv_resolver::resolver::choose_version package=repro
        0.005937s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of repro @ file:///private/tmp/repro (*)
     uv_resolver::resolver::get_dependencies_forking package=repro, version=0.1.0
       uv_resolver::resolver::get_dependencies package=repro, version=0.1.0
      0.005992s   0ms DEBUG uv_resolver::resolver Adding transitive dependency for repro==0.1.0: a-private-package*
     uv_resolver::resolver::choose_version package=a-private-package
 uv_resolver::resolver::process_request request=Versions a-private-package
   uv_client::registry_client::simple_api package=a-private-package
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/didi/Library/Caches/uv/simple-v9/pypi/a-private-package.rkyv
 uv_resolver::resolver::process_request request=Prefetch a-private-package *
 uv_client::cached_client::from_path_sync path="/Users/didi/Library/Caches/uv/simple-v9/pypi/a-private-package.rkyv"
        0.006238s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/a-private-package/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/a-private-package/"
          0.423795s 417ms DEBUG uv_auth::middleware Checking netrc for credentials for https://pypi.org/simple/a-private-package/
        0.423956s 417ms DEBUG uv_resolver::resolver Searching for a compatible version of a-private-package (*)
      0.423986s 418ms DEBUG uv_resolver::resolver No compatible version found for: a-private-package
     uv_resolver::resolver::choose_version package=repro
        0.424089s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of repro @ file:///private/tmp/repro (<0.1.0 | >0.1.0)
      0.424101s 418ms DEBUG uv_resolver::resolver No compatible version found for: repro
error: Because a-private-package was not found in the package registry and repro==0.1.0 depends on a-private-package, we can conclude that repro==0.1.0 cannot be used.
And because only repro==0.1.0 is available and you require repro, we can conclude that the requirements are unsatisfiable.
```
</details>


---

_Comment by @charliermarsh on 2024-07-09 16:17_

Thanks.

---

_Label `bug` added by @charliermarsh on 2024-07-09 16:17_

---

_Label `preview` added by @charliermarsh on 2024-07-09 16:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-09 16:19_

---

_Referenced in [astral-sh/uv#4930](../../astral-sh/uv/pulls/4930.md) on 2024-07-09 17:38_

---

_Closed by @charliermarsh on 2024-07-09 17:46_

---

_Closed by @charliermarsh on 2024-07-09 17:46_

---

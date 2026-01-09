---
number: 2104
title: uv pip installl git+ fails on arm64 + arm/v7
type: issue
state: closed
author: gregerspoulsen
labels:
  - needs-mre
assignees: []
created_at: 2024-03-01T08:01:57Z
updated_at: 2024-07-09T09:14:23Z
url: https://github.com/astral-sh/uv/issues/2104
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip installl git+ fails on arm64 + arm/v7

---

_Issue opened by @gregerspoulsen on 2024-03-01 08:01_

Running `uv pip install git+...` fails on arm64 + arm/v7 but completes on amd64.
Tested with uv version 0.1.12 and 0.1.13

Commands (jsons is just an example chosen because it doesn't require any build tools.):
```
docker run -it --rm --entrypoint=bash --platform=linux/arm64 python:3.12.2
pip install uv
uv venv
uv pip install jsons@git+https://github.com/ramonhagenaars/jsons.git
```

fails with: Caused by: object not found - no match for id (9abbf3a3bd32435ac74bc98c3554ad3c71086036);
The command completes with `--platform=linux/amd64`

And then just a huge thank you, uv is a fantastic tool, it has reduced the duration of building my container images by a factor of 4 :)

With verbosity:
 ```
root@cb678c35d842:/workdir# uv -v pip install jsons@git+https://github.com/ramonhagenaars/jsons.git
 uv::requirements::from_source source=jsons@git+https://github.com/ramonhagenaars/jsons.git
    0.171619s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /usr/local
    0.179862s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.2, skipping probing: /usr/local/bin/python
    0.181569s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at /usr/local/bin/python
    0.201506s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.252532s   7ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.265274s   4ms DEBUG uv_resolver::resolver Adding direct dependency: jsons*
   uv_resolver::resolver::choose_version package=jsons
        0.275192s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of jsons @ git+https://github.com/ramonhagenaars/jsons.git (*)
 uv_resolver::resolver::process_request request=Metadata jsons @ git+https://github.com/ramonhagenaars/jsons.git
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=jsons @ git+https://github.com/ramonhagenaars/jsons.git
 uv_git::source::fetch
      0.616576s 322ms DEBUG uv_git::source Updating git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/ramonhagenaars/jsons.git", query: None, fragment: None }`
      0.629879s 335ms DEBUG uv_git::git Attempting GitHub fast path for: https://api.github.com/repos/ramonhagenaars/jsons/commits/HEAD
      1.044262s 750ms DEBUG uv_git::git skipping gc as there's only 2 pack files
      1.049657s 755ms DEBUG uv_git::git Performing a Git fetch for: https://github.com/ramonhagenaars/jsons.git
      2.786967s   2s  DEBUG uv_git::git Attempting GitHub fast path for: https://api.github.com/repos/ramonhagenaars/jsons/commits/HEAD
      2.888567s   2s  DEBUG uv_git::git skipping gc as there's only 0 pack files
      2.889080s   2s  DEBUG uv_git::git Performing a Git fetch for: https://github.com/ramonhagenaars/jsons.git
error: Failed to download and build: jsons @ git+https://github.com/ramonhagenaars/jsons.git
  Caused by: Git operation failed
  Caused by: object not found - no match for id (9abbf3a3bd32435ac74bc98c3554ad3c71086036); class=Odb (9); code=NotFound (-3)
```


---

_Comment by @charliermarsh on 2024-04-19 20:14_

This completes without error for me based on the reproduction above.

---

_Comment by @charliermarsh on 2024-04-19 20:15_

Are you still seeing this?

---

_Label `needs-mre` added by @charliermarsh on 2024-04-19 20:15_

---

_Comment by @konstin on 2024-07-09 09:14_

Closing for missing reproduction

---

_Closed by @konstin on 2024-07-09 09:14_

---

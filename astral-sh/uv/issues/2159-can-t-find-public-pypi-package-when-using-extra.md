---
number: 2159
title: "Can't find public pypi package when using extra-index-url"
type: issue
state: closed
author: albertferras-vrf
labels:
  - duplicate
assignees: []
created_at: 2024-03-04T15:47:22Z
updated_at: 2024-03-06T08:16:40Z
url: https://github.com/astral-sh/uv/issues/2159
synced_at: 2026-01-10T01:23:13Z
---

# Can't find public pypi package when using extra-index-url

---

_Issue opened by @albertferras-vrf on 2024-03-04 15:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello, I've tried adding `uv` to our Dockerfile on my company's project but I'm facing the following issue, which is not raised when using a normal `pip install -r requirements.txt`. I have reduced the reproducibility of the issue as much as possible.

The problem appears when trying to install `sqlalchemy~=2.0.27` with a custom --extra-index-url. In my case, the extra-index-url is a private pip repository for internal stuff. I have tried with other packages such as `alembic~=1.13.1` and have the same issue.

Consider the 4 following commands:
`pip install SQLAlchemy~=2.0.27`  -> OK
`uv -v pip install SQLAlchemy~=2.0.27`  -> OK
`pip install --extra-index-url https://xxxxxxx/ SQLAlchemy~=2.0.27`  -> OK
`uv -v pip install --extra-index-url https://xxxxxxx/ SQLAlchemy~=2.0.27`  -> Fails

I could not find a way to create a dummy pypi server locally and try with it.

Verbose logs of the failing install:
```
$ uv -v pip install --extra-index-url https://xxxxxxx/ SQLAlchemy~=2.0.27
 uv::requirements::from_source source=requirements.txt
    0.000364s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/albertferras/.pyenv/versions/3.11.6/envs/venvtest
    0.000490s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.6, skipping probing: /Users/albertferras/.pyenv/versions/3.11.6/envs/venvtest/bin/python
    0.000498s DEBUG uv::commands::pip_install Using Python 3.11.6 environment at /Users/albertferras/.pyenv/versions/3.11.6/envs/venvtest/bin/python
    0.000815s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.138336s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.6
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.138382s   0ms DEBUG uv_resolver::resolver Adding direct dependency: sqlalchemy>=2.0.27, <2.1.dev0
   uv_resolver::resolver::choose_version package=sqlalchemy
     uv_resolver::resolver::package_wait package_name=sqlalchemy
 uv_resolver::resolver::process_request request=Versions sqlalchemy
   uv_client::registry_client::simple_api package=sqlalchemy
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/albertferras/Library/Caches/uv/simple-v3/eb4442371341a9d9/sqlalchemy.rkyv
 uv_resolver::resolver::process_request request=Prefetch sqlalchemy >=2.0.27, <2.1.dev0
 uv_client::cached_client::from_path_sync path="/Users/albertferras/Library/Caches/uv/simple-v3/eb4442371341a9d9/sqlalchemy.rkyv"
          0.138560s   0ms DEBUG uv_client::cached_client No cache entry for: https://xxxxxx/sqlalchemy/
       uv_client::cached_client::fresh_request url="https://xxxxxxx/sqlalchemy/"
       uv_client::cached_client::new_cache file=/Users/albertferras/Library/Caches/uv/simple-v3/eb4442371341a9d9/sqlalchemy.rkyv
       uv_client::registry_client::parse_simple_api package=sqlalchemy
         uv_client::html::parse url=https://xxxxxxxx/sqlalchemy/
   uv_resolver::version_map::from_metadata
        0.338461s 200ms DEBUG uv_resolver::resolver Searching for a compatible version of sqlalchemy (>=2.0.27, <2.1.dev0)
      0.338486s 200ms DEBUG uv_resolver::resolver No compatible version found for: sqlalchemy
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of sqlalchemy are available:
          sqlalchemy<2.0.27
          sqlalchemy>=2.1.dev0
      and you require sqlalchemy>=2.0.27,<2.1.dev0, we can conclude that the requirements are unsatisfiable.

      hint: sqlalchemy was requested with a pre-release marker (e.g., sqlalchemy>=2.0.27,<2.1.dev0), but pre-releases weren't enabled (try: `--prerelease=allow`)```




---

_Renamed from "No compatible version found for: sqlalchemy" to "Can't find package when using extra-index-url" by @albertferras-vrf on 2024-03-04 15:50_

---

_Renamed from "Can't find package when using extra-index-url" to "Can't find public pypi package when using extra-index-url" by @albertferras-vrf on 2024-03-04 15:52_

---

_Comment by @zanieb on 2024-03-04 17:03_

Hi! This looks like a duplicate of 

- https://github.com/astral-sh/uv/issues/1377
- https://github.com/astral-sh/uv/issues/1600
- https://github.com/astral-sh/uv/issues/2011


---

_Closed by @zanieb on 2024-03-04 17:03_

---

_Label `duplicate` added by @zanieb on 2024-03-04 17:03_

---

_Comment by @albertferras-vrf on 2024-03-05 08:33_

@zanieb is it supposed to be fixed? I see the first ticket is linked to this PR https://github.com/astral-sh/uv/pull/2083 which was released on 0.1.13 but the issue is still happening both on 0.1.13 and 0.1.14 (released tonight), which contain that fix already.

---

_Comment by @zanieb on 2024-03-05 15:16_

I believe the description of https://github.com/astral-sh/uv/pull/2083 should clarify things — we will not discover package versions across multiple indexes so if any version of a package is present in your extra index URL then it will not be read from the public index.

---

_Referenced in [astral-sh/uv#2205](../../astral-sh/uv/issues/2205.md) on 2024-03-05 16:42_

---

_Comment by @albertferras-vrf on 2024-03-06 08:16_

I've digged a little bit into this because I was not sure it was the same issue, because I knew sqlalchemy did not exist in our private repository.
When I tried with another pypi server --extra-index-url `https://pypi.ngc.nvidia.com` (this doesn't have sqlalchemy listed) as extra-index-url it worked well:

It might be related to how our private pypi server works:
private repo: `https://xxxxx/sqlalchemy/` returns a 200 but there is no content. (`pip index` using only this server 
```
# pip seems to be ok with that
pip index versions --index-url https://xxxx/ SQLalchemy
WARNING: pip index is currently an experimental command. It may be removed/changed in a future release without prior warning.
ERROR: No matching distribution found for SQLalchemy
```

official repo: `https://pypi.org/simple/sqlalchemynothing/` returns 404


```
# verbose logs of the part where uv tries to fetch the package from private repo
       uv_client::cached_client::fresh_request url="https://xxxxx/sqlalchemy/"
       uv_client::cached_client::new_cache file=/private/var/folders/8f/_t_852217390v_y07psqp9wc0000gp/T/.tmptsy4Zh/simple-v3/eb4442371341a9d9/sqlalchemy.rkyv
       uv_client::registry_client::parse_simple_api package=sqlalchemy
         uv_client::html::parse url=https://xxxx/sqlalchemy/
   uv_resolver::version_map::from_metadata
        0.400696s 234ms DEBUG uv_resolver::resolver Searching for a compatible version of sqlalchemy (>=2.0.27, <2.1.dev0)
      0.400709s 234ms DEBUG uv_resolver::resolver No compatible version found for: sqlalchemy

# how it fetches from nvidia first, then fallback to pypi later
       uv_client::cached_client::fresh_request url="https://pypi.ngc.nvidia.com/sqlalchemy/"
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/private/var/folders/8f/_t_852217390v_y07psqp9wc0000gp/T/.tmpMLKG4D/simple-v3/pypi/sqlalchemy.rkyv
 uv_client::cached_client::from_path_sync path="/private/var/folders/8f/_t_852217390v_y07psqp9wc0000gp/T/.tmpMLKG4D/simple-v3/pypi/sqlalchemy.rkyv"
          0.939307s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/sqlalchemy/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/sqlalchemy/"
       uv_client::cached_client::new_cache file=/private/var/folders/8f/_t_852217390v_y07psqp9wc0000gp/T/.tmpMLKG4D/simple-v3/pypi/sqlalchemy.rkyv
       uv_client::registry_client::parse_simple_api package=sqlalchemy
```

I guess the issue is on our private pypi server and we should return a 404 even though there is no package version available

---

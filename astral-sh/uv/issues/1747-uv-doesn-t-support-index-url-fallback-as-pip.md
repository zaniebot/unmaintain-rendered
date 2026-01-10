---
number: 1747
title: "Uv doesn't support index-url fallback as pip"
type: issue
state: closed
author: kkpattern
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2024-02-20T11:40:20Z
updated_at: 2024-02-22T05:00:56Z
url: https://github.com/astral-sh/uv/issues/1747
synced_at: 2026-01-10T01:23:08Z
---

# Uv doesn't support index-url fallback as pip

---

_Issue opened by @kkpattern on 2024-02-20 11:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Thanks for this awesome tool.

In our project, we have a [`pypiserver`](https://pypi.org/project/pypiserver/) set up to host our internal packages. For the public packages, `pypiserver` will fallback to pypi.org with a `303` http response. For example, when we install `wheel`, this is the log from `pip install`:

```bash
Looking in indexes: http://our.internal.pypiserver/simple/
1 location(s) to search for versions of wheel:
* http://our.internal.pypiserver/simple/wheel/
Fetching project page and analyzing links: http://our.internal.pypiserver/simple/wheel/
Getting page http://our.internal.pypiserver/simple/wheel/
Found index url http://our.internal.pypiserver/simple/
Looking up "http://our.internal.pypiserver/simple/wheel/" in the cache
Request header has "max_age" as 0, cache bypassed
Starting new HTTP connection (1): our.internal.pypiserver:80
http://our.internal.pypiserver:80 "GET /simple/wheel/ HTTP/1.1" 303 0
Status code 303 not in (200, 203, 300, 301, 308)
Starting new HTTP connection (1): pypi.python.org:80
```

When we use `UV_INDEX_URL` with uv, uv will fail to install the public packages:

```bash
error: Failed to download: wheel==0.42.0
  Caused by: HTTP status client error (404 Not Found) for url (http://our.internal.pypiserver/simple/wheel/wheel-0.42.0-py3-none-any.whl)
```

The difference seems to be that `pip` will first look at `/simple/wheel/` and `uv` will look at `simple/wheel/wheel-0.42.0-py3-none-any.whl` directly. Maybe `pypiserver` should return `303` in the case too?


---

_Comment by @charliermarsh on 2024-02-20 14:56_

Just to double-check, do you run this with `--index-url` or `--extra-index-url`? (We might not be following the rediret or something.)

---

_Label `bug` added by @charliermarsh on 2024-02-20 14:56_

---

_Label `needs-mre` added by @charliermarsh on 2024-02-20 14:56_

---

_Comment by @kkpattern on 2024-02-20 14:57_

I ran this with `UV_INDEX_URL`, no `--index-url` or `--extra-index-url`.

---

_Comment by @charliermarsh on 2024-02-20 15:02_

Thanks!

---

_Comment by @charliermarsh on 2024-02-20 19:02_

Unfortunately this "just worked" for me when testing locally.

---

_Comment by @charliermarsh on 2024-02-20 19:05_

Is there anything you can share about how you've configured `pypiserver`?

---

_Comment by @charliermarsh on 2024-02-20 19:05_

E.g., this works without error: `echo "mypy" | uv pip compile - --index-url http://localhost:8080/simple --verbose --no-cache`

---

_Comment by @kkpattern on 2024-02-21 02:13_

I found it's related to the fallback url configuration of `pypiserver`, we put a cache proxy before pypi.org.

I can reproduce this locally with the following instructions:

```bash
# first setup a proxpi cache proxy
pip install proxpi gunicorn
gunicorn proxpi.server:app --bind 0.0.0.0:8000

# then setup a pypiserver
pypi-server run -p 8080 /tmp/packages --fallback-url "http://localhost:8000/index"
```

Then uv will fail to install the packages through the `pypiserver`:

```bash
(.venv) ☁  uv  echo "wheel" | uv pip compile - --index-url http://localhost:8080/simple --verbose --no-cache
 uv::requirements::from_source source=-
    0.001565s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /home/user/test/uv/.venv
    0.001743s DEBUG uv_interpreter::interpreter Detecting markers for: /home/user/test/uv/.venv/bin/python
    0.032706s DEBUG uv::commands::pip_compile Using Python 3.11.3 interpreter at /home/user/test/uv/.venv/bin/python for builds
    0.033791s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.036118s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.3
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.036313s   0ms DEBUG uv_resolver::resolver Adding direct dependency: wheel*
   uv_resolver::resolver::choose_version package=wheel
     uv_resolver::resolver::package_wait package_name=wheel
 uv_resolver::resolver::process_request request=Versions wheel
   uv_client::registry_client::simple_api package=wheel
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpV8O0o8/simple-v1/c043adf7183e3e19/wheel.rkyv
 uv_resolver::resolver::process_request request=Prefetch wheel *
          0.037140s   0ms DEBUG uv_client::cached_client No cache entry for: http://localhost:8080/simple/wheel/
       uv_client::cached_client::fresh_request url="http://localhost:8080/simple/wheel/"
       uv_client::cached_client::new_cache file=/tmp/.tmpV8O0o8/simple-v1/c043adf7183e3e19/wheel.rkyv
       uv_client::registry_client::parse_simple_api package=wheel
 uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=wheel==0.42.0
     uv_client::registry_client::wheel_metadata built_dist=wheel==0.42.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpV8O0o8/wheels-v0/index/c043adf7183e3e19/wheel/wheel-0.42.0-py3-none-any.msgpack
        0.118153s  81ms DEBUG uv_resolver::resolver Searching for a compatible version of wheel (*)
        0.118176s  81ms DEBUG uv_resolver::resolver Selecting: wheel==0.42.0 (wheel-0.42.0-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=wheel, version=0.42.0
     uv_resolver::resolver::distributions_wait package_id=wheel-0.42.0
              0.118255s   0ms DEBUG uv_client::cached_client No cache entry for: http://localhost:8080/simple/wheel/wheel-0.42.0-py3-none-any.whl
           uv_client::cached_client::fresh_request url="http://localhost:8080/simple/wheel/wheel-0.42.0-py3-none-any.whl"
error: Failed to download: wheel==0.42.0
  Caused by: HTTP status client error (404 Not Found) for url (http://localhost:8080/simple/wheel/wheel-0.42.0-py3-none-any.whl)
```

Pip can install the package successfully in this case:

```bash
pip install --index-url http://localhost:8080/simple wheel -vv 
Using pip 22.3.1 from /home/user/test/uv/.venv/lib/python3.11/site-packages/pip (python 3.11)
Non-user install because user site-packages disabled
Created temporary directory: /tmp/pip-build-tracker-y2f89g7m
Initialized build tracking at /tmp/pip-build-tracker-y2f89g7m
Created build tracker: /tmp/pip-build-tracker-y2f89g7m
Entered build tracker: /tmp/pip-build-tracker-y2f89g7m
Created temporary directory: /tmp/pip-install-11ixfkuq
Created temporary directory: /tmp/pip-ephem-wheel-cache-a9_gzlyv
Looking in indexes: http://localhost:8080/simple
1 location(s) to search for versions of wheel:
* http://localhost:8080/simple/wheel/
Fetching project page and analyzing links: http://localhost:8080/simple/wheel/
Getting page http://localhost:8080/simple/wheel/
Found index url http://localhost:8080/simple
Starting new HTTP connection (1): localhost:8080
http://localhost:8080 "GET /simple/wheel/ HTTP/1.1" 303 0
Starting new HTTP connection (1): localhost:8000
http://localhost:8000 "GET /index/wheel/ HTTP/1.1" 200 6758
Fetched page http://localhost:8080/simple/wheel/ as application/vnd.pypi.simple.v1+json
  ...
  Found link http://localhost:8000/index/wheel/wheel-0.42.0-py3-none-any.whl (from http://localhost:8000/index/wheel/) (requires-python:>=3.7), version: 0.42.0
  Found link http://localhost:8000/index/wheel/wheel-0.42.0.tar.gz (from http://localhost:8000/index/wheel/) (requires-python:>=3.7), version: 0.42.0
Skipping link: not a file: http://localhost:8080/simple/wheel/
Given no hashes to check 130 links for project 'wheel': discarding no candidates
Collecting wheel
  Created temporary directory: /tmp/pip-unpack-p982xpms
  Resetting dropped connection: localhost
  http://localhost:8000 "GET /index/wheel/wheel-0.42.0-py3-none-any.whl HTTP/1.1" 200 65375
  Downloading http://localhost:8000/index/wheel/wheel-0.42.0-py3-none-any.whl (65 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 65.4/65.4 kB 201.8 MB/s eta 0:00:00
  Added wheel from http://localhost:8000/index/wheel/wheel-0.42.0-py3-none-any.whl to build tracker '/tmp/pip-build-tracker-y2f89g7m'
  Removed wheel from http://localhost:8000/index/wheel/wheel-0.42.0-py3-none-any.whl from build tracker '/tmp/pip-build-tracker-y2f89g7m'
Created temporary directory: /tmp/pip-unpack-givexmn1
Installing collected packages: wheel

  changing mode of /home/user/test/uv/.venv/bin/wheel to 755
Successfully installed wheel-0.42.0
Remote version of pip: 24.0
Local version of pip:  22.3.1
Was pip installed by pip? True
Removed build tracker: '/tmp/pip-build-tracker-y2f89g7m'
```

Interestingly, if I set the fallback url to pypi.org:

```bash
pypi-server run -p 8080 /tmp/packages --fallback-url "https://pypi.python.org/simple"
```

Then uv will install the package successfully:

```bash
(.venv) ☁  uv  echo "wheel" | uv pip compile - --index-url http://localhost:8080/simple --verbose --no-cache
 uv::requirements::from_source source=-
    0.001598s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /home/user/test/uv/.venv
    0.001756s DEBUG uv_interpreter::interpreter Detecting markers for: /home/user/test/uv/.venv/bin/python
    0.045794s DEBUG uv::commands::pip_compile Using Python 3.11.3 interpreter at /home/user/test/uv/.venv/bin/python for builds
    0.046657s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.048157s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.3
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.048241s   0ms DEBUG uv_resolver::resolver Adding direct dependency: wheel*
   uv_resolver::resolver::choose_version package=wheel
     uv_resolver::resolver::package_wait package_name=wheel
 uv_resolver::resolver::process_request request=Versions wheel
   uv_client::registry_client::simple_api package=wheel
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpKlb0dx/simple-v1/c043adf7183e3e19/wheel.rkyv
 uv_resolver::resolver::process_request request=Prefetch wheel *
          0.048914s   0ms DEBUG uv_client::cached_client No cache entry for: http://localhost:8080/simple/wheel/
       uv_client::cached_client::fresh_request url="http://localhost:8080/simple/wheel/"
       uv_client::cached_client::new_cache file=/tmp/.tmpKlb0dx/simple-v1/c043adf7183e3e19/wheel.rkyv
       uv_client::registry_client::parse_simple_api package=wheel
 uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=wheel==0.42.0
     uv_client::registry_client::wheel_metadata built_dist=wheel==0.42.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpKlb0dx/wheels-v0/index/c043adf7183e3e19/wheel/wheel-0.42.0-py3-none-any.msgpack
        0.841446s 793ms DEBUG uv_resolver::resolver Searching for a compatible version of wheel (*)
        0.841462s 793ms DEBUG uv_resolver::resolver Selecting: wheel==0.42.0 (wheel-0.42.0-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=wheel, version=0.42.0
     uv_resolver::resolver::distributions_wait package_id=wheel-0.42.0
              0.841534s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/c7/c3/55076fc728723ef927521abaa1955213d094933dc36d4a2008d5101e1af5/wheel-0.42.0-py3-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/c7/c3/55076fc728723ef927521abaa1955213d094933dc36d4a2008d5101e1af5/wheel-0.42.0-py3-none-any.whl"
           uv_client::cached_client::new_cache file=/tmp/.tmpKlb0dx/wheels-v0/index/c043adf7183e3e19/wheel/wheel-0.42.0-py3-none-any.msgpack
           uv_client::registry_client::read_metadata_range_request wheel=wheel-0.42.0-py3-none-any.whl
Resolved 1 package in 1.10s
# This file was autogenerated by uv via the following command:
#    uv pip compile - --index-url http://localhost:8080/simple --verbose --no-cache
wheel==0.42.0
```

---

_Comment by @charliermarsh on 2024-02-21 05:41_

Thanks, I'll give this another try tomorrow.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-21 05:42_

---

_Comment by @charliermarsh on 2024-02-21 14:50_

Thanks, reproduced.

---

_Referenced in [astral-sh/uv#1816](../../astral-sh/uv/pulls/1816.md) on 2024-02-21 15:01_

---

_Closed by @charliermarsh on 2024-02-21 15:10_

---

_Comment by @charliermarsh on 2024-02-21 15:11_

Should work in the next release!

---

_Comment by @kkpattern on 2024-02-21 15:37_

Thanks!

---

_Comment by @charliermarsh on 2024-02-22 05:00_

Should be fixed in v0.1.7 (out now).

---

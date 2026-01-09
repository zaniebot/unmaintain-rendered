---
number: 5699
title: "--no-binary in conjunction with a custom uv index url results in a failure to build."
type: issue
state: closed
author: harmstyler
labels:
  - bug
assignees: []
created_at: 2024-08-01T18:01:35Z
updated_at: 2024-08-06T18:14:13Z
url: https://github.com/astral-sh/uv/issues/5699
synced_at: 2026-01-07T13:12:17-06:00
---

# --no-binary in conjunction with a custom uv index url results in a failure to build.

---

_Issue opened by @harmstyler on 2024-08-01 18:01_

When I attempt to use `--no-binary` in conjunction  with a custom uv index url via the `UV_INDEX_URL` environment variable, then uv still tries to grab the whl files but then errors because it can't use them. The .tar.gz build files are available, as that was my first suspission. The normal `pip install --no-binary` works with this setup, also.

I'm using `uv` on a docker container - Ubuntu 20.04
We're using the system python - Python 3.8.10

uv version:  0.2.32

The command I ran:
```
uv pip -vvv install --no-cache --no-binary :all: -r requirements.txt
```

The output:
```
 uv_requirements::specification::from_source source=requirements.txt
    0.008109s DEBUG uv_python::discovery Searching for Python interpreter in system path
    0.056792s DEBUG uv_python::discovery Found `cpython-3.8.10-linux-x86_64-gnu` at `/tmp/.venv/bin/python3` (virtual environment)
    0.056859s DEBUG uv::commands::pip::install Using Python 3.8.10 environment at .venv/bin/python3
    0.056901s DEBUG uv_fs Acquired lock for `.venv`
    0.056958s DEBUG uv::commands::pip::install At least one requirement is not satisfied: paho-mqtt==1.3.1
 uv_client::linehaul::linehaul
    0.057713s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_resolver::resolver::solve
    0.060195s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.8.10
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.060320s   0ms DEBUG uv_resolver::resolver Adding direct dependency: oic==1.7.0
    0.060357s   0ms DEBUG uv_resolver::resolver Adding direct dependency: paho-mqtt==1.3.1
   uv_resolver::resolver::choose_version package=oic
 uv_resolver::resolver::process_request request=Versions oic
   uv_client::registry_client::simple_api package=oic
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpKji4il/simple-v10/index/ab2a7f31a81931c8/oic.rkyv
 uv_resolver::resolver::process_request request=Versions paho-mqtt
   uv_client::registry_client::simple_api package=paho-mqtt
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpKji4il/simple-v10/index/ab2a7f31a81931c8/paho-mqtt.rkyv
 uv_client::cached_client::from_path_sync path="/tmp/.tmpKji4il/simple-v10/index/ab2a7f31a81931c8/oic.rkyv"
 uv_client::cached_client::from_path_sync path="/tmp/.tmpKji4il/simple-v10/index/ab2a7f31a81931c8/paho-mqtt.rkyv"
 uv_resolver::resolver::process_request request=Prefetch paho-mqtt ==1.3.1
 uv_resolver::resolver::process_request request=Prefetch oic ==1.7.0
        0.061368s   0ms DEBUG uv_client::cached_client No cache entry for: https://pkgs.dev.azure.com/acme/project/_packaging/acme_feed/pypi/simple/oic/
       uv_client::cached_client::fresh_request url="https://pkgs.dev.azure.com/acme/project/_packaging/acme_feed/pypi/simple/oic/"
        0.061507s   0ms DEBUG uv_client::cached_client No cache entry for: https://pkgs.dev.azure.com/acme/project/_packaging/acme_feed/pypi/simple/paho-mqtt/
       uv_client::cached_client::fresh_request url="https://pkgs.dev.azure.com/acme/project/_packaging/acme_feed/pypi/simple/paho-mqtt/"
       uv_client::cached_client::new_cache file=/tmp/.tmpKji4il/simple-v10/index/ab2a7f31a81931c8/oic.rkyv
       uv_client::registry_client::parse_simple_api package=oic
         uv_client::html::parse url=https://pkgs.dev.azure.com/acme/project/_packaging/acme_feed/pypi/simple/oic/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=oic==1.7.0
     uv_client::registry_client::wheel_metadata built_dist=oic==1.7.0
      0.381445s 321ms DEBUG uv_resolver::resolver Searching for a compatible version of oic (==1.7.0)
       uv_client::cached_client::get_serde
      0.381527s 321ms DEBUG uv_resolver::resolver Selecting: oic==1.7.0 [compatible] (oic-1.7.0.tar.gz)
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpKji4il/wheels-v1/index/ab2a7f31a81931c8/oic/oic-1.7.0-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/tmp/.tmpKji4il/wheels-v1/index/ab2a7f31a81931c8/oic/oic-1.7.0-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies_forking package=oic, version=1.7.0
     uv_resolver::resolver::get_dependencies package=oic, version=1.7.0
            0.381986s   0ms DEBUG uv_client::cached_client No cache entry for: https://pkgs.dev.azure.com/acme/<key>/_packaging/<pkg_key>/pypi/download/oic/1.7/oic-1.7.0-py3-none-any.whl#sha256=b74bd06c7de1ab4f8e798f714062e6a68f68ad9cdbed1f1c30a7fb887602f321
           uv_client::cached_client::fresh_request url="https://pkgs.dev.azure.com/acme/<key>/_packaging/<pkg_key>/pypi/download/oic/1.7/oic-1.7.0-py3-none-any.whl#sha256=b74bd06c7de1ab4f8e798f714062e6a68f68ad9cdbed1f1c30a7fb887602f321"
      0.445964s  64ms WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for oic==1.7.0; downloading wheel directly (HTTP status client error (405 Method Not Allowed) for url (https://pkgs.dev.azure.com/acme/<key>/_packaging/<pkg_key>/pypi/download/oic/1.7/oic-1.7.0-py3-none-any.whl#sha256=b74bd06c7de1ab4f8e798f714062e6a68f68ad9cdbed1f1c30a7fb887602f321))
error: Failed to download `oic==1.7.0`
  Caused by: Using pre-built wheels is disabled
```


---

_Comment by @zanieb on 2024-08-01 18:05_

Thanks for the report. This is weird... I suspect this is a regression? I'm not sure why we select the source distribution then try to download the wheel.

This error is thrown at 

https://github.com/astral-sh/uv/blob/41c1fc0c4dd268fcd5caf5551b2e7be334865716/crates/uv-distribution/src/distribution_database.rs#L153

which we are calling from:

https://github.com/astral-sh/uv/blob/41c1fc0c4dd268fcd5caf5551b2e7be334865716/crates/uv-distribution/src/distribution_database.rs#L130-L136



---

_Comment by @zanieb on 2024-08-01 18:10_

Does this error occur if you do `uv cache clean oic` first? We _do_ allow reading metadata from cached wheels with `--no-binary` — maybe we're trying to refresh the cache?

---

_Comment by @charliermarsh on 2024-08-01 18:22_

I can take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-01 18:42_

---

_Comment by @charliermarsh on 2024-08-01 18:50_

It's possible that in `--no-binary` mode, we still use wheels to fetch _metadata_ (even though we don't download and install them). My guess is that's what's happening here, and we're then hitting the fallback (downloading the wheel) since your registry doesn't support range requests.

---

_Label `bug` added by @charliermarsh on 2024-08-01 18:50_

---

_Comment by @charliermarsh on 2024-08-01 18:54_

@harmstyler -- Just curious, what's the motivation for using `--no-binary`? Trying to make a decision on how to approach this change, just trying to learn more about the use-cases.

---

_Comment by @harmstyler on 2024-08-01 19:12_

> @harmstyler -- Just curious, what's the motivation for using `--no-binary`? Trying to make a decision on how to approach this change, just trying to learn more about the use-cases.

Our company wants to build the whl files for security reasons. We used to use the `no-manylinux` package in the past. That package outlines the _why_ pretty well https://pypi.org/project/no-manylinux/

---

_Referenced in [astral-sh/uv#5707](../../astral-sh/uv/pulls/5707.md) on 2024-08-01 19:28_

---

_Closed by @charliermarsh on 2024-08-06 18:14_

---

_Closed by @charliermarsh on 2024-08-06 18:14_

---

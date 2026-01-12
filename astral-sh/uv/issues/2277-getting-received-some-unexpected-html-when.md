```yaml
number: 2277
title: "Getting \"Received some unexpected HTML when working with\" error installing from extra-index"
type: issue
state: closed
author: charliermarsh
labels:
  - registry
  - needs-mre
assignees: []
created_at: 2024-03-07T14:12:59Z
updated_at: 2024-03-08T20:31:29Z
url: https://github.com/astral-sh/uv/issues/2277
synced_at: 2026-01-12T15:58:36Z
```

# Getting "Received some unexpected HTML when working with" error installing from extra-index

---

_@charliermarsh_

Originally reported by @korneevm.

---

Hi!

I'm using `uv 0.1.15 (a8ac7b1eb 2024-03-05)` от Mac (intel). I've set env variable `UV_EXTRA_INDEX_URL="https://readonly:secret@pypi.localrepo.tech/pypi/"` ([PyPICloud](https://pypicloud.readthedocs.io/en/latest/)).

When I do `uv --verbose pip install internal-library==1.1.0` to install my package, I'm getting an error:

```
uv::requirements::from_source source=internal-library==1.1.0
    0.001615s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/korneevm/projects/some_project/env
    0.001929s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.6, skipping probing: /Users/korneevm/projects/some_project/env/bin/python
    0.001946s DEBUG uv::commands::pip_install Using Python 3.11.6 environment at /Users/korneevm/projects/some_project/env/bin/python
    0.005726s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.328674s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.6
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.328779s   0ms DEBUG uv_resolver::resolver Adding direct dependency: internal-library==1.1.0
   uv_resolver::resolver::choose_version package=internal-library
     uv_resolver::resolver::package_wait package_name=internal-library
 uv_resolver::resolver::process_request request=Versions internal-library
   uv_client::registry_client::simple_api package=internal-library
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/internal-library.rkyv
 uv_resolver::resolver::process_request request=Prefetch internal-library ==1.1.0
 uv_client::cached_client::from_path_sync path="/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/internal-library.rkyv"
          0.329441s   0ms DEBUG uv_client::cached_client Found stale response for: https://pypi.localrepo.tech/pypi/internal-library/
          0.329451s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.localrepo.tech/pypi/internal-library/
       uv_client::cached_client::revalidation_request url="https://pypi.localrepo.tech/pypi/internal-library/"
          1.624882s   1s  DEBUG uv_client::cached_client Found modified response for: https://pypi.localrepo.tech/pypi/internal-library/
       uv_client::cached_client::new_cache file=/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/internal-library.rkyv
       uv_client::registry_client::parse_simple_api package=internal-library
         uv_client::html::parse url=https://readonly:secret@pypi.localrepo.tech/pypi/internal-library/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=internal-library==1.1.0
     uv_client::registry_client::wheel_metadata built_dist=internal-library==1.1.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/korneevm/Library/Caches/uv/wheels-v0/index/f8028b1c869304e5/internal-library/internal_library-1.1.0-py3-none-any.msgpack
        1.628458s   1s  DEBUG uv_resolver::resolver Searching for a compatible version of internal-library (==1.1.0)
 uv_client::cached_client::from_path_sync path="/Users/korneevm/Library/Caches/uv/wheels-v0/index/f8028b1c869304e5/internal-library/internal_library-1.1.0-py3-none-any.msgpack"
        1.628501s   1s  DEBUG uv_resolver::resolver Selecting: internal-library==1.1.0 (internal_library-1.1.0-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=internal-library, version=1.1.0
     uv_resolver::resolver::distributions_wait package_id=internal-library-1.1.0
              1.628580s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.localrepo.tech/api/package/internal-library/internal_library-1.1.0-py3-none-any.whl#sha256=c34eb6ca75eca3956b00a03c7d17ea871636e850266db9176b597af32f95a095
           uv_client::cached_client::fresh_request url="https://pypi.localrepo.tech/api/package/internal-library/internal_library-1.1.0-py3-none-any.whl#sha256=c34eb6ca75eca3956b00a03c7d17ea871636e850266db9176b597af32f95a095"
           uv_client::cached_client::new_cache file=/Users/korneevm/Library/Caches/uv/wheels-v0/index/f8028b1c869304e5/internal-library/internal_library-1.1.0-py3-none-any.msgpack
           uv_client::registry_client::read_metadata_range_request wheel=internal_library-1.1.0-py3-none-any.whl
          1.939138s 310ms  WARN uv_client::registry_client Range requests not supported for internal_library-1.1.0-py3-none-any.whl; downloading wheel
        2.560580s 932ms DEBUG uv_resolver::resolver Adding transitive dependency: structlog>=22.1.0
        2.560611s 932ms DEBUG uv_resolver::resolver Adding transitive dependency: orjson>=3.7.8
        2.560620s 932ms DEBUG uv_resolver::resolver Adding transitive dependency: structlog-sentry>=2.0.0
   uv_resolver::resolver::choose_version package=structlog
     uv_resolver::resolver::package_wait package_name=structlog
 uv_resolver::resolver::process_request request=Versions structlog
   uv_client::registry_client::simple_api package=structlog
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/structlog.rkyv
 uv_resolver::resolver::process_request request=Versions orjson
   uv_client::registry_client::simple_api package=orjson
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/orjson.rkyv
 uv_client::cached_client::from_path_sync path="/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/structlog.rkyv"
 uv_resolver::resolver::process_request request=Versions structlog-sentry
   uv_client::registry_client::simple_api package=structlog-sentry
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/structlog-sentry.rkyv
 uv_resolver::resolver::process_request request=Prefetch structlog-sentry >=2.0.0
 uv_resolver::resolver::process_request request=Prefetch orjson >=3.7.8
 uv_resolver::resolver::process_request request=Prefetch structlog >=22.1.0
          2.561054s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.localrepo.tech/pypi/structlog/
       uv_client::cached_client::fresh_request url="https://pypi.localrepo.tech/pypi/structlog/"
 uv_client::cached_client::from_path_sync path="/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/orjson.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/structlog-sentry.rkyv"
          2.561158s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.localrepo.tech/pypi/orjson/
       uv_client::cached_client::fresh_request url="https://pypi.localrepo.tech/pypi/orjson/"
          2.561229s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.localrepo.tech/pypi/structlog-sentry/
       uv_client::cached_client::fresh_request url="https://pypi.localrepo.tech/pypi/structlog-sentry/"
       uv_client::cached_client::new_cache file=/Users/korneevm/Library/Caches/uv/simple-v3/f8028b1c869304e5/structlog.rkyv
       uv_client::registry_client::parse_simple_api package=structlog
         uv_client::html::parse url=https://pypi.org/project/structlog/
error: Received some unexpected HTML from https://pypi.org/project/structlog/
  Caused by: Unexpected fragment (expected `#sha256=...`) on URL: content
```

While `pip install internal-library==1.1.0` works fine.


---

_Comment by @charliermarsh on 2024-03-07 14:14_

@korneevm -- Heads up I recreated your issue here as your initial post may have included some sensitive credentials in the verbose logging.

---

_Label `registry` added by @charliermarsh on 2024-03-07 14:14_

---

_Comment by @charliermarsh on 2024-03-07 14:15_

Do you have an `--index-url` or `UV_INDEX_URL` set?

---

_Comment by @charliermarsh on 2024-03-07 14:20_

For some reason at the end you're looking in https://pypi.org/project/structlog/ instead of https://pypi.org/simple/structlog/, so I'm wondering if the index URL was overridden with the wrong value.

---

_Label `needs-mre` added by @charliermarsh on 2024-03-07 14:20_

---

_Comment by @korneevm on 2024-03-08 02:08_

@charliermarsh thanks! I didn't override `index-url` in any way, just added `UV_EXTRA_INDEX_URL` to the envs with the same value as `PIP_EXTRA_INDEX_URL`.

---

_Comment by @charliermarsh on 2024-03-08 02:27_

As far as I can tell from the logs, it seems like your index is redirecting from `https://pypi.localrepo.tech/pypi/structlog` to `https://pypi.org/project/structlog`?

---

_Comment by @korneevm on 2024-03-08 02:28_

I've checked it again - when I do `pip install any-package --verbose` I get `Looking in indexes: https://pypi.org/simple, https://readonly:****@pypi.localrepo.tech/pypi/`

---

_Comment by @charliermarsh on 2024-03-08 05:59_

Is it possible that your index is redirecting when packages aren't present? What happens if you navigate to https://readonly:secret@pypi.localrepo.tech/pypi/structlog in your browser or with cURL?

---

_Comment by @korneevm on 2024-03-08 08:54_

@charliermarsh I think I've found a way to reproduce this issue:

1. We use the latest https://hub.docker.com/r/stevearc/pypicloud/tags with default config
2. If I set `UV_EXTRA_INDEX_URL` to https://readonly:****@pypi.localrepo.tech/pypi/ install fails with the error above
3. If I set it to https://readonly:****@pypi.localrepo.tech/simple/ it works like a charm

`uv` here definitely behaves differently from pip/pip-tools but I'm not sure how many people will have the same problem, I'll fix pip URLs in my repos and call it a day :-)

Thanks a lot for your help, I definitely wouldn't be able to figure this out alone.

---

_Comment by @charliermarsh on 2024-03-08 20:31_

Oh interesting! That's good to know. Perhaps your index does some redirecting and preserves the `pypi`. My guess is that when `pip` tries `https://pypi.localrepo.tech/pypi/structlog` (which is invalid), it then moves on to checking PyPI, whereas we stop at the first "valid" index.

---

_Closed by @charliermarsh on 2024-03-08 20:31_

---

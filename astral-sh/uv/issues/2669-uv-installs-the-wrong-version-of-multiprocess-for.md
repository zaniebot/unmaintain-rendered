---
number: 2669
title: "`uv` installs the wrong version of `multiprocess` for Python 3.11+"
type: issue
state: closed
author: ma-sadeghi
labels:
  - needs-mre
assignees: []
created_at: 2024-03-26T11:05:09Z
updated_at: 2024-03-26T14:38:15Z
url: https://github.com/astral-sh/uv/issues/2669
synced_at: 2026-01-10T01:23:20Z
---

# `uv` installs the wrong version of `multiprocess` for Python 3.11+

---

_Issue opened by @ma-sadeghi on 2024-03-26 11:05_

```
# Create a fresh virtualenv with Python 3.11+
conda create -n test python=3.11
conda activate test
uv pip install multiprocess
```

Now, inspect `site-packages/multiprocess/util.py` under `spawnv_passfds` function:

```python
        return _posixsubprocess.fork_exec(
            args, [os.fsencode(path)], True, passfds, None, None,
            -1, -1, -1, -1, -1, -1, errpipe_read, errpipe_write,
            False, False, None, None, None, -1, None)
```

which takes 21 arguments, and compare to the actual function signature from the repository:

https://github.com/uqfoundation/multiprocess/blob/39fedbdcaef54c90d486d122faa93ad20631d602/py3.11/multiprocess/util.py#L456-L460

which takes 23 arguments.

It seems that `uv` is grabbing multiprocess for Python 3.10. See uqfoundation/multiprocess/issues/145 and uqfoundation/multiprocess/issues/122.

PS. This doesn't happen when I use `pip install multiprocess`.



---

_Renamed from "`uv` install the wrong version of `multiprocess` for Python 3.11+" to "`uv` installs the wrong version of `multiprocess` for Python 3.11+" by @ma-sadeghi on 2024-03-26 11:15_

---

_Comment by @charliermarsh on 2024-03-26 13:10_

Can you please post the output of running with `-vvv`?

---

_Label `needs-mre` added by @charliermarsh on 2024-03-26 13:11_

---

_Comment by @ma-sadeghi on 2024-03-26 13:53_

There you go:

```shell
❯ uv pip install multiprocess -vvv
 uv::requirements::from_source source=multiprocess
    0.000167s DEBUG uv_interpreter::python_environment Found a virtualenv named .venv at: /home/amin/Code/tmp/bug/.venv
    0.000279s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.2, skipping probing: /home/amin/Code/tmp/bug/.venv/bin/python
    0.000286s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at /home/amin/Code/tmp/bug/.venv/bin/python
    0.000799s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.001739s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.001844s   0ms DEBUG uv_resolver::resolver Adding direct dependency: multiprocess*
   uv_resolver::resolver::choose_version package=multiprocess
     uv_resolver::resolver::package_wait package_name=multiprocess
 uv_resolver::resolver::process_request request=Versions multiprocess
   uv_client::registry_client::simple_api package=multiprocess
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/simple-v3/pypi/multiprocess.rkyv
 uv_resolver::resolver::process_request request=Prefetch multiprocess *
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/simple-v3/pypi/multiprocess.rkyv"
          0.002310s   0ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/multiprocess/
          0.002321s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/multiprocess/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/multiprocess/"
            0.002335s   0ms DEBUG uv_auth::middleware No credentials found for: https://pypi.org/simple/multiprocess/
          0.056166s  54ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/multiprocess/
       uv_client::cached_client::refresh_cache file=/home/amin/.cache/uv/simple-v3/pypi/multiprocess.rkyv
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=multiprocess==0.70.16
     uv_client::registry_client::wheel_metadata built_dist=multiprocess==0.70.16
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.msgpack
        0.056820s  54ms DEBUG uv_resolver::resolver Searching for a compatible version of multiprocess (*)
        0.056828s  54ms DEBUG uv_resolver::resolver Selecting: multiprocess==0.70.16 (multiprocess-0.70.16-py312-none-any.whl)
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=multiprocess, version=0.70.16
     uv_resolver::resolver::distributions_wait package_id=multiprocess-0.70.16
              0.056893s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl.metadata
        0.056941s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: dill>=0.3.8
   uv_resolver::resolver::choose_version package=dill
     uv_resolver::resolver::package_wait package_name=dill
 uv_resolver::resolver::process_request request=Versions dill
   uv_client::registry_client::simple_api package=dill
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/simple-v3/pypi/dill.rkyv
 uv_resolver::resolver::process_request request=Prefetch dill >=0.3.8
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/simple-v3/pypi/dill.rkyv"
          0.057070s   0ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/dill/
          0.057077s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/dill/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/dill/"
            0.057090s   0ms DEBUG uv_auth::middleware No credentials found for already-seen URL: https://pypi.org/simple/dill/
            0.057095s   0ms DEBUG uv_auth::middleware No credentials found for: https://pypi.org/simple/dill/
          0.066193s   9ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/dill/
       uv_client::cached_client::refresh_cache file=/home/amin/.cache/uv/simple-v3/pypi/dill.rkyv
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=dill==0.3.8
     uv_client::registry_client::wheel_metadata built_dist=dill==0.3.8
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack
        0.066783s   9ms DEBUG uv_resolver::resolver Searching for a compatible version of dill (>=0.3.8)
        0.066793s   9ms DEBUG uv_resolver::resolver Selecting: dill==0.3.8 (dill-0.3.8-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=dill, version=0.3.8
     uv_resolver::resolver::distributions_wait package_id=dill-0.3.8
              0.066847s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata
Resolved 2 packages in 65ms
    0.067053s DEBUG uv_installer::plan Requirement already cached: dill==0.3.8
    0.067181s DEBUG uv_installer::plan Requirement already cached: multiprocess==0.70.16
    0.067207s DEBUG uv_installer::plan Unnecessary package: mpire==2.10.0
    0.067211s DEBUG uv_installer::plan Unnecessary package: pygments==2.17.2
    0.067214s DEBUG uv_installer::plan Unnecessary package: tqdm==4.66.2
    0.067218s DEBUG uv_installer::plan Unnecessary package: bug==0.1.0 (from file:///home/amin/Code/tmp/bug)
 uv_installer::installer::install num_wheels=2
Installed 2 packages in 2ms
 + dill==0.3.8
 + multiprocess==0.70.16
```

---

_Comment by @ma-sadeghi on 2024-03-26 14:05_

Something strange happened: I removed `uv`'s cache (`rm -rf ~/.cache/uv/`), and reinstalled `multiprocess`, and it's the correct installation now!

Here's my hypothesis: IIRC, I was using Python 3.10, then switched over to 3.12 (I'm using `rye` to manage the virtualenv). Something might have happened during this transition. But again, I'm not sure, so don't put too much weight on this.

---

_Comment by @ma-sadeghi on 2024-03-26 14:05_

In case it's useful for debugging, here's the output after removing `uv`'s cache:

```shell
❯ uv pip install multiprocess -vvv
 uv::requirements::from_source source=multiprocess
    0.000209s DEBUG uv_interpreter::python_environment Found a virtualenv named .venv at: /home/amin/Code/tmp/bug/.venv
    0.000403s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.2, skipping probing: /home/amin/Code/tmp/bug/.venv/bin/python
    0.000415s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at /home/amin/Code/tmp/bug/.venv/bin/python
    0.001065s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.002172s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.002280s   0ms DEBUG uv_resolver::resolver Adding direct dependency: multiprocess*
   uv_resolver::resolver::choose_version package=multiprocess
     uv_resolver::resolver::package_wait package_name=multiprocess
 uv_resolver::resolver::process_request request=Versions multiprocess
   uv_client::registry_client::simple_api package=multiprocess
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/simple-v3/pypi/multiprocess.rkyv
 uv_resolver::resolver::process_request request=Prefetch multiprocess *
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/simple-v3/pypi/multiprocess.rkyv"
          0.002870s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/multiprocess/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/multiprocess/"
            0.002924s   0ms DEBUG uv_auth::middleware No credentials found for: https://pypi.org/simple/multiprocess/
       uv_client::cached_client::new_cache file=/home/amin/.cache/uv/simple-v3/pypi/multiprocess.rkyv
       uv_client::registry_client::parse_simple_api package=multiprocess
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=multiprocess==0.70.16
     uv_client::registry_client::wheel_metadata built_dist=multiprocess==0.70.16
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.msgpack
        0.063817s  61ms DEBUG uv_resolver::resolver Searching for a compatible version of multiprocess (*)
        0.063827s  61ms DEBUG uv_resolver::resolver Selecting: multiprocess==0.70.16 (multiprocess-0.70.16-py312-none-any.whl)
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=multiprocess, version=0.70.16
     uv_resolver::resolver::distributions_wait package_id=multiprocess-0.70.16
              0.063889s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl.metadata"
                0.063926s   0ms DEBUG uv_auth::middleware No credentials found for: https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl.metadata
           uv_client::cached_client::new_cache file=/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.msgpack
           uv_client::registry_client::parse_metadata21 
        0.277753s 213ms DEBUG uv_resolver::resolver Adding transitive dependency: dill>=0.3.8
   uv_resolver::resolver::choose_version package=dill
     uv_resolver::resolver::package_wait package_name=dill
 uv_resolver::resolver::process_request request=Versions dill
   uv_client::registry_client::simple_api package=dill
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/simple-v3/pypi/dill.rkyv
 uv_resolver::resolver::process_request request=Prefetch dill >=0.3.8
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/simple-v3/pypi/dill.rkyv"
          0.277928s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/dill/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/dill/"
            0.277954s   0ms DEBUG uv_auth::middleware No credentials found for already-seen URL: https://pypi.org/simple/dill/
            0.277960s   0ms DEBUG uv_auth::middleware No credentials found for: https://pypi.org/simple/dill/
       uv_client::cached_client::new_cache file=/home/amin/.cache/uv/simple-v3/pypi/dill.rkyv
       uv_client::registry_client::parse_simple_api package=dill
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=dill==0.3.8
     uv_client::registry_client::wheel_metadata built_dist=dill==0.3.8
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack
        0.291964s  14ms DEBUG uv_resolver::resolver Searching for a compatible version of dill (>=0.3.8)
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack"
        0.291992s  14ms DEBUG uv_resolver::resolver Selecting: dill==0.3.8 (dill-0.3.8-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=dill, version=0.3.8
     uv_resolver::resolver::distributions_wait package_id=dill-0.3.8
              0.292041s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata"
                0.292063s   0ms DEBUG uv_auth::middleware No credentials found for already-seen URL: https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata
                0.292070s   0ms DEBUG uv_auth::middleware No credentials found for: https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata
           uv_client::cached_client::new_cache file=/home/amin/.cache/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21 
Resolved 2 packages in 300ms
    0.302583s DEBUG uv_installer::plan Requirement already satisfied: dill==0.3.8
    0.302649s DEBUG uv_installer::plan Identified uncached requirement: multiprocess ==0.70.16
    0.302679s DEBUG uv_installer::plan Unnecessary package: mpire==2.10.0
    0.302685s DEBUG uv_installer::plan Unnecessary package: pygments==2.17.2
    0.302689s DEBUG uv_installer::plan Unnecessary package: tqdm==4.66.2
    0.302692s DEBUG uv_installer::plan Unnecessary package: bug==0.1.0 (from file:///home/amin/Code/tmp/bug)
 uv_installer::downloader::download total=1
   uv_installer::downloader::get_wheel name=multiprocess==0.70.16, size=Some(146741), url="https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("multiprocess"), version: "0.70.16", python_tag: ["py312"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("4afd9e1f60a38f28d561aaf24caa8791b25397195503e49b9b3882824049790d") })), filename: "multiprocess-0.70.16-py312-none-any.whl", hashes: Hashes { md5: None, sha256: Some("fc0544c531920dde3b00c29863377f87e1632601092ea2daca74e4beb40faa2e") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8" }])), size: Some(146741), upload_time_utc_ms: Some(1706467949395), url: AbsoluteUrl("https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl"), yanked: Some(Bool(false)) }, index: Pypi(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }) }))
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.http
 uv_client::cached_client::from_path_sync path="/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.http"
              0.302915s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl"
                0.302939s   0ms DEBUG uv_auth::middleware No credentials found for already-seen URL: https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl
                0.302945s   0ms DEBUG uv_auth::middleware No credentials found for: https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl
           uv_client::cached_client::new_cache file=/home/amin/.cache/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py312-none-any.http
           uv_distribution::distribution_database::wheel wheel=multiprocess==0.70.16
Downloaded 1 package in 40ms
 uv_installer::installer::install num_wheels=1
Installed 1 package in 2ms
 + multiprocess==0.70.16
```

---

_Referenced in [sybrenjansen/mpire#127](../../sybrenjansen/mpire/issues/127.md) on 2024-03-26 14:07_

---

_Comment by @ma-sadeghi on 2024-03-26 14:19_

**Update**: I switched back and forth between Python 3.10 and 3.12, and I can no longer reproduce the issue, even without removing the cache. Feel free to close this issue, unless you think there's more to it. Thanks for the quick follow up!

---

_Comment by @charliermarsh on 2024-03-26 14:38_

That's weird! Must be something related to caching the interpreter state... But I'm glad it's resolved. If it comes up again just LMK!

---

_Closed by @charliermarsh on 2024-03-26 14:38_

---

_Referenced in [datachain-ai/datachain#1256](../../datachain-ai/datachain/issues/1256.md) on 2025-07-28 16:56_

---

```yaml
number: 2261
title: "uv installs wheel for a different python version for `multiprocess`"
type: issue
state: closed
author: skshetry
labels:
  - bug
assignees: []
created_at: 2024-03-07T04:31:12Z
updated_at: 2024-03-07T14:04:56Z
url: https://github.com/astral-sh/uv/issues/2261
synced_at: 2026-01-12T15:58:36Z
```

# uv installs wheel for a different python version for `multiprocess`

---

_@skshetry_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv pip install multiprocess` installs `multiprocess-0.70.16-py38-none-any.whl` wheel for Python 3.12. `pip` correctly installs `
multiprocess-0.70.16-py312-none-any.whl`. 

```bash
#! /bin/sh

cd "$(mktemp -d)"
rm -rf $(uv cache dir)
uv --version
uv venv -p 3.12
source .venv/bin/activate
uv pip install --verbose multiprocess
```

```
uv_installer::downloader::get_wheel name=multiprocess==0.70.16, size=Some(132628), url="https://files.pythonhosted.org/packages/ea/89/38df130f2c799090c978b366cfdf5b96d08de5b29a4a293df7f7429fa50b/multiprocess-0.70.16-py38-none-any.whl"
```

<details><summary>uv output</summary>


<p>

```bash
uv 0.1.15
Using Python 3.12.2 interpreter at: /usr/local/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
 uv::requirements::from_source source=multiprocess
    0.000413s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.92oXomAb79/.venv
    0.000596s DEBUG uv_interpreter::interpreter Probing interpreter info for: /private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.92oXomAb79/.venv/bin/python
    0.052221s DEBUG uv_interpreter::interpreter Found Python 3.12.2 for: /private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.92oXomAb79/.venv/bin/python
    0.052711s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at /private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.92oXomAb79/.venv/bin/python
    0.053919s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.298799s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.298886s   0ms DEBUG uv_resolver::resolver Adding direct dependency: multiprocess*
   uv_resolver::resolver::choose_version package=multiprocess
     uv_resolver::resolver::package_wait package_name=multiprocess
 uv_resolver::resolver::process_request request=Versions multiprocess
   uv_client::registry_client::simple_api package=multiprocess
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/saugat/Library/Caches/uv/simple-v3/pypi/multiprocess.rkyv
 uv_resolver::resolver::process_request request=Prefetch multiprocess *
 uv_client::cached_client::from_path_sync path="/Users/saugat/Library/Caches/uv/simple-v3/pypi/multiprocess.rkyv"
          0.299322s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/multiprocess/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/multiprocess/"
       uv_client::cached_client::new_cache file=/Users/saugat/Library/Caches/uv/simple-v3/pypi/multiprocess.rkyv
       uv_client::registry_client::parse_simple_api package=multiprocess
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=multiprocess==0.70.16
     uv_client::registry_client::wheel_metadata built_dist=multiprocess==0.70.16
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py38-none-any.msgpack
        0.458003s 159ms DEBUG uv_resolver::resolver Searching for a compatible version of multiprocess (*)
        0.458019s 159ms DEBUG uv_resolver::resolver Selecting: multiprocess==0.70.16 (multiprocess-0.70.16-py38-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/saugat/Library/Caches/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py38-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=multiprocess, version=0.70.16
     uv_resolver::resolver::distributions_wait package_id=multiprocess-0.70.16
              0.458107s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/ea/89/38df130f2c799090c978b366cfdf5b96d08de5b29a4a293df7f7429fa50b/multiprocess-0.70.16-py38-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/ea/89/38df130f2c799090c978b366cfdf5b96d08de5b29a4a293df7f7429fa50b/multiprocess-0.70.16-py38-none-any.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py38-none-any.msgpack
           uv_client::registry_client::parse_metadata21
        0.586427s 128ms DEBUG uv_resolver::resolver Adding transitive dependency: dill>=0.3.8
   uv_resolver::resolver::choose_version package=dill
     uv_resolver::resolver::package_wait package_name=dill
 uv_resolver::resolver::process_request request=Versions dill
   uv_client::registry_client::simple_api package=dill
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/saugat/Library/Caches/uv/simple-v3/pypi/dill.rkyv
 uv_resolver::resolver::process_request request=Prefetch dill >=0.3.8
 uv_client::cached_client::from_path_sync path="/Users/saugat/Library/Caches/uv/simple-v3/pypi/dill.rkyv"
          0.586704s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/dill/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/dill/"
       uv_client::cached_client::new_cache file=/Users/saugat/Library/Caches/uv/simple-v3/pypi/dill.rkyv
       uv_client::registry_client::parse_simple_api package=dill
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=dill==0.3.8
     uv_client::registry_client::wheel_metadata built_dist=dill==0.3.8
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack
        0.635846s  49ms DEBUG uv_resolver::resolver Searching for a compatible version of dill (>=0.3.8)
        0.635864s  49ms DEBUG uv_resolver::resolver Selecting: dill==0.3.8 (dill-0.3.8-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/saugat/Library/Caches/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=dill, version=0.3.8
     uv_resolver::resolver::distributions_wait package_id=dill-0.3.8
              0.636019s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21
Resolved 2 packages in 360ms
    0.659407s DEBUG uv_installer::plan Identified uncached requirement: dill ==0.3.8
    0.659486s DEBUG uv_installer::plan Identified uncached requirement: multiprocess ==0.70.16
 uv_installer::downloader::download total=2
   uv_installer::downloader::get_wheel name=multiprocess==0.70.16, size=Some(132628), url="https://files.pythonhosted.org/packages/ea/89/38df130f2c799090c978b366cfdf5b96d08de5b29a4a293df7f7429fa50b/multiprocess-0.70.16-py38-none-any.whl"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("multiprocess"), version: "0.70.16", python_tag: ["py38"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("afae2f161faa255a820fa77e06fc93ef7faf238cad6b8f32000db87d372f524f") })), filename: "multiprocess-0.70.16-py38-none-any.whl", hashes: Hashes { md5: None, sha256: Some("a71d82033454891091a226dfc319d0cfa8019a4e888ef9ca910372a446de4435") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8" }])), size: Some(132628), upload_time_utc_ms: Some(1706467950853), url: AbsoluteUrl("https://files.pythonhosted.org/packages/ea/89/38df130f2c799090c978b366cfdf5b96d08de5b29a4a293df7f7429fa50b/multiprocess-0.70.16-py38-none-any.whl"), yanked: Some(Bool(false)) }, index: Pypi(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }) }))
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py38-none-any.http
   uv_installer::downloader::get_wheel name=dill==0.3.8, size=Some(116252), url="https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl"
 uv_client::cached_client::from_path_sync path="/Users/saugat/Library/Caches/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py38-none-any.http"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("dill"), version: "0.3.8", python_tag: ["py3"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("531912b36714f09cab26c579912d1047d72b274ee1ad4252d91888310682e2cb") })), filename: "dill-0.3.8-py3-none-any.whl", hashes: Hashes { md5: None, sha256: Some("c36ca9ffb54365bdd2f8eb3eff7d2a21237f8452b57ace88b1ac615b7e815bd7") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8" }])), size: Some(116252), upload_time_utc_ms: Some(1706398934239), url: AbsoluteUrl("https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl"), yanked: Some(Bool(false)) }, index: Pypi(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }) }))
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.http
              0.659994s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/ea/89/38df130f2c799090c978b366cfdf5b96d08de5b29a4a293df7f7429fa50b/multiprocess-0.70.16-py38-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/ea/89/38df130f2c799090c978b366cfdf5b96d08de5b29a4a293df7f7429fa50b/multiprocess-0.70.16-py38-none-any.whl"
 uv_client::cached_client::from_path_sync path="/Users/saugat/Library/Caches/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.http"
              0.660080s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl"
           uv_client::cached_client::new_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/dill/dill-0.3.8-py3-none-any.http
           uv_distribution::distribution_database::download wheel=dill==0.3.8
           uv_client::cached_client::new_cache file=/Users/saugat/Library/Caches/uv/wheels-v0/pypi/multiprocess/multiprocess-0.70.16-py38-none-any.http
           uv_distribution::distribution_database::download wheel=multiprocess==0.70.16
Downloaded 2 packages in 79ms
 uv_installer::installer::install num_wheels=2
Installed 2 packages in 5ms
 + dill==0.3.8
 + multiprocess==0.70.16
```

</p>
</details> 

### Comparing with `pip`

```bash
#! /bin/sh

cd "$(mktemp -d)"
uv venv -p 3.12 --seed
source .venv/bin/activate
pip --version
pip install --verbose multiprocess
```

<details><summary>pip output</summary>
<p>

```console
Using Python 3.12.2 interpreter at: /usr/local/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
 + pip==24.0
Activate with: source .venv/bin/activate
pip 24.0 from /private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.7vziLpwtzM/.venv/lib/python3.12/site-packages/pip (python 3.12)
Using pip 24.0 from /private/var/folders/xh/trg29z296h70n109kwfk6g800000gn/T/tmp.7vziLpwtzM/.venv/lib/python3.12/site-packages/pip (python 3.12)
Collecting multiprocess
  Obtaining dependency information for multiprocess from https://files.pythonhosted.org/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl.metadata
  Downloading multiprocess-0.70.16-py312-none-any.whl.metadata (7.2 kB)
Collecting dill>=0.3.8 (from multiprocess)
  Obtaining dependency information for dill>=0.3.8 from https://files.pythonhosted.org/packages/c9/7a/cef76fd8438a42f96db64ddaa85280485a9c395e7df3db8158cfec1eee34/dill-0.3.8-py3-none-any.whl.metadata
  Downloading dill-0.3.8-py3-none-any.whl.metadata (10 kB)
Downloading multiprocess-0.70.16-py312-none-any.whl (146 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 146.7/146.7 kB 7.6 MB/s eta 0:00:00
Downloading dill-0.3.8-py3-none-any.whl (116 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 116.3/116.3 kB 109.3 MB/s eta 0:00:00
Installing collected packages: dill, multiprocess
Successfully installed dill-0.3.8 multiprocess-0.70.16
```

</p>
</details> 



---

_Renamed from "uv installs wheel for a different python version for multiprocess" to "uv installs wheel for a different python version for `multiprocess`" by @skshetry on 2024-03-07 04:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 04:45_

---

_Label `bug` added by @charliermarsh on 2024-03-07 04:46_

---

_Comment by @charliermarsh on 2024-03-07 04:46_

Thanks, fix here: https://github.com/astral-sh/uv/pull/2263.

---

_Closed by @charliermarsh on 2024-03-07 14:04_

---

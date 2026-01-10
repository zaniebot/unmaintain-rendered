```yaml
number: 1451
title: uv pip install for package works from --index-url but not from --extra-index-url
type: issue
state: closed
author: exs-avianello
labels:
  - bug
assignees: []
created_at: 2024-02-16T08:15:26Z
updated_at: 2024-04-03T06:49:42Z
url: https://github.com/astral-sh/uv/issues/1451
synced_at: 2026-01-10T05:40:31Z
```

# uv pip install for package works from --index-url but not from --extra-index-url

---

_Issue opened by @exs-avianello on 2024-02-16 08:15_

Hello!

I am testing out `uv` with a package called [openeye-toolkits](https://docs.eyesopen.com/toolkits/python/quickstart-python/linuxosx_virtualenv.html), which is not hosted on PyPI but on another (public) package index.

If the index is provided as `--index-url` everything works fine* (the download eventually times out but that's probably because of https://github.com/astral-sh/uv/issues/1549): 

```
uv pip install -v --no-cache --index-url "https://pypi.anaconda.org/OpenEye/simple" openeye-toolkits
    0.004410s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: .../.venv
    0.005885s DEBUG uv_interpreter::interpreter Detecting markers for: .../.venv/bin/python
    0.079283s DEBUG uv::commands::pip_install Using Python 3.11.5 environment at .../.venv/bin/python
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.096637s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.5
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.098462s   0ms DEBUG uv_resolver::resolver Adding direct dependency: openeye-toolkits*
   uv_resolver::resolver::choose_version package=openeye-toolkits
     uv_resolver::resolver::package_wait package_name=openeye-toolkits
 uv_resolver::resolver::process_request request=Versions openeye-toolkits
   uv_client::registry_client::simple_api package=openeye-toolkits
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/simple-v0/937cb77f9f4a4ef4/openeye-toolkits.rkyv
 uv_resolver::resolver::process_request request=Prefetch openeye-toolkits *
          0.101202s   1ms  WARN uv_client::cached_client Broken cache entry at /private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/simple-v0/937cb77f9f4a4ef4/openeye-toolkits.rkyv, removing: failed to open file `/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/simple-v0/937cb77f9f4a4ef4/openeye-toolkits.rkyv`
          0.101565s   1ms DEBUG uv_client::cached_client No cache entry for: https://pypi.anaconda.org/OpenEye/simple/openeye-toolkits/
       uv_client::cached_client::fresh_request url="https://pypi.anaconda.org/OpenEye/simple/openeye-toolkits/"
       uv_client::cached_client::new_cache file=/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/simple-v0/937cb77f9f4a4ef4/openeye-toolkits.rkyv
       uv_client::registry_client::parse_simple_api package=openeye-toolkits
         uv_client::html::parse url=Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.anaconda.org")), port: None, path: "/OpenEye/simple/openeye-toolkits/", query: None, fragment: None }
 uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=openeye-toolkits==2023.2.3
     uv_client::registry_client::wheel_metadata built_dist=openeye-toolkits==2023.2.3
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/wheels-v0/index/937cb77f9f4a4ef4/openeye-toolkits/openeye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.msgpack
              0.802496s   0ms  WARN uv_client::cached_client Broken cache entry at /private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/wheels-v0/index/937cb77f9f4a4ef4/openeye-toolkits/openeye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.msgpack, removing: failed to open file `/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/wheels-v0/index/937cb77f9f4a4ef4/openeye-toolkits/openeye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.msgpack`
        0.802534s 702ms DEBUG uv_resolver::resolver Searching for a compatible version of openeye-toolkits (*)
        0.802548s 702ms DEBUG uv_resolver::resolver Selecting: openeye-toolkits==2023.2.3 (OpenEye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.whl)
   uv_resolver::resolver::get_dependencies package=openeye-toolkits, version=2023.2.3
     uv_resolver::resolver::distributions_wait package_id=openeye-toolkits-2023.2.3
              0.802634s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.anaconda.org/openeye/simple/openeye-toolkits/2023.2.3/OpenEye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.whl
           uv_client::cached_client::fresh_request url="https://pypi.anaconda.org/openeye/simple/openeye-toolkits/2023.2.3/OpenEye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.whl"
           uv_client::cached_client::new_cache file=/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpoWZVgP/wheels-v0/index/937cb77f9f4a4ef4/openeye-toolkits/openeye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.msgpack
           uv_client::registry_client::read_metadata_range_request wheel=openeye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.whl
          1.117623s 315ms DEBUG uv_client::registry_client Range requests not supported for openeye_toolkits-2023.2.3-py39.py310.py311-none-macosx_10_16_universal2.whl; downloading wheel
error: Failed to download: openeye-toolkits==2023.2.3
  Caused by: Failed to write to the client cache
  Caused by: request or response body error: operation timed out
  Caused by: operation timed out
```

But if the index is provided as an `--extra-index-url` `uv` seems to be giving up before looking up the extra index:
```
uv pip install -v --no-cache --extra-index-url "https://pypi.anaconda.org/OpenEye/simple" openeye-toolkits
    0.001477s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: .../.venv
    0.001695s DEBUG uv_interpreter::interpreter Detecting markers for: .../.venv/bin/python
    0.079837s DEBUG uv::commands::pip_install Using Python 3.11.5 environment at .../.venv/bin/python
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.086637s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.5
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.086823s   0ms DEBUG uv_resolver::resolver Adding direct dependency: openeye-toolkits*
   uv_resolver::resolver::choose_version package=openeye-toolkits
     uv_resolver::resolver::package_wait package_name=openeye-toolkits
 uv_resolver::resolver::process_request request=Versions openeye-toolkits
   uv_client::registry_client::simple_api package=openeye-toolkits
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpjr1cog/simple-v0/pypi/openeye-toolkits.rkyv
 uv_resolver::resolver::process_request request=Prefetch openeye-toolkits *
          0.087341s   0ms  WARN uv_client::cached_client Broken cache entry at /private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpjr1cog/simple-v0/pypi/openeye-toolkits.rkyv, removing: failed to open file `/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpjr1cog/simple-v0/pypi/openeye-toolkits.rkyv`
          0.087452s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/openeye-toolkits/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/openeye-toolkits/"
       uv_client::cached_client::new_cache file=/private/var/folders/bq/m80wbcjd677_l_hfy99xpm980000gq/T/.tmpjr1cog/simple-v0/pypi/openeye-toolkits.rkyv
       uv_client::registry_client::parse_simple_api package=openeye-toolkits
 uv_resolver::version_map::from_metadata
        0.390276s 303ms DEBUG uv_resolver::resolver Searching for a compatible version of openeye-toolkits (*)
      0.390308s 303ms DEBUG uv_resolver::resolver No compatible version found for: openeye-toolkits
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of openeye-toolkits and you require openeye-toolkits, we can conclude that the requirements are unsatisfiable.
```

The equivalent `pip`-native commands work in both cases. 

Note that it might very well be a problem specific to this package because historically we have had general issues with it / its metadata 🙌 

---

_Comment by @indigoviolet on 2024-02-17 09:15_

I see this with `rye` (https://rye-up.com/guide/sources/) 0.24.0 and https://docs.apryse.com/documentation/python/get-started/python3/mac/

---

_Comment by @kafonek on 2024-02-17 21:47_

Per #1600 (closed as dupe), another library to test with is `polygraphy==0.47.1`, which has an entry on the nvidia repo but not PyPI.

 - ✅ `uv pip install polygraphy==0.47.1 --index-url https://pypi.ngc.nvidia.com` 
 - ❌ `uv pip install polygraphy==0.47.1 --extra-index-url https://pypi.ngc.nvidia.com`

---

_Comment by @charliermarsh on 2024-02-17 21:49_

Yeah I suspect the general problem here is that we only look at versions from PyPI, if the package exists on PyPI. So if there are versions that _only_ exist in another index, we miss those right now.

---

_Comment by @zyd14 on 2024-02-21 18:30_

I'm also running into this issue. However, @charliermarsh, the packages I'm trying to install exist on PyPI as well as in CodeArtifact - but the specific version I want to install from CodeArtifact only exists in CodeArtifact (it is differentiated by adding a `+etx` to the end of the version).

---

_Label `bug` added by @BurntSushi on 2024-02-29 15:30_

---

_Closed by @BurntSushi on 2024-02-29 16:57_

---

_Comment by @leosongwei on 2024-04-03 06:49_

any way to set a global index-url?

---

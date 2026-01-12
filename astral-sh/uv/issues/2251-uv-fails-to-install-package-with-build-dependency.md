```yaml
number: 2251
title: uv fails to install package with build dependency on pip
type: issue
state: closed
author: kpeez
labels:
  - question
assignees: []
created_at: 2024-03-06T21:05:17Z
updated_at: 2024-03-07T23:58:00Z
url: https://github.com/astral-sh/uv/issues/2251
synced_at: 2026-01-12T15:58:36Z
```

# uv fails to install package with build dependency on pip

---

_@kpeez_

## Description

I am attempting to install [ViTPose](https://github.com/ViTAE-Transformer/ViTPose) directly from the repository. Using `uv pip install` results in a failure with ModuleNotFoundError: No module named 'pip', despite pip being successfully installed in the environment. However, using pip directly after activating the virtual environment succeeds without any errors

## Environment:
Operating System: macOS 14.3.1
Python Version: 3.11.8
uv Version: 0.1.15
pip Version: 24.0


## Steps to reproduce
The following fails:
```
mkdir test
cd test
uv venv
uv pip install "mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose"
```

but this works without issue:
```
mkdir test
cd test
uv venv
source .venv/bin/activate
pip install "mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose"
```

## Traceback

Here is the traceback for the failed installation attempt (using --verbose):
```
 uv::requirements::from_source source=mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose
    0.001636s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/kyle/Desktop/test/.venv
    0.002054s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.8, skipping probing: /Users/kyle/Desktop/test/.venv/bin/python
    0.002062s DEBUG uv::commands::pip_install Using Python 3.11.8 environment at /Users/kyle/Desktop/test/.venv/bin/python
    0.003154s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.152214s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.8
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.152834s   0ms DEBUG uv_resolver::resolver Adding direct dependency: mmpose*
   uv_resolver::resolver::choose_version package=mmpose
        0.153109s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose (*)
 uv_resolver::resolver::process_request request=Metadata mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose
 uv_git::source::fetch
      0.287567s   5ms DEBUG uv_git::source Updating git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/ViTAE-Transformer/ViTPose", query: None, fragment: None }`
      0.290143s   8ms DEBUG uv_git::git Attempting GitHub fast path for: https://api.github.com/repos/ViTAE-Transformer/ViTPose/commits/HEAD
      0.629571s 347ms DEBUG uv_git::git skipping gc as there's only 2 pack files
      0.629910s 348ms DEBUG uv_git::git Performing a Git fetch for: https://github.com/ViTAE-Transformer/ViTPose
      1.291590s   1s  DEBUG uv_git::git Update submodules for: /Users/kyle/Library/Caches/uv/git-v0/checkouts/a4bbcc9f2738db1d/d521645/
        1.301932s   1s  DEBUG uv_distribution::source Fetching source distribution from Git: git+https://github.com/ViTAE-Transformer/ViTPose@d5216452796c90c6bc29f5c5ec0bdba94366768a
 uv_git::source::fetch
      1.433566s   0ms DEBUG uv_git::git Update submodules for: /Users/kyle/Library/Caches/uv/git-v0/checkouts/a4bbcc9f2738db1d/d521645/
        1.436974s   1s  DEBUG uv_distribution::source Using cached metadata for mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose@d5216452796c90c6bc29f5c5ec0bdba94366768a
   uv_resolver::resolver::get_dependencies package=mmpose, version=0.24.0
     uv_resolver::resolver::distributions_wait package_id=f43c44c0d83cdeb1
        1.437129s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: chumpy*
        1.437137s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: json-tricks*
        1.437140s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: matplotlib*
        1.437143s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: munkres*
        1.437147s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: numpy*
        1.437149s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: opencv-python*
        1.437151s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: pillow*
        1.437154s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: scipy*
        1.437156s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: torchvision*
        1.437158s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: xtcocotools>=1.8
   uv_resolver::resolver::choose_version package=chumpy
     uv_resolver::resolver::package_wait package_name=chumpy
 uv_resolver::resolver::process_request request=Versions chumpy
   uv_client::registry_client::simple_api package=chumpy
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/chumpy.rkyv
 uv_resolver::resolver::process_request request=Versions json-tricks
   uv_client::registry_client::simple_api package=json-tricks
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/json-tricks.rkyv
 uv_resolver::resolver::process_request request=Versions matplotlib
   uv_client::registry_client::simple_api package=matplotlib
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/matplotlib.rkyv
 uv_resolver::resolver::process_request request=Versions munkres
   uv_client::registry_client::simple_api package=munkres
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/munkres.rkyv
 uv_resolver::resolver::process_request request=Versions numpy
   uv_client::registry_client::simple_api package=numpy
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/numpy.rkyv
 uv_resolver::resolver::process_request request=Versions opencv-python
   uv_client::registry_client::simple_api package=opencv-python
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/opencv-python.rkyv
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/chumpy.rkyv"
 uv_resolver::resolver::process_request request=Versions pillow
   uv_client::registry_client::simple_api package=pillow
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/matplotlib.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/json-tricks.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/numpy.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/opencv-python.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/munkres.rkyv"
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/pillow.rkyv
 uv_resolver::resolver::process_request request=Versions scipy
   uv_client::registry_client::simple_api package=scipy
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/scipy.rkyv
 uv_resolver::resolver::process_request request=Versions torchvision
   uv_client::registry_client::simple_api package=torchvision
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/torchvision.rkyv
 uv_resolver::resolver::process_request request=Versions xtcocotools
   uv_client::registry_client::simple_api package=xtcocotools
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/xtcocotools.rkyv
 uv_resolver::resolver::process_request request=Prefetch xtcocotools >=1.8
 uv_resolver::resolver::process_request request=Prefetch torchvision *
 uv_resolver::resolver::process_request request=Prefetch scipy *
 uv_resolver::resolver::process_request request=Prefetch pillow *
 uv_resolver::resolver::process_request request=Prefetch opencv-python *
 uv_resolver::resolver::process_request request=Prefetch numpy *
 uv_resolver::resolver::process_request request=Prefetch munkres *
 uv_resolver::resolver::process_request request=Prefetch matplotlib *
 uv_resolver::resolver::process_request request=Prefetch json-tricks *
 uv_resolver::resolver::process_request request=Prefetch chumpy *
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/pillow.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/torchvision.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/scipy.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/xtcocotools.rkyv"
          1.438552s   1ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/chumpy/
          1.438562s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/chumpy/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/chumpy/"
          1.438638s   1ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/munkres/
          1.438642s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/munkres/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/munkres/"
          1.438664s   1ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/json-tricks/
          1.438667s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/json-tricks/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/json-tricks/"
          1.439123s   1ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/xtcocotools/
          1.439132s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/xtcocotools/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/xtcocotools/"
          1.439252s   1ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/torchvision/
          1.439266s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/torchvision/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/torchvision/"
          1.440223s   2ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/opencv-python/
          1.440239s   2ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/opencv-python/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/opencv-python/"
          1.440288s   2ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/matplotlib/
          1.440294s   2ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/matplotlib/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/matplotlib/"
          1.440473s   2ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/scipy/
          1.440480s   2ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/scipy/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/scipy/"
          1.440665s   3ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/pillow/
          1.440670s   3ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/pillow/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/pillow/"
          1.441169s   3ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/numpy/
          1.441173s   3ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/numpy/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/numpy/"
          1.556887s 119ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/scipy/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/scipy.rkyv
          1.557508s 120ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/matplotlib/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/matplotlib.rkyv
          1.558080s 120ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/opencv-python/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/opencv-python.rkyv
          1.558348s 120ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/pillow/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/pillow.rkyv
          1.558833s 121ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/xtcocotools/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/xtcocotools.rkyv
          1.558932s 121ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/numpy/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/numpy.rkyv
          1.559557s 122ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/munkres/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/munkres.rkyv
          1.559642s 122ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/json-tricks/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/json-tricks.rkyv
          1.559728s 122ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/chumpy/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/chumpy.rkyv
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=scipy==1.12.0
     uv_client::registry_client::wheel_metadata built_dist=scipy==1.12.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/scipy/scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/scipy/scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=matplotlib==3.8.3
     uv_client::registry_client::wheel_metadata built_dist=matplotlib==3.8.3
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/matplotlib/matplotlib-3.8.3-cp311-cp311-macosx_11_0_arm64.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/matplotlib/matplotlib-3.8.3-cp311-cp311-macosx_11_0_arm64.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=opencv-python==4.9.0.80
     uv_client::registry_client::wheel_metadata built_dist=opencv-python==4.9.0.80
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/opencv-python/opencv_python-4.9.0.80-cp37-abi3-macosx_11_0_arm64.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/opencv-python/opencv_python-4.9.0.80-cp37-abi3-macosx_11_0_arm64.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=xtcocotools==1.14.3
     uv_client::registry_client::wheel_metadata built_dist=xtcocotools==1.14.3
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/xtcocotools/xtcocotools-1.14.3-cp311-cp311-macosx_10_9_universal2.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/xtcocotools/xtcocotools-1.14.3-cp311-cp311-macosx_10_9_universal2.msgpack"
          1.566682s 128ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/torchvision/
       uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/torchvision.rkyv
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pillow==10.2.0
     uv_client::registry_client::wheel_metadata built_dist=pillow==10.2.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/pillow/pillow-10.2.0-cp311-cp311-macosx_11_0_arm64.msgpack
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=munkres==1.1.4
     uv_client::registry_client::wheel_metadata built_dist=munkres==1.1.4
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/munkres/munkres-1.1.4-py2.py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/pillow/pillow-10.2.0-cp311-cp311-macosx_11_0_arm64.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=json-tricks==3.17.3
     uv_client::registry_client::wheel_metadata built_dist=json-tricks==3.17.3
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/munkres/munkres-1.1.4-py2.py3-none-any.msgpack"
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/json-tricks/json_tricks-3.17.3-py2.py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/json-tricks/json_tricks-3.17.3-py2.py3-none-any.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=numpy==1.26.4
     uv_client::registry_client::wheel_metadata built_dist=numpy==1.26.4
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/numpy/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.msgpack
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=chumpy==0.70
     uv_client::cached_client::get_serde
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/built-wheels-v0/pypi/chumpy/0.70/manifest.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/numpy/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.msgpack"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/built-wheels-v0/pypi/chumpy/0.70/manifest.msgpack"
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=torchvision==0.17.1
     uv_client::registry_client::wheel_metadata built_dist=torchvision==0.17.1
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/torchvision/torchvision-0.17.1-cp311-cp311-macosx_11_0_arm64.msgpack
        1.567443s 130ms DEBUG uv_resolver::resolver Searching for a compatible version of chumpy (*)
        1.567448s 130ms DEBUG uv_resolver::resolver Selecting: chumpy==0.70 (chumpy-0.70.tar.gz)
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/torchvision/torchvision-0.17.1-cp311-cp311-macosx_11_0_arm64.msgpack"
   uv_resolver::resolver::get_dependencies package=chumpy, version=0.70
     uv_resolver::resolver::distributions_wait package_id=chumpy-0.70
            1.567977s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/01/f7/865755c8bdb837841938de622e6c8b5cb6b1c933bde3bd3332f0cd4574f1/chumpy-0.70.tar.gz
              1.568008s   1ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/24/db/6ec78a4f10673a641cdb11694c2de2f64aa00e838551248cb11b8b057440/matplotlib-3.8.3-cp311-cp311-macosx_11_0_arm64.whl.metadata
              1.568086s   1ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/77/df/b56175c3fb5bc058774bdcf35f5a71cf9c3c5b909f98a1c688eb71cd3b1f/opencv_python-4.9.0.80-cp37-abi3-macosx_11_0_arm64.whl.metadata
              1.568104s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/a8/95/affe174621ebc9b32544a918ae6cb758ac06061d0ee51d783e06906ceba1/torchvision-0.17.1-cp311-cp311-macosx_11_0_arm64.whl.metadata
     uv_distribution::source::build_source_dist_metadata
          1.568136s   0ms DEBUG uv_distribution::source Preparing metadata for: chumpy==0.70
       uv_dispatch::setup_build package_id="chumpy==0.70", subdirectory=None
         uv_resolver::resolver::solve
              1.570463s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.8
           uv_resolver::resolver::choose_version package=root
           uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
                1.570488s   0ms DEBUG uv_resolver::resolver Adding direct dependency: wheel*
                1.570495s   0ms DEBUG uv_resolver::resolver Adding direct dependency: setuptools>=40.8.0
           uv_resolver::resolver::choose_version package=wheel
             uv_resolver::resolver::package_wait package_name=wheel
              1.570522s   3ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata
              1.570540s   4ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/21/d4/e6c57acc61e59cd46acca27af1f400094d5dee218e372cc604b8162b97cb/scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl.metadata
              1.570561s   3ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/46/ce/a84284ab66a278825109b03765d7411be3ff18250da44faa9fb5ea9a16a0/pillow-10.2.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
              1.570576s   3ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/bb/70/ff89ebf5c3e014467224e6b0a0647ba90c0dfb1d3309caf1228f1007f760/xtcocotools-1.14.3-cp311-cp311-macosx_10_9_universal2.whl.metadata
              1.570587s   3ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/ae/fd/e3edcf827e7f9c17c5ea1a192841dcfb1dd575a7518c25c5cadd921625b1/json_tricks-3.17.3-py2.py3-none-any.whl.metadata
              1.570594s   3ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/90/ab/0301c945a704218bc9435f0e3c88884f6b19ef234d8899fb47ce1ccfd0c9/munkres-1.1.4-py2.py3-none-any.whl.metadata
         uv_resolver::resolver::process_request request=Versions wheel
           uv_client::registry_client::simple_api package=wheel
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/wheel.rkyv
         uv_resolver::resolver::process_request request=Versions setuptools
           uv_client::registry_client::simple_api package=setuptools
             uv_client::cached_client::get_cacheable
               uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/setuptools.rkyv
         uv_resolver::resolver::process_request request=Prefetch setuptools >=40.8.0
         uv_resolver::resolver::process_request request=Prefetch wheel *
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/wheel.rkyv"
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/setuptools.rkyv"
                  1.572065s   1ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/wheel/
                  1.572071s   1ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/wheel/
               uv_client::cached_client::revalidation_request url="https://pypi.org/simple/wheel/"
                  1.574021s   3ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/setuptools/
                  1.574026s   3ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/setuptools/
               uv_client::cached_client::revalidation_request url="https://pypi.org/simple/setuptools/"
                  1.590939s  20ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/wheel/
               uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/wheel.rkyv
           uv_resolver::version_map::from_metadata
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=wheel==0.42.0
             uv_client::registry_client::wheel_metadata built_dist=wheel==0.42.0
               uv_client::cached_client::get_serde
                 uv_client::cached_client::get_cacheable
                   uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/wheel/wheel-0.42.0-py3-none-any.msgpack
                1.591573s  21ms DEBUG uv_resolver::resolver Searching for a compatible version of wheel (*)
                1.591581s  21ms DEBUG uv_resolver::resolver Selecting: wheel==0.42.0 (wheel-0.42.0-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/wheel/wheel-0.42.0-py3-none-any.msgpack"
           uv_resolver::resolver::get_dependencies package=wheel, version=0.42.0
             uv_resolver::resolver::distributions_wait package_id=wheel-0.42.0
                      1.591738s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/c7/c3/55076fc728723ef927521abaa1955213d094933dc36d4a2008d5101e1af5/wheel-0.42.0-py3-none-any.whl.metadata
           uv_resolver::resolver::choose_version package=setuptools
             uv_resolver::resolver::package_wait package_name=setuptools
                  1.595932s  25ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/setuptools/
               uv_client::cached_client::refresh_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/setuptools.rkyv
           uv_resolver::version_map::from_metadata
           uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==69.1.1
             uv_client::registry_client::wheel_metadata built_dist=setuptools==69.1.1
               uv_client::cached_client::get_serde
                 uv_client::cached_client::get_cacheable
                   uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/setuptools/setuptools-69.1.1-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/setuptools/setuptools-69.1.1-py3-none-any.msgpack"
                1.596909s   5ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (>=40.8.0)
                1.596921s   5ms DEBUG uv_resolver::resolver Selecting: setuptools==69.1.1 (setuptools-69.1.1-py3-none-any.whl)
           uv_resolver::resolver::get_dependencies package=setuptools, version=69.1.1
             uv_resolver::resolver::distributions_wait package_id=setuptools-69.1.1
                      1.597058s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/c0/7a/3da654f49c95d0cc6e9549a855b5818e66a917e852ec608e77550c8dc08b/setuptools-69.1.1-py3-none-any.whl.metadata
         uv_dispatch::install resolution="setuptools==69.1.1, wheel==0.42.0", venv="/Users/kyle/Library/Caches/uv/.tmpMnuMOi/.venv"
              1.597204s   0ms DEBUG uv_dispatch Installing in setuptools==69.1.1, wheel==0.42.0 in /Users/kyle/Library/Caches/uv/.tmpMnuMOi/.venv
              1.597445s   0ms DEBUG uv_installer::plan Requirement already cached: setuptools==69.1.1
              1.597536s   0ms DEBUG uv_installer::plan Requirement already cached: wheel==0.42.0
              1.597559s   0ms DEBUG uv_dispatch Installing build requirements: setuptools==69.1.1, wheel==0.42.0
           uv_installer::installer::install num_wheels=2
            1.602983s  34ms DEBUG uv_build Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
         uv_build::run_python_script script="get_requires_for_build_wheel", python_version=3.11.8
error: Failed to download and build: chumpy==0.70
  Caused by: Failed to build: chumpy==0.70
  Caused by: Build backend failed to determine extra requires with `build_wheel()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 9, in <module>
ModuleNotFoundError: No module named 'pip'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/kyle/Library/Caches/uv/.tmpMnuMOi/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/kyle/Library/Caches/uv/.tmpMnuMOi/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/Users/kyle/Library/Caches/uv/.tmpMnuMOi/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/kyle/Library/Caches/uv/.tmpMnuMOi/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 11, in <module>
ModuleNotFoundError: No module named 'pip'
---
```

Please let me know if you need any additional information or if there are any steps I can take to further investigate or mitigate this issue.

---

_Comment by @zanieb on 2024-03-06 21:14_

Hi! Thanks for the thorough write-up. ~You can use `uv venv --seed` to get `pip` in your environment which should unblock you.~

The root of the problem here is that `chumpy` requires `pip` in its `setup.py`

https://github.com/mattloper/chumpy/blob/51d5afd92a8ded3637553be8cef41f328a1c863a/setup.py#L8-L11

This is improper because `pip` is not guaranteed to be installed at this time. Since we don't include `pip` in our virtual environments by default, retrieving `chumpy`'s requirements via `setuptools` fails. I'd report this as a bug in `chumpy`'s setup file.


---

_Label `question` added by @zanieb on 2024-03-06 21:15_

---

_Comment by @charliermarsh on 2024-03-06 21:17_

Hmm, I don't _think_ `--seed` will work, since the build will take place in an isolated environment regardless.

---

_Comment by @kpeez on 2024-03-06 21:18_

I just tried with `uv venv --seed` and am running into the same issue. 

---

_Comment by @charliermarsh on 2024-03-06 21:20_

For what it's worth, this also fails with `pip` if you specify `--use-pep517`, which will be the default some day:

```
ViTPose on  main [?] via 🐍 v3.12.1
❯ pip install -e . --use-pep517
Obtaining file:///Users/crmarsh/workspace/puffin/ViTPose
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Collecting chumpy (from mmpose==0.24.0)
  Using cached chumpy-0.70.tar.gz (50 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [26 lines of output]
      Traceback (most recent call last):
        File "<string>", line 9, in <module>
      ModuleNotFoundError: No module named 'pip'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-3ntxg243/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-3ntxg243/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-3ntxg243/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-3ntxg243/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 11, in <module>
      ModuleNotFoundError: No module named 'pip'
      [end of output]
```

---

_Comment by @charliermarsh on 2024-03-06 21:21_

It really needs to be fixed in the package itself, but I'm trying to think of a reasonable workaround.

---

_Comment by @charliermarsh on 2024-03-06 21:50_

I think the best solution is for us to support `--no-build-isolation`.

---

_Comment by @kpeez on 2024-03-06 22:01_

Thank you both for the fast responses! I reported the issue on the `chumpy` repo.

---

_Comment by @charliermarsh on 2024-03-06 22:02_

Thanks! I appreciate the thorough issue. We'll try to come up with a reasonable workaround for this.

---

_Comment by @kpeez on 2024-03-06 22:14_

Of course! Thank you guys for making such an awesome tool 😄 

I have a related issue where `uv pip install` fails to install [scikit-fmm](https://github.com/scikit-fmm/scikit-fmm) but `pip install` inside the virtual environment does not (if this should be its own separate issue I'm happy to move it). 

This works:
```bash
uv venv --seed
source .venv/bin/activate
pip install scikit-fmm # works with or without --use-pep517
```

but this does not:
```bash
uv venv
uv pip install scikit-fmm
```

## Traceback
Here is the traceback for the failed installation attempt (using --verbose):

```
 uv::requirements::from_source source=scikit-fmm
    0.000635s DEBUG uv_interpreter::python_environment Found a virtualenv named .venv at: /Users/kyle/Desktop/demo/.venv
    0.000732s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.11.8, skipping probing: /Users/kyle/Desktop/demo/.venv/bin/python
    0.000739s DEBUG uv::commands::pip_install Using Python 3.11.8 environment at /Users/kyle/Desktop/demo/.venv/bin/python
    0.000990s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.148756s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.8
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.152075s   0ms DEBUG uv_resolver::resolver Adding direct dependency: scikit-fmm*
   uv_resolver::resolver::choose_version package=scikit-fmm
     uv_resolver::resolver::package_wait package_name=scikit-fmm
 uv_resolver::resolver::process_request request=Versions scikit-fmm
   uv_client::registry_client::simple_api package=scikit-fmm
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/scikit-fmm.rkyv
 uv_resolver::resolver::process_request request=Prefetch scikit-fmm *
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/scikit-fmm.rkyv"
          0.152728s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/scikit-fmm/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=scikit-fmm==2023.4.2
     uv_client::registry_client::wheel_metadata built_dist=scikit-fmm==2023.4.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/scikit-fmm/scikit_fmm-2023.4.2-cp311-cp311-win_amd64.msgpack
        0.153080s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of scikit-fmm (*)
        0.153084s   0ms DEBUG uv_resolver::resolver Selecting: scikit-fmm==2023.4.2 (scikit_fmm-2023.4.2-cp311-cp311-win_amd64.whl)
   uv_resolver::resolver::get_dependencies package=scikit-fmm, version=2023.4.2
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/scikit-fmm/scikit_fmm-2023.4.2-cp311-cp311-win_amd64.msgpack"
     uv_resolver::resolver::distributions_wait package_id=scikit-fmm-2023.4.2
              0.153150s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/4c/df/2fe2e2676a06ecbfafebae47b699d22d04ed965e4ab2b4f62a83d36b0460/scikit_fmm-2023.4.2-cp311-cp311-win_amd64.whl.metadata
        0.153195s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: numpy>=1.0.2
   uv_resolver::resolver::choose_version package=numpy
     uv_resolver::resolver::package_wait package_name=numpy
 uv_resolver::resolver::process_request request=Versions numpy
   uv_client::registry_client::simple_api package=numpy
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/numpy.rkyv
 uv_resolver::resolver::process_request request=Prefetch numpy >=1.0.2
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/numpy.rkyv"
          0.153786s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/numpy/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=numpy==1.26.4
     uv_client::registry_client::wheel_metadata built_dist=numpy==1.26.4
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/numpy/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.msgpack
        0.154469s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of numpy (>=1.0.2)
        0.154473s   1ms DEBUG uv_resolver::resolver Selecting: numpy==1.26.4 (numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl)
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/numpy/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.msgpack"
   uv_resolver::resolver::get_dependencies package=numpy, version=1.26.4
     uv_resolver::resolver::distributions_wait package_id=numpy-1.26.4
              0.154542s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata
Resolved 2 packages in 5ms
    0.154834s DEBUG uv_installer::plan Requirement already cached: numpy==1.26.4
    0.154938s DEBUG uv_installer::plan Identified uncached requirement: scikit-fmm ==2023.4.2
    0.154984s DEBUG uv_installer::plan Unnecessary package: wheel==0.42.0
    0.154988s DEBUG uv_installer::plan Unnecessary package: setuptools==69.1.1
    0.154989s DEBUG uv_installer::plan Unnecessary package: pip==24.0
 uv_installer::downloader::download total=1
   uv_installer::downloader::get_wheel name=scikit-fmm==2023.4.2, size=Some(435123), url="https://files.pythonhosted.org/packages/27/30/5c1026ada50c5a48ea92fbb3788015c28e28d46f6eb1d45d3a900c255e46/scikit-fmm-2023.4.2.tar.gz"
     uv_distribution::distribution_database::get_or_build_wheel dist=Source(Registry(RegistrySourceDist { filename: SourceDistFilename { name: PackageName("scikit-fmm"), version: "2023.4.2", extension: TarGz }, file: File { dist_info_metadata: Some(Bool(false)), filename: "scikit-fmm-2023.4.2.tar.gz", hashes: Hashes { md5: None, sha256: Some("d7871c47f820772aba92f25651ef14e4d27de8ce393c0ae3c02ab3c6e2423c0b") }, requires_python: None, size: Some(435123), upload_time_utc_ms: Some(1680531380652), url: AbsoluteUrl("https://files.pythonhosted.org/packages/27/30/5c1026ada50c5a48ea92fbb3788015c28e28d46f6eb1d45d3a900c255e46/scikit-fmm-2023.4.2.tar.gz"), yanked: Some(Bool(false)) }, index: Pypi(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }) }))
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/built-wheels-v0/pypi/scikit-fmm/2023.4.2/manifest.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/built-wheels-v0/pypi/scikit-fmm/2023.4.2/manifest.msgpack"
              0.155136s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/27/30/5c1026ada50c5a48ea92fbb3788015c28e28d46f6eb1d45d3a900c255e46/scikit-fmm-2023.4.2.tar.gz
       uv_distribution::source::build_source_dist
            0.155172s   0ms DEBUG uv_distribution::source Building: scikit-fmm==2023.4.2
         uv_dispatch::setup_build package_id="scikit-fmm==2023.4.2", subdirectory=None
           uv_resolver::resolver::solve
                0.156693s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.11.8
             uv_resolver::resolver::choose_version package=root
             uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
                  0.156713s   0ms DEBUG uv_resolver::resolver Adding direct dependency: wheel*
                  0.156718s   0ms DEBUG uv_resolver::resolver Adding direct dependency: setuptools==64.0.0
                  0.156721s   0ms DEBUG uv_resolver::resolver Adding direct dependency: oldest-supported-numpy*
             uv_resolver::resolver::choose_version package=wheel
               uv_resolver::resolver::package_wait package_name=wheel
           uv_resolver::resolver::process_request request=Versions wheel
             uv_client::registry_client::simple_api package=wheel
               uv_client::cached_client::get_cacheable
                 uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/wheel.rkyv
           uv_resolver::resolver::process_request request=Versions setuptools
             uv_client::registry_client::simple_api package=setuptools
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/wheel.rkyv"
               uv_client::cached_client::get_cacheable
                 uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/setuptools.rkyv
           uv_resolver::resolver::process_request request=Versions oldest-supported-numpy
             uv_client::registry_client::simple_api package=oldest-supported-numpy
               uv_client::cached_client::get_cacheable
                 uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/simple-v3/pypi/oldest-supported-numpy.rkyv
           uv_resolver::resolver::process_request request=Prefetch oldest-supported-numpy *
           uv_resolver::resolver::process_request request=Prefetch setuptools ==64.0.0
           uv_resolver::resolver::process_request request=Prefetch wheel *
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/oldest-supported-numpy.rkyv"
                    0.156870s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/wheel/
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/simple-v3/pypi/setuptools.rkyv"
             uv_resolver::version_map::from_metadata
                    0.156950s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/oldest-supported-numpy/
             uv_resolver::version_map::from_metadata
             uv_distribution::distribution_database::get_or_build_wheel_metadata dist=wheel==0.42.0
               uv_client::registry_client::wheel_metadata built_dist=wheel==0.42.0
                 uv_client::cached_client::get_serde
                   uv_client::cached_client::get_cacheable
                     uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/wheel/wheel-0.42.0-py3-none-any.msgpack
             uv_distribution::distribution_database::get_or_build_wheel_metadata dist=oldest-supported-numpy==2023.12.21
               uv_client::registry_client::wheel_metadata built_dist=oldest-supported-numpy==2023.12.21
                 uv_client::cached_client::get_serde
                   uv_client::cached_client::get_cacheable
                     uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/oldest-supported-numpy/oldest_supported_numpy-2023.12.21-py3-none-any.msgpack
                  0.157047s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of wheel (*)
                  0.157053s   0ms DEBUG uv_resolver::resolver Selecting: wheel==0.42.0 (wheel-0.42.0-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/oldest-supported-numpy/oldest_supported_numpy-2023.12.21-py3-none-any.msgpack"
             uv_resolver::resolver::get_dependencies package=wheel, version=0.42.0
               uv_resolver::resolver::distributions_wait package_id=wheel-0.42.0
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/wheel/wheel-0.42.0-py3-none-any.msgpack"
                        0.157105s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/dc/5c/e3c84cfdd488701aa074b22cf5bd227fb15d26e1d55a66d9088c39afa123/oldest_supported_numpy-2023.12.21-py3-none-any.whl.metadata
                        0.157165s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/c7/c3/55076fc728723ef927521abaa1955213d094933dc36d4a2008d5101e1af5/wheel-0.42.0-py3-none-any.whl.metadata
             uv_resolver::resolver::choose_version package=setuptools
               uv_resolver::resolver::package_wait package_name=setuptools
                    0.157325s   0ms DEBUG uv_client::cached_client Found fresh response for: https://pypi.org/simple/setuptools/
             uv_resolver::version_map::from_metadata
             uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==64.0.0
               uv_client::registry_client::wheel_metadata built_dist=setuptools==64.0.0
                 uv_client::cached_client::get_serde
                   uv_client::cached_client::get_cacheable
                     uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/setuptools/setuptools-64.0.0-py3-none-any.msgpack
                  0.157667s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (==64.0.0)
                  0.157671s   0ms DEBUG uv_resolver::resolver Selecting: setuptools==64.0.0 (setuptools-64.0.0-py3-none-any.whl)
             uv_resolver::resolver::get_dependencies package=setuptools, version=64.0.0
               uv_resolver::resolver::distributions_wait package_id=setuptools-64.0.0
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/setuptools/setuptools-64.0.0-py3-none-any.msgpack"
                        0.157731s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/3e/83/e206edff58159a927c76bbfeef1bf8b39cb12bbb32ae3c6227deb16d0121/setuptools-64.0.0-py3-none-any.whl.metadata
             uv_resolver::resolver::choose_version package=oldest-supported-numpy
               uv_resolver::resolver::package_wait package_name=oldest-supported-numpy
                  0.157775s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of oldest-supported-numpy (*)
                  0.157778s   0ms DEBUG uv_resolver::resolver Selecting: oldest-supported-numpy==2023.12.21 (oldest_supported_numpy-2023.12.21-py3-none-any.whl)
             uv_resolver::resolver::get_dependencies package=oldest-supported-numpy, version=2023.12.21
               uv_resolver::resolver::distributions_wait package_id=oldest-supported-numpy-2023.12.21
                  0.157792s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: numpy==1.23.2
             uv_resolver::resolver::choose_version package=numpy
               uv_resolver::resolver::package_wait package_name=numpy
                  0.157805s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of numpy (==1.23.2)
                  0.157830s   0ms DEBUG uv_resolver::resolver Selecting: numpy==1.23.2 (numpy-1.23.2-cp311-cp311-macosx_11_0_arm64.whl)
             uv_resolver::resolver::get_dependencies package=numpy, version=1.23.2
               uv_resolver::resolver::distributions_wait package_id=numpy-1.23.2
           uv_resolver::resolver::process_request request=Prefetch numpy ==1.23.2
           uv_resolver::resolver::process_request request=Metadata numpy==1.23.2
             uv_distribution::distribution_database::get_or_build_wheel_metadata dist=numpy==1.23.2
               uv_client::registry_client::wheel_metadata built_dist=numpy==1.23.2
                 uv_client::cached_client::get_serde
                   uv_client::cached_client::get_cacheable
                     uv_client::cached_client::read_and_parse_cache file=/Users/kyle/Library/Caches/uv/wheels-v0/pypi/numpy/numpy-1.23.2-cp311-cp311-macosx_11_0_arm64.msgpack
 uv_client::cached_client::from_path_sync path="/Users/kyle/Library/Caches/uv/wheels-v0/pypi/numpy/numpy-1.23.2-cp311-cp311-macosx_11_0_arm64.msgpack"
                        0.157927s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/10/8e/843caee5e70d9edb8b01dc9418edbf475200abde5299136683006ed2d58b/numpy-1.23.2-cp311-cp311-macosx_11_0_arm64.whl.metadata
           uv_dispatch::install resolution="numpy==1.23.2, setuptools==64.0.0, oldest-supported-numpy==2023.12.21, wheel==0.42.0", venv="/Users/kyle/Library/Caches/uv/.tmpfvmESL/.venv"
                0.157978s   0ms DEBUG uv_dispatch Installing in numpy==1.23.2, setuptools==64.0.0, oldest-supported-numpy==2023.12.21, wheel==0.42.0 in /Users/kyle/Library/Caches/uv/.tmpfvmESL/.venv
                0.158111s   0ms DEBUG uv_installer::plan Requirement already cached: numpy==1.23.2
                0.158174s   0ms DEBUG uv_installer::plan Requirement already cached: oldest-supported-numpy==2023.12.21
                0.158261s   0ms DEBUG uv_installer::plan Requirement already cached: setuptools==64.0.0
                0.158326s   0ms DEBUG uv_installer::plan Requirement already cached: wheel==0.42.0
                0.158336s   0ms DEBUG uv_dispatch Installing build requirements: numpy==1.23.2, oldest-supported-numpy==2023.12.21, setuptools==64.0.0, wheel==0.42.0
             uv_installer::installer::install num_wheels=4
              0.171650s  16ms DEBUG uv_build Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
           uv_build::run_python_script script="get_requires_for_build_wheel", python_version=3.11.8
         uv_build::build package_id="scikit-fmm==2023.4.2"
              0.498062s   0ms DEBUG uv_build Calling `setuptools.build_meta:__legacy__.build_wheel(metadata_directory=None)`
           uv_build::run_python_script script="build_wheel", python_version=3.11.8
error: Failed to download distributions
  Caused by: Failed to fetch wheel: scikit-fmm==2023.4.2
  Caused by: Failed to build: scikit-fmm==2023.4.2
  Caused by: Build backend failed to build wheel through `build_wheel()`:
--- stdout:
running bdist_wheel
running build
running config_cc
INFO: unifing config_cc, config, build_clib, build_ext, build commands --compiler options
running config_fc
INFO: unifing config_fc, config, build_clib, build_ext, build commands --fcompiler options
running build_src
INFO: build_src
INFO: building extension "skfmm.cfmm" sources
INFO: building extension "skfmm.pheap" sources
INFO: build_src: building npy-pkg config files
running build_py
running build_ext
INFO: customize UnixCCompiler
INFO: customize UnixCCompiler using build_ext
INFO: CCompilerOpt.__init__[813] : load cache from file -> /Users/kyle/Library/Caches/uv/built-wheels-v0/pypi/scikit-fmm/2023.4.2/cjbk5_ptjiZMvk2IgkOtT/scikit-fmm-2023.4.2.tar.gz/build/temp.macosx-14.3-arm64-cpython-311/ccompiler_opt_cache_ext.py
INFO: CCompilerOpt.__init__[824] : hit the file cache
INFO: customize UnixCCompiler
WARN: #### ['clang', '-Wsign-compare', '-Wunreachable-code', '-DNDEBUG', '-g', '-fwrapv', '-O3', '-Wall'] #######
INFO: customize UnixCCompiler using build_ext
installing to build/bdist.macosx-14.3-arm64/wheel
running install
running install_lib
creating build/bdist.macosx-14.3-arm64/wheel
creating build/bdist.macosx-14.3-arm64/wheel/skfmm
copying build/lib.macosx-14.3-arm64-cpython-311/skfmm/pheap.cpython-311-darwin.so -> build/bdist.macosx-14.3-arm64/wheel/skfmm
copying build/lib.macosx-14.3-arm64-cpython-311/skfmm/heap.py -> build/bdist.macosx-14.3-arm64/wheel/skfmm
copying build/lib.macosx-14.3-arm64-cpython-311/skfmm/pfmm.py -> build/bdist.macosx-14.3-arm64/wheel/skfmm
copying build/lib.macosx-14.3-arm64-cpython-311/skfmm/__init__.py -> build/bdist.macosx-14.3-arm64/wheel/skfmm
copying build/lib.macosx-14.3-arm64-cpython-311/skfmm/setup.py -> build/bdist.macosx-14.3-arm64/wheel/skfmm
copying build/lib.macosx-14.3-arm64-cpython-311/skfmm/cfmm.cpython-311-darwin.so -> build/bdist.macosx-14.3-arm64/wheel/skfmm
running install_egg_info
running egg_info
writing scikit_fmm.egg-info/PKG-INFO
writing dependency_links to scikit_fmm.egg-info/dependency_links.txt
writing requirements to scikit_fmm.egg-info/requires.txt
writing top-level names to scikit_fmm.egg-info/top_level.txt
reading manifest file 'scikit_fmm.egg-info/SOURCES.txt'
reading manifest template 'MANIFEST.in'
warning: no files found matching 'README.txt'
adding license file 'LICENSE.txt'
writing manifest file 'scikit_fmm.egg-info/SOURCES.txt'
Copying scikit_fmm.egg-info to build/bdist.macosx-14.3-arm64/wheel/scikit_fmm-2023.4.2-py3.11.egg-info
running install_scripts
running install_clib
INFO: customize UnixCCompiler
creating build/bdist.macosx-14.3-arm64/wheel/scikit_fmm-2023.4.2.dist-info/WHEEL
creating '/Users/kyle/Library/Caches/uv/built-wheels-v0/pypi/scikit-fmm/2023.4.2/cjbk5_ptjiZMvk2IgkOtT/.tmpEg7gWW/tmp3wpenwga/scikit_fmm-2023.4.2-cp311-cp311-macosx_14_0_arm64.whl' and adding 'build/bdist.macosx-14.3-arm64/wheel' to it
adding 'skfmm/__init__.py'
adding 'skfmm/cfmm.cpython-311-darwin.so'
adding 'skfmm/heap.py'
adding 'skfmm/pfmm.py'
adding 'skfmm/pheap.cpython-311-darwin.so'
adding 'skfmm/setup.py'
adding 'scikit_fmm-2023.4.2.dist-info/LICENSE.txt'
adding 'scikit_fmm-2023.4.2.dist-info/METADATA'
adding 'scikit_fmm-2023.4.2.dist-info/WHEEL'
adding 'scikit_fmm-2023.4.2.dist-info/top_level.txt'
adding 'scikit_fmm-2023.4.2.dist-info/RECORD'
removing build/bdist.macosx-14.3-arm64/wheel
scikit_fmm-2023.4.2-cp311-cp311-macosx_14_0_arm64.whl
INFO:
########### EXT COMPILER OPTIMIZATION ###########
INFO: Platform      :
  Architecture: aarch64
  Compiler    : clang

CPU baseline  :
  Requested   : 'min'
  Enabled     : NEON NEON_FP16 NEON_VFPV4 ASIMD
  Flags       : none
  Extra checks: none

CPU dispatch  :
  Requested   : 'max -xop -fma4'
  Enabled     : ASIMDHP ASIMDDP ASIMDFHM
  Generated   : none
INFO: CCompilerOpt.cache_flush[857] : write cache to path -> /Users/kyle/Library/Caches/uv/built-wheels-v0/pypi/scikit-fmm/2023.4.2/cjbk5_ptjiZMvk2IgkOtT/scikit-fmm-2023.4.2.tar.gz/build/temp.macosx-14.3-arm64-cpython-311/ccompiler_opt_cache_ext.py
--- stderr:
<string>:115: DeprecationWarning:

  `numpy.distutils` is deprecated since NumPy 1.23.0, as a result
  of the deprecation of `distutils` itself. It will be removed for
  Python >= 3.12. For older Python versions it will remain present.
  It is recommended to use `setuptools < 60.0` for those Python versions.
  For more details, see:
    https://numpy.org/devdocs/reference/distutils_status_migration.html


/Users/kyle/Library/Caches/uv/.tmpfvmESL/.venv/lib/python3.11/site-packages/setuptools/dist.py:530: UserWarning: Normalizing '2023.04.02' to '2023.4.2'
  warnings.warn(tmpl.format(**locals()))
/Users/kyle/Library/Caches/uv/.tmpfvmESL/.venv/lib/python3.11/site-packages/setuptools/command/egg_info.py:643: SetuptoolsDeprecationWarning: Custom 'build_py' does not implement 'get_data_files_without_manifest'.
Please extend command classes from setuptools instead of distutils.
  warnings.warn(
---
```


---

_Comment by @charliermarsh on 2024-03-07 23:51_

@kpeez - You can now install this using `--no-build-isolation`. This effectively requires that _you_ install the build dependencies upfront, and then allows builds to access those dependencies:

```
# Create virtualenv...
uv venv

# Install build dependencies...
uv pip install setuptools wheel cython numpy

# Install mmpose.
uv pip install "mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose"
```

---

_Comment by @charliermarsh on 2024-03-07 23:51_

Got this to work on my side:

```
❯ uv pip install "mmpose @ git+https://github.com/ViTAE-Transformer/ViTPose" --no-build-isolation
 Updated https://github.com/ViTAE-Transformer/ViTPose (d521645)                                                                                                                                                                               Resolved 30 packages in 665ms
   Built xtcocotools==1.14.3                                                                                                                                                                                                                  Downloaded 3 packages in 2.57s
Installed 27 packages in 240ms
 + chumpy==0.70
 + contourpy==1.2.0
 + cycler==0.12.1
 + filelock==3.13.1
 + fonttools==4.49.0
 + fsspec==2024.2.0
 + jinja2==3.1.3
 + json-tricks==3.17.3
 + kiwisolver==1.4.5
 + markupsafe==2.1.5
 + matplotlib==3.8.3
 + mmpose==0.24.0 (from git+https://github.com/ViTAE-Transformer/ViTPose@d5216452796c90c6bc29f5c5ec0bdba94366768a)
 + mpmath==1.3.0
 + munkres==1.1.4
 + networkx==3.2.1
 + opencv-python==4.9.0.80
 + packaging==23.2
 + pillow==10.2.0
 + pyparsing==3.1.2
 + python-dateutil==2.9.0.post0
 + scipy==1.12.0
 + six==1.16.0
 + sympy==1.12
 + torch==2.2.1
 + torchvision==0.17.1
 + typing-extensions==4.10.0
 + xtcocotools==1.14.3
```


---

_Closed by @charliermarsh on 2024-03-07 23:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 23:51_

---

_Comment by @kpeez on 2024-03-07 23:57_

@charliermarsh also works on my machine now!

---

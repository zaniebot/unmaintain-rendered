---
number: 11554
title: uv does not seem to install so files of coremltools
type: issue
state: open
author: twoertwein
labels:
  - needs-mre
assignees: []
created_at: 2025-02-16T14:33:59Z
updated_at: 2025-06-22T02:26:16Z
url: https://github.com/astral-sh/uv/issues/11554
synced_at: 2026-01-07T13:12:18-06:00
---

# uv does not seem to install so files of coremltools

---

_Issue opened by @twoertwein on 2025-02-16 14:33_

### Summary

```sh
$ uv init example
$ cd example
$ uv python pin 3.11
$ uv add coremltools
$ uv run python -c "import coremltools"
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLCPUComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLGPUComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLNeuralEngineComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLComputePlanProxy: No module named 'coremltools.libcoremlpython'
Fail to import BlobReader from libmilstoragepython. No module named 'coremltools.libmilstoragepython'
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLModelAssetProxy: No module named 'coremltools.libcoremlpython'
```

The uv installation folder of coremltools does not contain the so files (exist in the pip installation)

uv installation:
```
__init__.py	__pycache__	_deps		converters	models		optimize	proto		test		version.py
```

pip installation:
```
__init__.py		_deps			libcoremlpython.so	libmodelpackage.so	optimize		test
__pycache__		converters		libmilstoragepython.so	models			proto			version.py
```

### Platform

mac M2

### Version

0.5.31

### Python version

3.11

---

_Label `bug` added by @twoertwein on 2025-02-16 14:33_

---

_Comment by @zanieb on 2025-02-16 15:34_

How did you install with `pip`?

```
❯ uv venv
Using CPython 3.13.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install coremlutils
  × No solution found when resolving dependencies:
  ╰─▶ Because coremlutils was not found in the package registry and you require coremlutils, we can conclude that your requirements are unsatisfiable.
❯ uv pip install coremltools==8.2
Resolved 11 packages in 312ms
      Built coremltools==8.2
Prepared 10 packages in 1.76s
Installed 11 packages in 57ms
 + attrs==25.1.0
 + cattrs==24.1.2
 + coremltools==8.2
 + mpmath==1.3.0
 + numpy==2.2.3
 + packaging==24.2
 + protobuf==5.29.3
 + pyaml==25.1.0
 + pyyaml==6.0.2
 + sympy==1.13.3
 + tqdm==4.67.1
❯ ls .venv/lib/python3.13/site-packages/coremltools
__init__.py	_deps		converters	models		optimize	proto		test		version.py
❯ uv pip install pip
Resolved 1 package in 96ms
Prepared 1 package in 193ms
Installed 1 package in 6ms
 + pip==25.0.1
❯ .venv/bin/pip install coremltools==8.2 --force-reinstall
Collecting coremltools==8.2
  Downloading coremltools-8.2.tar.gz (1.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.5/1.5 MB 10.3 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting numpy>=1.14.5 (from coremltools==8.2)
  Downloading numpy-2.2.3-cp313-cp313-macosx_14_0_arm64.whl.metadata (62 kB)
Collecting protobuf>=3.1.0 (from coremltools==8.2)
  Downloading protobuf-5.29.3-cp38-abi3-macosx_10_9_universal2.whl.metadata (592 bytes)
Collecting sympy (from coremltools==8.2)
  Downloading sympy-1.13.3-py3-none-any.whl.metadata (12 kB)
Collecting tqdm (from coremltools==8.2)
  Downloading tqdm-4.67.1-py3-none-any.whl.metadata (57 kB)
Collecting packaging (from coremltools==8.2)
  Using cached packaging-24.2-py3-none-any.whl.metadata (3.2 kB)
Collecting attrs>=21.3.0 (from coremltools==8.2)
  Downloading attrs-25.1.0-py3-none-any.whl.metadata (10 kB)
Collecting cattrs (from coremltools==8.2)
  Downloading cattrs-24.1.2-py3-none-any.whl.metadata (8.4 kB)
Collecting pyaml (from coremltools==8.2)
  Downloading pyaml-25.1.0-py3-none-any.whl.metadata (12 kB)
Collecting PyYAML (from pyaml->coremltools==8.2)
  Downloading PyYAML-6.0.2-cp313-cp313-macosx_11_0_arm64.whl.metadata (2.1 kB)
Collecting mpmath<1.4,>=1.1.0 (from sympy->coremltools==8.2)
  Downloading mpmath-1.3.0-py3-none-any.whl.metadata (8.6 kB)
Downloading attrs-25.1.0-py3-none-any.whl (63 kB)
Downloading numpy-2.2.3-cp313-cp313-macosx_14_0_arm64.whl (5.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.1/5.1 MB 41.6 MB/s eta 0:00:00
Downloading protobuf-5.29.3-cp38-abi3-macosx_10_9_universal2.whl (417 kB)
Downloading cattrs-24.1.2-py3-none-any.whl (66 kB)
Using cached packaging-24.2-py3-none-any.whl (65 kB)
Downloading pyaml-25.1.0-py3-none-any.whl (26 kB)
Downloading sympy-1.13.3-py3-none-any.whl (6.2 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.2/6.2 MB 42.4 MB/s eta 0:00:00
Downloading tqdm-4.67.1-py3-none-any.whl (78 kB)
Downloading mpmath-1.3.0-py3-none-any.whl (536 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 536.2/536.2 kB 18.4 MB/s eta 0:00:00
Using cached PyYAML-6.0.2-cp313-cp313-macosx_11_0_arm64.whl (171 kB)
Building wheels for collected packages: coremltools
  Building wheel for coremltools (pyproject.toml) ... done
  Created wheel for coremltools: filename=coremltools-8.2-py3-none-any.whl size=1917235 sha256=9215c0980a14d0423cc4d62282e13fae87828ad8fc1b7dc7eb58755649044740
  Stored in directory: /Users/zb/Library/Caches/pip/wheels/13/94/ec/2840f5638896f3cedea11a64987334f9cedffc1be409617244
Successfully built coremltools
Installing collected packages: mpmath, tqdm, sympy, PyYAML, protobuf, packaging, numpy, attrs, pyaml, cattrs, coremltools
  Attempting uninstall: mpmath
    Found existing installation: mpmath 1.3.0
    Uninstalling mpmath-1.3.0:
      Successfully uninstalled mpmath-1.3.0
  Attempting uninstall: tqdm
    Found existing installation: tqdm 4.67.1
    Uninstalling tqdm-4.67.1:
      Successfully uninstalled tqdm-4.67.1
  Attempting uninstall: sympy
    Found existing installation: sympy 1.13.3
    Uninstalling sympy-1.13.3:
      Successfully uninstalled sympy-1.13.3
  Attempting uninstall: PyYAML
    Found existing installation: PyYAML 6.0.2
    Uninstalling PyYAML-6.0.2:
      Successfully uninstalled PyYAML-6.0.2
  Attempting uninstall: protobuf
    Found existing installation: protobuf 5.29.3
    Uninstalling protobuf-5.29.3:
      Successfully uninstalled protobuf-5.29.3
  Attempting uninstall: packaging
    Found existing installation: packaging 24.2
    Uninstalling packaging-24.2:
      Successfully uninstalled packaging-24.2
  Attempting uninstall: numpy
    Found existing installation: numpy 2.2.3
    Uninstalling numpy-2.2.3:
      Successfully uninstalled numpy-2.2.3
  Attempting uninstall: attrs
    Found existing installation: attrs 25.1.0
    Uninstalling attrs-25.1.0:
      Successfully uninstalled attrs-25.1.0
  Attempting uninstall: pyaml
    Found existing installation: pyaml 25.1.0
    Uninstalling pyaml-25.1.0:
      Successfully uninstalled pyaml-25.1.0
  Attempting uninstall: cattrs
    Found existing installation: cattrs 24.1.2
    Uninstalling cattrs-24.1.2:
      Successfully uninstalled cattrs-24.1.2
  Attempting uninstall: coremltools
    Found existing installation: coremltools 8.2
    Uninstalling coremltools-8.2:
      Successfully uninstalled coremltools-8.2
Successfully installed PyYAML-6.0.2 attrs-25.1.0 cattrs-24.1.2 coremltools-8.2 mpmath-1.3.0 numpy-2.2.3 packaging-24.2 protobuf-5.29.3 pyaml-25.1.0 sympy-1.13.3 tqdm-4.67.1
❯ ls .venv/lib/python3.13/site-packages/coremltools
__init__.py	__pycache__	_deps		converters	models		optimize	proto		test		version.py
❯ .venv/bin/python -c "import coremltools"
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLCPUComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLGPUComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLNeuralEngineComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLComputePlanProxy: No module named 'coremltools.libcoremlpython'
Fail to import BlobReader from libmilstoragepython. No module named 'coremltools.libmilstoragepython'
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLModelAssetProxy: No module named 'coremltools.libcoremlpython'
```


---

_Comment by @twoertwein on 2025-02-16 15:42_

I'm not sure if coremtools supports 3.12/13.

```
$ python --version
Python 3.11.10

$ python -m pip install coremltools
Collecting coremltools
  Using cached coremltools-8.2-cp311-none-macosx_11_0_arm64.whl.metadata (2.5 kB)
Requirement already satisfied: numpy>=1.14.5 in ./miniforge3/lib/python3.11/site-packages (from coremltools) (1.26.4)
Requirement already satisfied: protobuf>=3.1.0 in ./miniforge3/lib/python3.11/site-packages (from coremltools) (4.25.5)
Requirement already satisfied: sympy in ./miniforge3/lib/python3.11/site-packages (from coremltools) (1.13.1)
Requirement already satisfied: tqdm in ./miniforge3/lib/python3.11/site-packages (from coremltools) (4.67.1)
Requirement already satisfied: packaging in ./miniforge3/lib/python3.11/site-packages (from coremltools) (24.0)
Requirement already satisfied: attrs>=21.3.0 in ./miniforge3/lib/python3.11/site-packages (from coremltools) (24.2.0)
Requirement already satisfied: cattrs in ./miniforge3/lib/python3.11/site-packages (from coremltools) (23.2.3)
Requirement already satisfied: pyaml in ./miniforge3/lib/python3.11/site-packages (from coremltools) (24.7.0)
Requirement already satisfied: PyYAML in ./miniforge3/lib/python3.11/site-packages (from pyaml->coremltools) (6.0.2)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in ./miniforge3/lib/python3.11/site-packages (from sympy->coremltools) (1.3.0)
Using cached coremltools-8.2-cp311-none-macosx_11_0_arm64.whl (2.6 MB)
Installing collected packages: coremltools
Successfully installed coremltools-8.2
```


---

_Comment by @zanieb on 2025-02-16 15:49_

Ah okay I _can_ reproduce this on Python 3.11. Confusing.

---

_Comment by @zanieb on 2025-02-16 16:08_

Debugging now...

These files are requested in a setuptools `package_data` blurb https://github.com/apple/coremltools/blob/605ac1c7f06c19a09853e1757f7f3379d7d4e9fd/setup.py#L71-L80

We're installing this wheel https://inspector.pypi.io/project/coremltools/8.2/packages/0b/ac/2431ce0270fcf60a8f68d19475b7406754016dcdac9986b742df16339dca/coremltools-8.2-cp312-none-macosx_11_0_arm64.whl/

Which does include the `.so` files.

They're also present in the RECORD 
https://inspector.pypi.io/project/coremltools/8.2/packages/0b/ac/2431ce0270fcf60a8f68d19475b7406754016dcdac9986b742df16339dca/coremltools-8.2-cp312-none-macosx_11_0_arm64.whl/coremltools-8.2.dist-info/RECORD#line.2

If we inspect our cache entry for the wheel, the `.so` files are present!

```
❯ ls /Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools
__init__.py		converters		libmilstoragepython.so	models			proto			version.py
_deps			libcoremlpython.so	libmodelpackage.so	optimize		test
```

We call

https://github.com/astral-sh/uv/blob/35564e189ddcac3b514984bd5ca62c376533f1e0/crates/uv-install-wheel/src/install.rs#L78-L79

Which logs

```
TRACE Extracted 2 files name=PackageName("coremltools")
```

This is surprising at first, but on macOS we can clone whole directories. With some more logs..

```
TRACE Cloning /Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools to /Users/zb/workspace/uv/example/.venv/lib/python3.11/site-packages/coremltools
TRACE Cloning /Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools-8.2.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.11/site-packages/coremltools-8.2.dist-info
````

So... it seems like APFS isn't cloning the `.so` file correctly?

To confirm, we can install with the `copy` link mode and things are working as intended

```
❯ uv venv -p 3.11
Using CPython 3.11.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install coremltools==8.2 --link-mode copy
Resolved 11 packages in 17ms
Installed 11 packages in 177ms
 + attrs==25.1.0
 + cattrs==24.1.2
 + coremltools==8.2
 + mpmath==1.3.0
 + numpy==2.2.3
 + packaging==24.2
 + protobuf==5.29.3
 + pyaml==25.1.0
 + pyyaml==6.0.2
 + sympy==1.13.3
 + tqdm==4.67.1
❯ ls .venv/lib/python3.11/site-packages/coremltools
__init__.py		converters		libmilstoragepython.so	models			proto			version.py
_deps			libcoremlpython.so	libmodelpackage.so	optimize		test
```

As to why APFS does not behave correctly here, I have no clue.

This does not reproduce with `cp`

```
❯ cp -v -Rc /Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools ./coremltools | grep "\.so"
/Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools/libcoremlpython.so -> ./coremltools/coremltools/libcoremlpython.so
/Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools/libmodelpackage.so -> ./coremltools/coremltools/libmodelpackage.so
/Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools/_deps/kmeans1d/_core.cpython-311-darwin.so -> ./coremltools/coremltools/_deps/kmeans1d/_core.cpython-311-darwin.so
/Users/zb/.cache/uv/archive-v0/_UMdwU9bOmVY6tYSHemYJ/coremltools/libmilstoragepython.so -> ./coremltools/coremltools/libmilstoragepython.so
❯ ls coremltools
__init__.py		converters		libcoremlpython.so	libmodelpackage.so	optimize		test
_deps			coremltools		libmilstoragepython.so	models			proto			version.py
```

However, I suspect that `cp` is recursively calling `clonefile` per file instead of once for the directory since it logs each file.

The Rust code is very minimal (from `reflink-copy`)

```rust
pub fn reflink(from: &Path, to: &Path) -> io::Result<()> {
    let src = cstr(from)?;
    let dest = cstr(to)?;

    let ret = unsafe { clonefile(src.as_ptr(), dest.as_ptr(), CLONE_NOOWNERCOPY) };

    if ret == -1 {
        Err(io::Error::last_os_error())
    } else {
        Ok(())
    }
}
```

---

_Comment by @zanieb on 2025-02-16 16:32_

Okay now I cannot reproduce this?

```
❯ uv venv -p 3.11
Using CPython 3.11.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install coremltools==8.2 --no-cache
Resolved 11 packages in 272ms
Prepared 11 packages in 943ms
Installed 11 packages in 29ms
 + attrs==25.1.0
 + cattrs==24.1.2
 + coremltools==8.2
 + mpmath==1.3.0
 + numpy==2.2.3
 + packaging==24.2
 + protobuf==5.29.3
 + pyaml==25.1.0
 + pyyaml==6.0.2
 + sympy==1.13.3
 + tqdm==4.67.1
❯ ls .venv/lib/python3.11/site-packages/coremltools
__init__.py		libcoremlpython.so	models			test
_deps			libmilstoragepython.so	optimize		version.py
converters		libmodelpackage.so	proto
❯ uv venv -p 3.11
Using CPython 3.11.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install coremltools==8.2
Resolved 11 packages in 17ms
Installed 11 packages in 37ms
 + attrs==25.1.0
 + cattrs==24.1.2
 + coremltools==8.2
 + mpmath==1.3.0
 + numpy==2.2.3
 + packaging==24.2
 + protobuf==5.29.3
 + pyaml==25.1.0
 + pyyaml==6.0.2
 + sympy==1.13.3
 + tqdm==4.67.1
❯ ls .venv/lib/python3.11/site-packages/coremltools
__init__.py		libcoremlpython.so	models			test
_deps			libmilstoragepython.so	optimize		version.py
converters		libmodelpackage.so	proto
```

---

_Comment by @twoertwein on 2025-02-17 13:50_

Thank you for debugging this so thoroughly and for also finding a temporary work around :)

---

_Comment by @zanieb on 2025-02-17 14:35_

I'm at a loss as to how to fix it since I cannot reproduce it anymore :D

---

_Comment by @twoertwein on 2025-02-17 16:17_

After installing it with `--no-cache` it works but, I'm able to reprodcue it again after running (all inside the `uv venv -p 3.11`):

```sh
$ uv pip uninstall coremltools
Uninstalled 1 package in 175ms
 - coremltools==8.2

$ uv pip install coremltools          
Resolved 11 packages in 177ms
Installed 1 package in 19ms
 + coremltools==8.2

$ uv run python -c  "import coremltools"
Torch version 2.6.0 has not been tested with coremltools. You may run into unexpected errors. Torch 2.5.0 is the most recent version that has been tested.
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLCPUComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLGPUComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLNeuralEngineComputeDeviceProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLComputePlanProxy: No module named 'coremltools.libcoremlpython'
Fail to import BlobReader from libmilstoragepython. No module named 'coremltools.libmilstoragepython'
Failed to load _MLModelProxy: No module named 'coremltools.libcoremlpython'
Failed to load _MLModelAssetProxy: No module named 'coremltools.libcoremlpython'
Fail to import BlobWriter from libmilstoragepython. No module named 'coremltools.libmilstoragepython'
```

---

_Comment by @zanieb on 2025-02-17 16:25_

In my example, I was creating a new empty environment each time. I can try futzing with it more though.

```
❯ uv pip uninstall coremltools
Uninstalled 1 package in 34ms
 - coremltools==8.2
❯ uv pip install coremltools
Resolved 11 packages in 8ms
Installed 1 package in 9ms
 + coremltools==8.2
❯ ls .venv/lib/python3.11/site-packages/coremltools
__init__.py		converters		libmilstoragepython.so	models			proto			version.py
_deps			libcoremlpython.so	libmodelpackage.so	optimize		test
```

---

_Referenced in [astral-sh/uv#11579](../../astral-sh/uv/pulls/11579.md) on 2025-02-17 17:32_

---

_Referenced in [astral-sh/uv#12869](../../astral-sh/uv/issues/12869.md) on 2025-04-14 03:34_

---

_Label `bug` removed by @charliermarsh on 2025-06-22 02:26_

---

_Label `needs-mre` added by @charliermarsh on 2025-06-22 02:26_

---

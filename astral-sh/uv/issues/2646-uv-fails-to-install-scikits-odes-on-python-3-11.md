---
number: 2646
title: "`uv` fails to install `scikits.odes` on Python 3.11 due to Cython dependency resolution mismatch, works on Python 3.10 and earlier (cannot find `longintrepr.h`?)"
type: issue
state: closed
author: agriyakhetarpal
labels: []
assignees: []
created_at: 2024-03-25T11:35:34Z
updated_at: 2025-01-26T15:42:30Z
url: https://github.com/astral-sh/uv/issues/2646
synced_at: 2026-01-10T01:23:20Z
---

# `uv` fails to install `scikits.odes` on Python 3.11 due to Cython dependency resolution mismatch, works on Python 3.10 and earlier (cannot find `longintrepr.h`?)

---

_Issue opened by @agriyakhetarpal on 2024-03-25 11:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Description

Hi there! I'm opening this issue because I'm unable to install a package with `uv`, but the installation works with `pip` – it's called [`scikits.odes`](https://pypi.org/project/scikits.odes/). The [installation instructions](https://scikits-odes.readthedocs.io/en/latest/installation.html) mention the requirement of a Fortran compiler and a SUNDIALS installation present before installing off of the source distribution available on PyPI (no wheels are available).

## How to reproduce

Here is a reproducer on an M-series macOS machine (compiling `scikits.odes` is much easier on Unix-like platforms such as GNU/Linux and macOS, rather than on Windows).

>[!NOTE]
> Installing SUNDIALS via Homebrew usually works, but it doesn't work at the time of writing because the formula was updated to SUNDIALS v7.0.0, which is not supported (we regularly test in CI against v6.5.0). I have provided another method of installing SUNDIALS v6.5.0 for the purpose of this MWE.

```bash
# install Python 3.11 with Homebrew
brew install python@3.11
# Clone repository (we provide a helper script to install SUNDIALS
git clone https://github.com/pybamm-team/PyBaMM.git --depth 1
# Create a virtual environment in venv/
python3.11 -m venv venv
# Activate it
source venv/bin/activate
# Install SUNDIALS v6.5.0 via helper script, to ~/.local/
pip install nox
nox -s pybamm-requires
# Install uv into this virtual environment via pip
pip install uv
# Point to the SUNDIALS installation directory before installation
export SUNDIALS_INST="$HOME/.local/"
# Install it
uv pip install scikits.odes --verbose --no-cache-dir
```

## Expected behaviour

The expected behaviour would be that I can install `scikits.odes` with `uv` on both Python 3.10 and Python 3.11 (or between Python 3.8–3.11 based on the provided official support for the package). I can currently install `scikits.odes` with `pip` on all Python versions from 3.8–3.11.

## Additional context

I see that https://github.com/astral-sh/uv/issues/1946 faced this issue of not being able to find `longintrepr.h` earlier, I don't know if that's related – I am not used to working with Cython or Fortran codebases, but can help debug a bit with `pybind11` linkage. However, I'm noticing with a deeper dive with the logs that `pip`'s resolver chooses to point to `cython==0.29.37`, while `uv` chooses `cython==3.0a7`, which might be causing the trouble?

I would be happy to provide additional reproducers or logs as necessary.

xref: pybamm-team/PyBaMM#3825, aio-libs/aiohttp#6600

## Specifications

`uv 0.1.24 (a5cae0292 2024-03-22)` installed via `pip` from PyPI
macOS M-series, running Mac OS X Sonoma v14.3 (arm64)


---

_Comment by @agriyakhetarpal on 2024-03-25 11:36_

<details>
<summary>Error logs from uv with Python 3.11</summary>

```
uv pip install scikits.odes --verbose --no-cache-dir
INFO Found a virtualenv through VIRTUAL_ENV at: /Users/agriyakhetarpal/Desktop/PyBaMM/venv
DEBUG Probing interpreter info for: /Users/agriyakhetarpal/Desktop/PyBaMM/venv/bin/python
DEBUG Found Python 3.11.8 for: /Users/agriyakhetarpal/Desktop/PyBaMM/venv/bin/python
DEBUG Using Python 3.11.8 environment at venv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: scikits-odes*
DEBUG No cache entry for: https://pypi.org/simple/scikits-odes/
DEBUG No credentials found for: https://pypi.org/simple/scikits-odes/
DEBUG Searching for a compatible version of scikits-odes (*)
DEBUG Selecting: scikits-odes==2.7.0 (scikits.odes-2.7.0.tar.gz)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/2d/d2376f8f3c1d7ea5089cdb68902db3dafd48dbf2667eb32d83000d69e130/scikits.odes-2.7.0.tar.gz
DEBUG No credentials found for: https://files.pythonhosted.org/packages/e9/2d/d2376f8f3c1d7ea5089cdb68902db3dafd48dbf2667eb32d83000d69e130/scikits.odes-2.7.0.tar.gz
DEBUG Downloading source distribution: scikits-odes==2.7.0
DEBUG Preparing metadata for: scikits-odes==2.7.0
DEBUG No static metadata available for: scikits-odes==2.7.0 (DynamicPkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: setuptools<=64.0.0
DEBUG Adding direct dependency: wheel*
DEBUG Adding direct dependency: numpy*
DEBUG Adding direct dependency: cython<3.0.0a8
DEBUG No cache entry for: https://pypi.org/simple/setuptools/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/setuptools/
DEBUG No credentials found for: https://pypi.org/simple/setuptools/
DEBUG No cache entry for: https://pypi.org/simple/wheel/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/wheel/
DEBUG No credentials found for: https://pypi.org/simple/wheel/
DEBUG No cache entry for: https://pypi.org/simple/numpy/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/numpy/
DEBUG No credentials found for: https://pypi.org/simple/numpy/
DEBUG No cache entry for: https://pypi.org/simple/cython/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/cython/
DEBUG No credentials found for: https://pypi.org/simple/cython/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3e/83/e206edff58159a927c76bbfeef1bf8b39cb12bbb32ae3c6227deb16d0121/setuptools-64.0.0-py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/3e/83/e206edff58159a927c76bbfeef1bf8b39cb12bbb32ae3c6227deb16d0121/setuptools-64.0.0-py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/3e/83/e206edff58159a927c76bbfeef1bf8b39cb12bbb32ae3c6227deb16d0121/setuptools-64.0.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of setuptools (<=64.0.0)
DEBUG Selecting: setuptools==64.0.0 (setuptools-64.0.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/89/4f/9451fcec723618351e7a4128f4fdc9f69d459f0c6baf0de99eb7ee69b10a/Cython-3.0a7-py2.py3-none-any.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/89/4f/9451fcec723618351e7a4128f4fdc9f69d459f0c6baf0de99eb7ee69b10a/Cython-3.0a7-py2.py3-none-any.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/89/4f/9451fcec723618351e7a4128f4fdc9f69d459f0c6baf0de99eb7ee69b10a/Cython-3.0a7-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.43.0 (wheel-0.43.0-py3-none-any.whl)
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==1.26.4 (numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (<3.0.0a8)
DEBUG Selecting: cython==3.0a7 (Cython-3.0a7-py2.py3-none-any.whl)
DEBUG Installing in cython==3.0a7, numpy==1.26.4, setuptools==64.0.0, wheel==0.43.0 in /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpwqoCO3/.venv
DEBUG Identified uncached requirement: cython ==3.0a7
DEBUG Identified uncached requirement: numpy ==1.26.4
DEBUG Identified uncached requirement: setuptools ==64.0.0
DEBUG Identified uncached requirement: wheel ==0.43.0
DEBUG Downloading and building requirements for build: cython==3.0a7, numpy==1.26.4, setuptools==64.0.0, wheel==0.43.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/3e/83/e206edff58159a927c76bbfeef1bf8b39cb12bbb32ae3c6227deb16d0121/setuptools-64.0.0-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/3e/83/e206edff58159a927c76bbfeef1bf8b39cb12bbb32ae3c6227deb16d0121/setuptools-64.0.0-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/3e/83/e206edff58159a927c76bbfeef1bf8b39cb12bbb32ae3c6227deb16d0121/setuptools-64.0.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/89/4f/9451fcec723618351e7a4128f4fdc9f69d459f0c6baf0de99eb7ee69b10a/Cython-3.0a7-py2.py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/89/4f/9451fcec723618351e7a4128f4fdc9f69d459f0c6baf0de99eb7ee69b10a/Cython-3.0a7-py2.py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/89/4f/9451fcec723618351e7a4128f4fdc9f69d459f0c6baf0de99eb7ee69b10a/Cython-3.0a7-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl
DEBUG No credentials found for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl
DEBUG Installing build requirements: wheel==0.43.0, cython==3.0a7, setuptools==64.0.0, numpy==1.26.4
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Calling `setuptools.build_meta:__legacy__.prepare_metadata_for_build_wheel()`
DEBUG Prepared metadata for: scikits-odes==2.7.0
DEBUG Adding transitive dependency: scipy*
DEBUG No cache entry for: https://pypi.org/simple/scipy/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/scipy/
DEBUG No credentials found for: https://pypi.org/simple/scipy/
DEBUG Searching for a compatible version of scipy (*)
DEBUG Selecting: scipy==1.12.0 (scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/21/d4/e6c57acc61e59cd46acca27af1f400094d5dee218e372cc604b8162b97cb/scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl.metadata
DEBUG No credentials found for already-seen URL: https://files.pythonhosted.org/packages/21/d4/e6c57acc61e59cd46acca27af1f400094d5dee218e372cc604b8162b97cb/scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl.metadata
DEBUG No credentials found for: https://files.pythonhosted.org/packages/21/d4/e6c57acc61e59cd46acca27af1f400094d5dee218e372cc604b8162b97cb/scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl.metadata
DEBUG Adding transitive dependency: numpy>=1.22.4, <1.29.0
DEBUG Searching for a compatible version of numpy (>=1.22.4, <1.29.0)
DEBUG Selecting: numpy==1.26.4 (numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl)
Resolved 3 packages in 1.80s
DEBUG Requirement already satisfied: numpy==1.26.4
DEBUG Identified uncached requirement: scikits-odes ==2.7.0
DEBUG Requirement already satisfied: scipy==1.12.0
DEBUG Preserving seed package: wheel==0.42.0
DEBUG Preserving seed package: setuptools==69.1.1
DEBUG Unnecessary package: packaging==24.0
DEBUG Unnecessary package: filelock==3.13.1
DEBUG Unnecessary package: virtualenv==20.25.1
DEBUG Preserving seed package: pip==24.0
DEBUG Unnecessary package: nox==2024.3.2
DEBUG Unnecessary package: distlib==0.3.8
DEBUG Preserving seed package: uv==0.1.24
DEBUG Unnecessary package: platformdirs==4.2.0
DEBUG Unnecessary package: colorlog==6.8.2
DEBUG Unnecessary package: argcomplete==3.2.3
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/2d/d2376f8f3c1d7ea5089cdb68902db3dafd48dbf2667eb32d83000d69e130/scikits.odes-2.7.0.tar.gz
DEBUG Building: scikits-odes==2.7.0
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: setuptools<=64.0.0
DEBUG Adding direct dependency: wheel*
DEBUG Adding direct dependency: numpy*
DEBUG Adding direct dependency: cython<3.0.0a8
DEBUG Searching for a compatible version of cython (<3.0.0a8)
DEBUG Selecting: cython==3.0a7 (Cython-3.0a7-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools (<=64.0.0)
DEBUG Selecting: setuptools==64.0.0 (setuptools-64.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.43.0 (wheel-0.43.0-py3-none-any.whl)
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==1.26.4 (numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Installing in cython==3.0a7, numpy==1.26.4, setuptools==64.0.0, wheel==0.43.0 in /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv
DEBUG Requirement already cached: cython==3.0a7
DEBUG Requirement already cached: numpy==1.26.4
DEBUG Requirement already cached: setuptools==64.0.0
DEBUG Requirement already cached: wheel==0.43.0
DEBUG Installing build requirements: cython==3.0a7, numpy==1.26.4, setuptools==64.0.0, wheel==0.43.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel(metadata_directory=None)`
error: Failed to download distributions
  Caused by: Failed to fetch wheel: scikits-odes==2.7.0
  Caused by: Failed to build: scikits-odes==2.7.0
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running config_cc
INFO: unifing config_cc, config, build_clib, build_ext, build commands --compiler options
running config_fc
INFO: unifing config_fc, config, build_clib, build_ext, build commands --fcompiler options
running build_src
INFO: build_src
INFO: building extension "scikits.odes.ddaspk" sources
creating build
creating build/src.macosx-14.0-arm64-3.11
creating build/src.macosx-14.0-arm64-3.11/scikits
creating build/src.macosx-14.0-arm64-3.11/scikits/odes
INFO: f2py options: []
INFO: f2py: scikits/odes/ddaspk.pyf
Reading fortran codes...
        Reading file 'scikits/odes/ddaspk.pyf' (format:free)
Post-processing...
        Block: ddaspk__user__routines
                Block: ddaspk_user_interface
                        Block: res
                        Block: jac
        Block: ddaspk
                        Block: ddaspk
In: scikits/odes/ddaspk.pyf:ddaspk:unknown_interface:ddaspk
get_useparameters: no module ddaspk__user__routines info used by ddaspk
Applying post-processing hooks...
  character_backward_compatibility_hook
Post-processing (stage 2)...
Building modules...
    Constructing call-back function "cb_res_in_ddaspk__user__routines"
      def res(x,y,yprime): return delta,ires
    Constructing call-back function "cb_jac_in_ddaspk__user__routines"
      def jac(x,y,yprime,cj): return wm
    Building module "ddaspk"...
    Generating possibly empty wrappers"
    Maybe empty "ddaspk-f2pywrappers.f"
        Constructing wrapper function "ddaspk"...
warning: callstatement is defined without callprotoargument
getarrdims:warning: assumed shape array, using 0 instead of '*'
getarrdims:warning: assumed shape array, using 0 instead of '*'
          y,yprime,t,idid = ddaspk(res,jac,y,yprime,t,tout,info,rtol,atol,rwork,iwork,[res_extra_args,jac_extra_args,overwrite_y,overwrite_yprime])
    Wrote C/API module "ddaspk" to file "build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c"
INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.c' to sources.
INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes' to include_dirs.
creating build/src.macosx-14.0-arm64-3.11/build
creating build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11
creating build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits
creating build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
copying /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/f2py/src/fortranobject.c -> build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
copying /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/f2py/src/fortranobject.h -> build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
INFO:   adding 'build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspk-f2pywrappers.f' to sources.
INFO: building extension "scikits.odes.lsodi" sources
INFO: f2py options: []
INFO: f2py: scikits/odes/lsodi.pyf
Reading fortran codes...
        Reading file 'scikits/odes/lsodi.pyf' (format:free)
Post-processing...
        Block: lsodi__user__routines
                Block: lsodi_user_interface
                        Block: res
                        Block: adda
                        Block: jac
        Block: lsodi
                        Block: lsodi
In: scikits/odes/lsodi.pyf:lsodi:unknown_interface:lsodi
get_useparameters: no module lsodi__user__routines info used by lsodi
                        Block: intdy
Applying post-processing hooks...
  character_backward_compatibility_hook
Post-processing (stage 2)...
Building modules...
    Constructing call-back function "cb_res_in_lsodi__user__routines"
      def res(tn,y,s,ires): return r
    Constructing call-back function "cb_adda_in_lsodi__user__routines"
      def adda(t,y,ml,mu,p,[nrowp]): return p
    Constructing call-back function "cb_jac_in_lsodi__user__routines"
      def jac(t,y,s,ml,mu,[nrowp]): return p
    Building module "lsodi"...
    Generating possibly empty wrappers"
    Maybe empty "lsodi-f2pywrappers.f"
        Constructing wrapper function "lsodi"...
getarrdims:warning: assumed shape array, using 0 instead of '*'
          y,ydoti,t,istate = lsodi(res,adda,jac,y,ydoti,t,tout,itol,rtol,atol,itask,istate,iopt,rwork,iwork,mf,[res_extra_args,adda_extra_args,jac_extra_args,overwrite_y,overwrite_ydoti])
    Generating possibly empty wrappers"
    Maybe empty "lsodi-f2pywrappers.f"
        Constructing wrapper function "intdy"...
getarrdims:warning: assumed shape array, using 0 instead of '*'
          dky,iflag = intdy(t,k,yh,nyh)
    Wrote C/API module "lsodi" to file "build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodimodule.c"
INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.c' to sources.
INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes' to include_dirs.
INFO:   adding 'build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodi-f2pywrappers.f' to sources.
INFO: build_src: building npy-pkg config files
running build_py
creating build/lib.macosx-14.0-arm64-cpython-311
creating build/lib.macosx-14.0-arm64-cpython-311/scikits
copying scikits/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits
creating build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/ddaspkint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/odeint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/dae.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/ode.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/dopri5.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/info.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
copying scikits/odes/lsodiint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
creating build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_get_info.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_user_return_vals_ida.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_on_funcs_ida.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_user_return_vals_cvode.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_dop.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_odeint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_on_funcs.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
copying scikits/odes/tests/test_dae.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
creating build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/ida.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/common_defs.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_cvode.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_ida.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_sundials.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_nvector_serial.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/idas.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/cvode.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_sunmatrix.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/cvodes.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_sunlinsol.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_cvodes.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_sunnonlinsol.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
copying scikits/odes/sundials/c_idas.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
running build_ext
SUNDIALS installation path set to `/Users/agriyakhetarpal/.local/` via $SUNDIALS_INST.
INFO: get_default_fcompiler: matching types: '['gnu95', 'nag', 'nagfor', 'absoft', 'ibm', 'intel', 'gnu', 'g95', 'pg']'
INFO: customize Gnu95FCompiler
INFO: Found executable /opt/homebrew/bin/gfortran
INFO: customize Gnu95FCompiler
INFO: customize Gnu95FCompiler using config
compiling '_configtest.c':
#include <sundials/sundials_config.h>


int main(void)
{
#if SUNDIALS_DOUBLE_PRECISION
#else
#error false or undefined macro
#endif
    ;
    return 0;
}

INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: clang: _configtest.c
success!
removing: _configtest.c _configtest.o _configtest.o.d
Found sundials built with double precision.
compiling '_configtest.c':
#include <sundials/sundials_config.h>


int main(void)
{
#if SUNDIALS_INT32_T
#else
#error false or undefined macro
#endif
    ;
    return 0;
}

INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: clang: _configtest.c
success!
removing: _configtest.c _configtest.o _configtest.o.d
Found sundials built with int32.
compiling '_configtest.c':
#include <sundials/sundials_config.h>


int main(void)
{
#ifdef SUNDIALS_BLAS_LAPACK
#else
#error undefined macro
#endif
    ;
    return 0;
}

INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: clang: _configtest.c
failure.
removing: _configtest.c _configtest.o
Compiling scikits/odes/sundials/common_defs.pyx because it changed.
Compiling scikits/odes/sundials/cvode.pyx because it changed.
Compiling scikits/odes/sundials/ida.pyx because it changed.
Compiling scikits/odes/sundials/cvodes.pyx because it changed.
Compiling scikits/odes/sundials/idas.pyx because it changed.
[1/5] Cythonizing scikits/odes/sundials/common_defs.pyx
[2/5] Cythonizing scikits/odes/sundials/cvode.pyx
[3/5] Cythonizing scikits/odes/sundials/cvodes.pyx
[4/5] Cythonizing scikits/odes/sundials/ida.pyx
[5/5] Cythonizing scikits/odes/sundials/idas.pyx
INFO: customize UnixCCompiler
INFO: customize UnixCCompiler using build_ext
INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=native)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/distutils
creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpblm3qfu6/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/distutils/checks
INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-march=native'
INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-O3)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-O3'
INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-Werror=switch)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-Werror=switch'
INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-Werror)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-Werror'
INFO: CCompilerOpt.__init__[1795] : check requested baseline
INFO: CCompilerOpt.feature_test[1560] : testing feature 'NEON_FP16' with flags ()
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-Werror=switch -Werror'
INFO: CCompilerOpt.feature_test[1560] : testing feature 'NEON' with flags ()
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-Werror=switch -Werror'
INFO: CCompilerOpt.feature_test[1560] : testing feature 'NEON_VFPV4' with flags ()
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-Werror=switch -Werror'
INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMD' with flags ()
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-Werror=switch -Werror'
INFO: CCompilerOpt.__init__[1804] : check requested dispatch-able features
INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=armv8.2-a+dotprod)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-march=armv8.2-a+dotprod'
INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMDDP' with flags (-march=armv8.2-a+dotprod)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-march=armv8.2-a+dotprod -Werror=switch -Werror'
INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=armv8.2-a+fp16)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-march=armv8.2-a+fp16'
INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMDHP' with flags (-march=armv8.2-a+fp16)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-march=armv8.2-a+fp16 -Werror=switch -Werror'
INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=armv8.2-a+fp16fml)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-march=armv8.2-a+fp16fml'
INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMDFHM' with flags (-march=armv8.2-a+fp16+fp16fml)
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
extra options: '-march=armv8.2-a+fp16+fp16fml -Werror=switch -Werror'
INFO: CCompilerOpt.__init__[1816] : skip features (ASIMD NEON_FP16 NEON NEON_VFPV4) since its part of baseline
INFO: CCompilerOpt.__init__[1820] : initialize targets groups
INFO: CCompilerOpt.__init__[1822] : parse target group simd_test
INFO: CCompilerOpt._parse_target_tokens[2033] : skip targets (XOP FMA4 VX VSX3 VSX4 SSE2 VXE VXE2 SSE42 AVX512F VSX VSX2 AVX512_SKX (AVX2 FMA3)) not part of baseline or dispatch-able features
INFO: CCompilerOpt._parse_policy_not_keepbase[2145] : skip baseline features (ASIMD)
INFO: CCompilerOpt.generate_dispatch_header[2366] : generate CPU dispatch header: (build/src.macosx-14.0-arm64-3.11/numpy/distutils/include/npy_cpu_dispatch_config.h)
WARN: CCompilerOpt.generate_dispatch_header[2375] : dispatch header dir build/src.macosx-14.0-arm64-3.11/numpy/distutils/include does not exist, creating it
INFO: get_default_fcompiler: matching types: '['gnu95', 'nag', 'nagfor', 'absoft', 'ibm', 'intel', 'gnu', 'g95', 'pg']'
INFO: customize Gnu95FCompiler
INFO: customize Gnu95FCompiler
INFO: customize Gnu95FCompiler using build_ext
INFO: building 'scikits.odes.ddaspk' extension
INFO: compiling C sources
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

creating build/temp.macosx-14.0-arm64-cpython-311/build
creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11
creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits
creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes
creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build
creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11
creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits
creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: clang: build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c
INFO: clang: build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.c
INFO: compiling Fortran sources
INFO: Fortran f77 compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -fPIC -O3 -funroll-loops
Fortran f90 compiler: /opt/homebrew/bin/gfortran -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
Fortran fix compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
creating build/temp.macosx-14.0-arm64-cpython-311/scikits
creating build/temp.macosx-14.0-arm64-cpython-311/scikits/odes
creating build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack
INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: gfortran:f77: scikits/odes/daepack/ddaspk.f
INFO: gfortran:f77: scikits/odes/daepack/stodi.f
INFO: gfortran:f77: scikits/odes/daepack/solsy.f
INFO: gfortran:f77: scikits/odes/daepack/cfode.f
INFO: gfortran:f77: scikits/odes/daepack/prepji.f
INFO: gfortran:f77: scikits/odes/daepack/xerrwv.f
INFO: gfortran:f77: scikits/odes/daepack/ainvg.f
INFO: gfortran:f77: scikits/odes/daepack/lsodi.f
INFO: gfortran:f77: scikits/odes/daepack/ewset.f
INFO: gfortran:f77: scikits/odes/daepack/daux.f
INFO: gfortran:f77: scikits/odes/daepack/dlinpk.f
INFO: gfortran:f77: scikits/odes/daepack/intdy.f
INFO: gfortran:f77: scikits/odes/daepack/vnorm.f
INFO: gfortran:f77: build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspk-f2pywrappers.f
INFO: /opt/homebrew/bin/gfortran -Wall -g -Wall -g -undefined dynamic_lookup -bundle build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ddaspk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/solsy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/stodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/cfode.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/xerrwv.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/prepji.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/lsodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ainvg.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ewset.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/daux.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/dlinpk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/intdy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/vnorm.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspk-f2pywrappers.o -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13 -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -lgfortran -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/ddaspk.cpython-311-darwin.so
INFO: building 'scikits.odes.lsodi' extension
INFO: compiling C sources
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: clang: build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodimodule.c
INFO: compiling Fortran sources
INFO: Fortran f77 compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -fPIC -O3 -funroll-loops
Fortran f90 compiler: /opt/homebrew/bin/gfortran -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
Fortran fix compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: gfortran:f77: scikits/odes/daepack/cfode.f
INFO: gfortran:f77: scikits/odes/daepack/stodi.f
INFO: gfortran:f77: scikits/odes/daepack/ddaspk.f
INFO: gfortran:f77: scikits/odes/daepack/solsy.f
INFO: gfortran:f77: scikits/odes/daepack/xerrwv.f
INFO: gfortran:f77: scikits/odes/daepack/lsodi.f
INFO: gfortran:f77: scikits/odes/daepack/ainvg.f
INFO: gfortran:f77: scikits/odes/daepack/prepji.f
INFO: gfortran:f77: scikits/odes/daepack/ewset.f
INFO: gfortran:f77: scikits/odes/daepack/daux.f
INFO: gfortran:f77: scikits/odes/daepack/dlinpk.f
INFO: gfortran:f77: scikits/odes/daepack/intdy.f
INFO: gfortran:f77: scikits/odes/daepack/vnorm.f
INFO: gfortran:f77: build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodi-f2pywrappers.f
INFO: /opt/homebrew/bin/gfortran -Wall -g -Wall -g -undefined dynamic_lookup -bundle build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodimodule.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ddaspk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/solsy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/stodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/cfode.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/xerrwv.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/prepji.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/lsodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ainvg.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ewset.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/daux.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/dlinpk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/intdy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/vnorm.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodi-f2pywrappers.o -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13 -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -lgfortran -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/lsodi.cpython-311-darwin.so
INFO: building 'scikits.odes.sundials.common_defs' extension
INFO: compiling C sources
INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

creating build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
INFO: clang: scikits/odes/sundials/common_defs.c
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
INFO: CCompilerOpt.cache_flush[864] : write cache to path -> /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/built-wheels-v1/pypi/scikits-odes/2.7.0/Xx_Xjt6-wQ6TA5YIXjkYZ/scikits.odes-2.7.0.tar.gz/build/temp.macosx-14.0-arm64-cpython-311/ccompiler_opt_cache_ext.py
--- stderr:
<string>:25: DeprecationWarning: 

  `numpy.distutils` is deprecated since NumPy 1.23.0, as a result
  of the deprecation of `distutils` itself. It will be removed for
  Python >= 3.12. For older Python versions it will remain present.
  It is recommended to use `setuptools < 60.0` for those Python versions.
  For more details, see:
    https://numpy.org/devdocs/reference/distutils_status_migration.html 


/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/setuptools/dist.py:286: SetuptoolsDeprecationWarning: The namespace_packages parameter is deprecated, consider using implicit namespaces instead (PEP 420).
  warnings.warn(msg, SetuptoolsDeprecationWarning)
/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/setuptools/dist.py:771: UserWarning: Usage of dash-separated 'description-file' will not be supported in future versions. Please use the underscore name 'description_file' instead
  warnings.warn(
/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/setuptools/config/setupcfg.py:508: SetuptoolsDeprecationWarning: The license_file parameter is deprecated, use license_files instead.
  warnings.warn(msg, warning_class)
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
append_needs: unknown need 'int'
append_needs: unknown need 'double'
append_needs: unknown need 'int'
_configtest.c:8:2: error: undefined macro
#error undefined macro
 ^
1 error generated.
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:466:12: warning: unused variable 'cj' [-Wunused-variable]
    double cj=(*cj_cb_capi);
           ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:467:9: warning: unused variable 'ires' [-Wunused-variable]
    int ires=(*ires_cb_capi);
        ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:468:12: warning: unused variable 'rpar' [-Wunused-variable]
    double rpar=(*rpar_cb_capi);
           ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:469:9: warning: unused variable 'ipar' [-Wunused-variable]
    int ipar=(*ipar_cb_capi);
        ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:685:14: warning: unused variable 'rpar_Dims' [-Wunused-variable]
    npy_intp rpar_Dims[1] = {-1};
             ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:686:14: warning: unused variable 'ipar_Dims' [-Wunused-variable]
    npy_intp ipar_Dims[1] = {-1};
             ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:1131:29: warning: passing arguments to a function without a prototype is deprecated in all versions of C and is not supported in C2x [-Wdeprecated-non-prototype]
                (*f2py_func)(cb_res_in_ddaspk__user__routines,&neq,&t,y,yprime,&tout,info,rtol,atol,&idid,rwork,&lrw,iwork,&liw,&rpar,&ipar,cb_jac_in_ddaspk__user__routines,psol) ;
                            ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:907:46: warning: variable 'res_cptr' set but not used [-Wunused-but-set-variable]
    cb_res_in_ddaspk__user__routines_typedef res_cptr;
                                             ^
build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:911:46: warning: variable 'jac_cptr' set but not used [-Wunused-but-set-variable]
    cb_jac_in_ddaspk__user__routines_typedef jac_cptr;
                                             ^
9 warnings generated.
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/solsy.f:55:72:

   55 |  320    wm(i+2) = 1.0d0/di
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/solsy.f:57:72:

   57 |  340    x(i) = wm(i+2)*x(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/cfode.f:62:72:

   62 |  110      pc(i) = pc(i-1) + fnqm1*pc(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/cfode.f:71:72:

   71 |  120      xpin = xpin + tsign*pc(i)/dfloat(i+1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
scikits/odes/daepack/cfode.f:76:72:

   76 |  130      elco(i+1,nq) = rq1fac*pc(i)/dfloat(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
scikits/odes/daepack/cfode.f:99:72:

   99 |  210      pc(i) = pc(i-1) + fnq*pc(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
scikits/odes/daepack/cfode.f:103:72:

  103 |  220      elco(i,nq) = pc(i)/pc(2)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/solsy.f:1:39:

    1 |       subroutine solsy (wm, iwm, x, tem)
      |                                       1
Warning: Unused dummy argument 'tem' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/stodi.f:162:72:

  162 |  125    el(i) = elco(i,nq)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
scikits/odes/daepack/stodi.f:183:72:

  183 |  155    el(i) = elco(i,nq)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 155 at (1)
scikits/odes/daepack/stodi.f:206:72:

  206 |         do 180 i = 1,n
      |                                                                        1
Warning: Fortran 2018 deleted feature: Shared DO termination label 180 at (1)
scikits/odes/daepack/stodi.f:207:72:

  207 |  180      yh(i,j) = yh(i,j)*r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
scikits/odes/daepack/stodi.f:228:72:

  228 |  210      yh1(i) = yh1(i) + yh1(i+nyh)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
scikits/odes/daepack/stodi.f:239:72:

  239 |  230    y(i) = yh(i,1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 230 at (1)
scikits/odes/daepack/stodi.f:262:72:

  262 |  260    acor(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
scikits/odes/daepack/stodi.f:276:72:

  276 |  380    y(i) = yh(i,1) + el1h*acor(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
scikits/odes/daepack/prepji.f:70:72:

   70 |  110    wm(i+2) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/prepji.f:74:72:

   74 |  120    wm(i+2) = wm(i+2)*con
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
scikits/odes/daepack/stodi.f:314:72:

  314 |  440      yh1(i) = yh1(i) - yh1(i+nyh)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 440 at (1)
scikits/odes/daepack/prepji.f:93:72:

   93 |  220      wm(i+j1) = (rtem(i) - savr(i))*fac
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
scikits/odes/daepack/stodi.f:353:72:

  353 |         do 470 i = 1,n
      |                                                                        1
Warning: Fortran 2018 deleted feature: Shared DO termination label 470 at (1)
scikits/odes/daepack/stodi.f:354:72:

  354 |  470      yh(i,j) = yh(i,j) + eljh*acor(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 470 at (1)
scikits/odes/daepack/stodi.f:360:72:

  360 |  490    yh(i,lmax) = acor(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 490 at (1)
scikits/odes/daepack/prepji.f:122:72:

  122 |  410    wm(i+2) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
scikits/odes/daepack/prepji.f:126:72:

  126 |  420    wm(i+2) = wm(i+2)*con
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 420 at (1)
scikits/odes/daepack/stodi.f:376:72:

  376 |  510      yh1(i) = yh1(i) - yh1(i+nyh)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
scikits/odes/daepack/prepji.f:146:72:

  146 |  530      y(i) = y(i) + r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
scikits/odes/daepack/stodi.f:396:72:

  396 |  530    savf(i) = acor(i) - yh(i,lmax)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
scikits/odes/daepack/prepji.f:159:72:

  159 |  540        wm(ii+i) = (rtem(i) - savr(i))*fac
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 540 at (1)
scikits/odes/daepack/stodi.f:423:72:

  423 |  600    yh(i,newq+1) = acor(i)*r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 600 at (1)
scikits/odes/daepack/stodi.f:454:72:

  454 |  710    acor(i) = acor(i)*r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 710 at (1)
scikits/odes/daepack/ddaspk.f:1696:72:

 1696 |  305      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 305 at (1)
scikits/odes/daepack/ddaspk.f:1822:72:

 1822 |  357      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 357 at (1)
scikits/odes/daepack/ddaspk.f:1861:72:

 1861 | 380      RWORK(ITEMP + I - 1) = H*YPRIME(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/ddaspk.f:2018:72:

 2018 |  515      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 515 at (1)
scikits/odes/daepack/ddaspk.f:2035:72:

 2035 | 524        ATOL(I)=R*ATOL(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 524 at (1)
scikits/odes/daepack/xerrwv.f:1:40:

    1 |       subroutine xerrwv (msg, nmes, nerr, level, ni, i1, i2, nr, r1, r2)
      |                                        1
Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/ddaspk.f:2765:72:

 2765 | 260         PHI(I,J)=BETA(J)*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
scikits/odes/daepack/ddaspk.f:2816:72:

 2816 | 405     DELTA(I) = PHI(I,KP1) + E(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 405 at (1)
scikits/odes/daepack/ddaspk.f:2824:72:

 2824 | 415     DELTA(I) = PHI(I,K) + DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 415 at (1)
scikits/odes/daepack/ainvg.f:27:72:

   27 |    10    pw(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ainvg.f:47:72:

   47 |   110    pw(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/ddaspk.f:2873:72:

 2873 | 510      DELTA(I)=E(I)-PHI(I,KP2)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
scikits/odes/daepack/ddaspk.f:2923:72:

 2923 | 580      PHI(I,KP2)=E(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 580 at (1)
scikits/odes/daepack/ddaspk.f:2926:72:

 2926 | 590      PHI(I,KP1)=PHI(I,KP1)+E(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 590 at (1)
scikits/odes/daepack/ddaspk.f:2929:72:

 2929 |          DO 595 I=1,NEQ
      |                                                                        1
Warning: Fortran 2018 deleted feature: Shared DO termination label 595 at (1)
scikits/odes/daepack/ddaspk.f:2930:72:

 2930 | 595      PHI(I,J)=PHI(I,J)+PHI(I,J+1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 595 at (1)
scikits/odes/daepack/ddaspk.f:2955:72:

 2955 | 610         PHI(I,J)=TEMP1*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 610 at (1)
scikits/odes/daepack/ddaspk.f:2959:72:

 2959 | 640      PSI(I-1)=PSI(I)-H
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 640 at (1)
scikits/odes/daepack/ddaspk.f:3051:72:

 3051 | 695       PHI(I,2) = R*PHI(I,2)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 695 at (1)
scikits/odes/daepack/lsodi.f:1282:72:

 1282 |  80     ydoti(i) = rwork(i+lwm-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
scikits/odes/daepack/lsodi.f:1291:72:

 1291 |  95     rwork(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
scikits/odes/daepack/lsodi.f:1333:72:

 1333 |  115        rwork(i+lyh-1) = y(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 115 at (1)
scikits/odes/daepack/ddaspk.f:3302:72:

 3302 |  20     WT(I) = 1.0D0/WT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/lsodi.f:1338:72:

 1338 |  125        rwork(i+lyd0-1) = ydoti(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
scikits/odes/daepack/lsodi.f:1346:72:

 1346 |  135    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 135 at (1)
scikits/odes/daepack/lsodi.f:1370:72:

 1370 |  140    tol = dmax1(tol,rtol(i))
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 140 at (1)
scikits/odes/daepack/ddaspk.f:3349:72:

 3349 | 10       YPOUT(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/lsodi.f:1391:72:

 1391 |  190    rwork(i+lyd0-1) = h0*rwork(i+lyd0-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 190 at (1)
scikits/odes/daepack/ddaspk.f:3359:72:

 3359 | 20          YPOUT(I)=YPOUT(I)+D*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:3391:72:

 3391 | 20      SUM = SUM + ((V(I)*RWT(I))/VMAX)**2
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/lsodi.f:1442:72:

 1442 |  260    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
scikits/odes/daepack/lsodi.f:1518:72:

 1518 |  410    y(i) = rwork(i+lyh-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
scikits/odes/daepack/ddaspk.f:3826:72:

 3826 |  20           P(I) = P(I)*RATIO1
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/lsodi.f:1626:72:

 1626 |  585     y(i) = rwork(i+lyh-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 585 at (1)
scikits/odes/daepack/lsodi.f:1638:72:

 1638 |  592    y(i) = rwork(i+lyh-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 592 at (1)
scikits/odes/daepack/ddaspk.f:4136:72:

 4136 | 310      YPRIME(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
scikits/odes/daepack/ddaspk.f:4140:72:

 4140 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:4191:72:

 4191 | 377      DELTA(I) = MIN(Y(I),0.0D0)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 377 at (1)
scikits/odes/daepack/ddaspk.f:4195:72:

 4195 | 378      E(I) = E(I) - DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 378 at (1)
scikits/odes/daepack/ddaspk.f:4313:72:

 4313 | 100     E(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
scikits/odes/daepack/ddaspk.f:4324:72:

 4324 | 320        DELTA(I) = DELTA(I) * CONFAC
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:4337:72:

 4337 | 340      YPRIME(I)=YPRIME(I)-CJ*DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
scikits/odes/daepack/ddaspk.f:4461:72:

 4461 | 110      WM(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/ddaspk.f:4485:72:

 4485 | 220        WM(NROW+L)=(E(L)-DELTA(L))*DELINV
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
scikits/odes/daepack/ddaspk.f:4507:72:

 4507 | 410      WM(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
scikits/odes/daepack/ddaspk.f:4534:72:

 4534 | 510       YPRIME(N)=YPRIME(N)+CJ*DEL
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
scikits/odes/daepack/ddaspk.f:4551:72:

 4551 | 520         WM(II+I)=(E(I)-DELTA(I))*DELINV
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 520 at (1)
scikits/odes/daepack/ddaspk.f:5091:72:

 5091 |  20           P(I) = P(I)*RATIO1
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:5421:72:

 5421 | 310      YPRIME(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
scikits/odes/daepack/ddaspk.f:5425:72:

 5425 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:5475:72:

 5475 |  360    DELTA(I) = MIN(Y(I),0.0D0)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
scikits/odes/daepack/ddaspk.f:5479:72:

 5479 |  370    E(I) = E(I) - DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 370 at (1)
scikits/odes/daepack/ddaspk.f:5606:72:

 5606 | 100     E(I) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
scikits/odes/daepack/ddaspk.f:5617:72:

 5617 | 320       DELTA(I) = DELTA(I) * CONFAC
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:5623:72:

 5623 | 340     SAVR(I) = DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
scikits/odes/daepack/ddaspk.f:5637:72:

 5637 | 360      YPRIME(I) = YPRIME(I) - CJ*DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
scikits/odes/daepack/ddaspk.f:5769:72:

 5769 |  110     X(I) = 0.D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/ddaspk.f:5793:72:

 5793 |  120     X(I) = X(I) + WM(LZ+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
scikits/odes/daepack/ddaspk.f:5964:72:

 5964 |  10     Z(I) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ddaspk.f:5975:72:

 5975 |  30         V(I,1) = R(I)*WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
scikits/odes/daepack/ddaspk.f:5978:72:

 5978 |  35         V(I,1) = R(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
scikits/odes/daepack/ddaspk.f:5997:72:

 5997 |  60       HES(I,J) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
scikits/odes/daepack/ddaspk.f:6038:72:

 6038 |  70             DL(K) = S*DL(K) + C*V(K,IP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
scikits/odes/daepack/ddaspk.f:6045:72:

 6045 |  80         DL(K) = S*DL(K) + C*V(K,LLP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
scikits/odes/daepack/ddaspk.f:6066:72:

 6066 |  130     Z(I) = 0.D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
scikits/odes/daepack/ddaspk.f:6088:72:

 6088 |  170              DL(K) = S*DL(K) + C*V(K,IP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 170 at (1)
scikits/odes/daepack/ddaspk.f:6093:72:

 6093 |  180           DL(K) = S*DL(K) + C*V(K,MAXLP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
scikits/odes/daepack/ddaspk.f:6110:72:

 6110 |  210    R(K) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
scikits/odes/daepack/ddaspk.f:6114:72:

 6114 |  220    Z(K) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
scikits/odes/daepack/ddaspk.f:6119:72:

 6119 |  240    Z(I) = Z(I)/WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 240 at (1)
scikits/odes/daepack/ddaspk.f:6221:72:

 6221 |  10     VTEM(I) = V(I)/WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ddaspk.f:6229:72:

 6229 |  20     Z(I) = Y(I) + VTEM(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:6243:72:

 6243 |  70     Z(I) = VTEM(I) - SAVR(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
scikits/odes/daepack/ddaspk.f:6255:72:

 6255 |  90     Z(I) = Z(I)*WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 90 at (1)
scikits/odes/daepack/ddaspk.f:2369:3:

 2369 | 770   MSG = 'DASPK--  RUN TERMINATED. APPARENT INFINITE LOOP'
      |   1
Warning: Label 770 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/ddaspk.f:1685:3:

 1685 | 300   CONTINUE
      |   1
Warning: Label 300 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/ddaspk.f:4189:3:

 4189 | 375   IF(NONNEG .EQ. 0) GO TO 390
      |   1
Warning: Label 375 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/ddaspk.f:3365:58:

 3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
      |                                                          1
Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3365:53:

 3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
      |                                                     1
Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4982:22:

 4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
      |                      1
Warning: Unused dummy argument 'rhok' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4982:11:

 4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
      |           1
Warning: Unused dummy argument 'wm' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:48:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |                                                1
Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:43:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |                                           1
Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:38:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |                                      1
Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:15:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |               1
Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4222:47:

 4222 |      *   S,CONFAC,TOLNEW,MULDEL,MAXIT,IRES,IDUM,IERNEW)
      |                                               1
Warning: Unused dummy argument 'idum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4220:45:

 4220 |       SUBROUTINE DNSD(X,Y,YPRIME,NEQ,RES,PDUM,WT,RPAR,IPAR,
      |                                             1
Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3248:56:

 3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
      |                                                        1
Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3248:51:

 3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
      |                                                   1
Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:19:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                   1
Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3399:57:

 3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
      |                                                         1
Warning: Unused dummy argument 'dumpwk' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:29:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                             1
Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:24:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                        1
Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3399:33:

 3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
      |                                 1
Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:55:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                                                       1
Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3399:16:

 3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
      |                1
Warning: Unused dummy argument 'jsdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3398:61:

 3398 |       SUBROUTINE DDASID(X,Y,YPRIME,NEQ,ICOPT,ID,RES,JACD,PDUM,H,TSCALE,
      |                                                             1
Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3957:26:

 3957 |      *   EPCON,JCALC,JFDUM,KP1,NONNEG,NTYPE,IERNLS)
      |                          1
Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4617:70:

 4617 |      *   WT,JSKIP,RPAR,IPAR,SAVR,DELTA,R,YIC,YPIC,PWK,WM,IWM,CJ,UROUND,
      |                                                                      1
Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:5237:40:

 5237 |      *   WM,IWM,CJ,CJOLD,CJLAST,S,UROUND,EPLI,SQRTN,RSQRTN,
      |                                        1
Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/ewset.f:17:72:

   17 |  15     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 15 at (1)
scikits/odes/daepack/ewset.f:21:72:

   21 |  25     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 25 at (1)
scikits/odes/daepack/ewset.f:25:72:

   25 |  35     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
scikits/odes/daepack/ewset.f:29:72:

   29 |  45     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 45 at (1)
scikits/odes/daepack/daux.f:2:46:

    2 |       DOUBLE PRECISION FUNCTION D1MACH (IDUMMY)
      |                                              1
Warning: Unused dummy argument 'idummy' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/daux.f:48:40:

   48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
      |                                        1
Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/daux.f:48:34:

   48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
      |                                  1
Warning: Unused dummy argument 'nmes' at (1) [-Wunused-dummy-argument]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/lsodi.f:1521:10:

 1521 |       if (ihit) t = tcrit
      |          ^
Warning: 'ihit' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/lsodi.f:1150:18:

 1150 |       logical ihit
      |                  ^
note: 'ihit' was declared here
scikits/odes/daepack/daux.f:214:28:

  214 |       INTEGER FUNCTION IXSAV (IPAR, IVALUE, ISET)
      |                            ^
Warning: '__result_ixsav' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/dlinpk.f:763:72:

  763 |    10 assign 30 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:768:20:

  768 |    20    go to next,(30, 50, 70, 110)
      |                    1
Warning: Deleted feature: Assigned GOTO statement at (1)
scikits/odes/daepack/dlinpk.f:770:72:

  770 |       assign 50 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:779:72:

  779 |       assign 70 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:785:72:

  785 |       assign 110 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:821:72:

  821 |    95    sum = sum + dx(j)**2
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
scikits/odes/daepack/dlinpk.f:798:5:

  798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
      |     1
Warning: Label 110 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/dlinpk.f:793:5:

  793 |    70 if( dabs(dx(i)) .gt. cutlo ) go to 75
      |     1
Warning: Label 70 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/dlinpk.f:775:5:

  775 |    50 if( dx(i) .eq. zero) go to 200
      |     1
Warning: Label 50 at (1) defined but not used [-Wunused-label]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/intdy.f:48:72:

   48 |  10     ic = ic*jj
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/intdy.f:51:72:

   51 |  20     dky(i) = c*yh(i,l)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/intdy.f:61:72:

   61 |  30       ic = ic*jj
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
scikits/odes/daepack/intdy.f:64:72:

   64 |  40       dky(i) = c*yh(i,jp1) + s*dky(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 40 at (1)
scikits/odes/daepack/intdy.f:69:72:

   69 |  60     dky(i) = r*dky(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/vnorm.f:14:72:

   14 |  10     sum = sum + (v(i)*w(i))**2
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/ddaspk.f:6062:10:

 6062 |       IF (RHO .LT. RNRM) GO TO 150
      |          ^
Warning: 'rho' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:5951:44:

 5951 |       DOUBLE PRECISION RNRM,C,DLNRM,PROD,RHO,S,SNORMW,DNRM2,TEM
      |                                            ^
note: 'rho' was declared here
scikits/odes/daepack/dlinpk.f:798:10:

  798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
      |          ^
Warning: 'xmax' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/dlinpk.f:717:63:

  717 |       double precision   dx(1), cutlo, cuthi, hitest, sum, xmax,zero,one
      |                                                               ^
note: 'xmax' was declared here
scikits/odes/daepack/ddaspk.f:2908:72:

 2908 |       R=(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
      |                                                                        ^
Warning: 'erkm1' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:2817:11:

 2817 |       ERKM1=SIGMA(K)*DDWNRM(NEQ,DELTA,VT,RPAR,IPAR)
      |           ^
note: 'erkm1' was declared here
scikits/odes/daepack/ddaspk.f:2998:72:

 2998 |       R = 0.90D0*(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
      |                                                                        ^
Warning: 'est' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:2812:9:

 2812 |       EST = ERK
      |         ^
note: 'est' was declared here
scikits/odes/daepack/ddaspk.f:2997:72:

 2997 |       TEMP2 = K + 1
      |                                                                        ^
Warning: 'knew' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:2813:10:

 2813 |       KNEW=K
      |          ^
note: 'knew' was declared here
scikits/odes/daepack/ddaspk.f:1771:10:

 1771 |       IF (INDEX .EQ. 0) TSCALE = TDIST
      |          ^
Warning: 'index' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:1572:11:

 1572 |       INDEX = 1
      |           ^
note: 'index' was declared here
ld: warning: ignoring duplicate libraries: '-lgfortran'
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/stodi.f:162:72:

  162 |  125    el(i) = elco(i,nq)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
scikits/odes/daepack/stodi.f:183:72:

  183 |  155    el(i) = elco(i,nq)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 155 at (1)
scikits/odes/daepack/stodi.f:206:72:

  206 |         do 180 i = 1,n
      |                                                                        1
Warning: Fortran 2018 deleted feature: Shared DO termination label 180 at (1)
scikits/odes/daepack/stodi.f:207:72:

  207 |  180      yh(i,j) = yh(i,j)*r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
scikits/odes/daepack/stodi.f:228:72:

  228 |  210      yh1(i) = yh1(i) + yh1(i+nyh)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
scikits/odes/daepack/stodi.f:239:72:

  239 |  230    y(i) = yh(i,1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 230 at (1)
scikits/odes/daepack/stodi.f:262:72:

  262 |  260    acor(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
scikits/odes/daepack/stodi.f:276:72:

  276 |  380    y(i) = yh(i,1) + el1h*acor(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
scikits/odes/daepack/stodi.f:314:72:

  314 |  440      yh1(i) = yh1(i) - yh1(i+nyh)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 440 at (1)
scikits/odes/daepack/stodi.f:353:72:

  353 |         do 470 i = 1,n
      |                                                                        1
Warning: Fortran 2018 deleted feature: Shared DO termination label 470 at (1)
scikits/odes/daepack/stodi.f:354:72:

  354 |  470      yh(i,j) = yh(i,j) + eljh*acor(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 470 at (1)
scikits/odes/daepack/stodi.f:360:72:

  360 |  490    yh(i,lmax) = acor(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 490 at (1)
scikits/odes/daepack/stodi.f:376:72:

  376 |  510      yh1(i) = yh1(i) - yh1(i+nyh)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
scikits/odes/daepack/stodi.f:396:72:

  396 |  530    savf(i) = acor(i) - yh(i,lmax)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
scikits/odes/daepack/stodi.f:423:72:

  423 |  600    yh(i,newq+1) = acor(i)*r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 600 at (1)
scikits/odes/daepack/cfode.f:62:72:

   62 |  110      pc(i) = pc(i-1) + fnqm1*pc(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/stodi.f:454:72:

  454 |  710    acor(i) = acor(i)*r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 710 at (1)
scikits/odes/daepack/cfode.f:71:72:

   71 |  120      xpin = xpin + tsign*pc(i)/dfloat(i+1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
scikits/odes/daepack/cfode.f:76:72:

   76 |  130      elco(i+1,nq) = rq1fac*pc(i)/dfloat(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
scikits/odes/daepack/cfode.f:99:72:

   99 |  210      pc(i) = pc(i-1) + fnq*pc(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
scikits/odes/daepack/cfode.f:103:72:

  103 |  220      elco(i,nq) = pc(i)/pc(2)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/xerrwv.f:1:40:

    1 |       subroutine xerrwv (msg, nmes, nerr, level, ni, i1, i2, nr, r1, r2)
      |                                        1
Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:1696:72:

 1696 |  305      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 305 at (1)
scikits/odes/daepack/ddaspk.f:1822:72:

 1822 |  357      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 357 at (1)
scikits/odes/daepack/ddaspk.f:1861:72:

 1861 | 380      RWORK(ITEMP + I - 1) = H*YPRIME(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
scikits/odes/daepack/lsodi.f:1282:72:

 1282 |  80     ydoti(i) = rwork(i+lwm-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
scikits/odes/daepack/lsodi.f:1291:72:

 1291 |  95     rwork(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/lsodi.f:1333:72:

 1333 |  115        rwork(i+lyh-1) = y(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 115 at (1)
scikits/odes/daepack/lsodi.f:1338:72:

 1338 |  125        rwork(i+lyd0-1) = ydoti(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
scikits/odes/daepack/lsodi.f:1346:72:

 1346 |  135    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 135 at (1)
scikits/odes/daepack/lsodi.f:1370:72:

 1370 |  140    tol = dmax1(tol,rtol(i))
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 140 at (1)
scikits/odes/daepack/lsodi.f:1391:72:

 1391 |  190    rwork(i+lyd0-1) = h0*rwork(i+lyd0-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 190 at (1)
scikits/odes/daepack/ddaspk.f:2018:72:

 2018 |  515      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 515 at (1)
scikits/odes/daepack/ddaspk.f:2035:72:

 2035 | 524        ATOL(I)=R*ATOL(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 524 at (1)
scikits/odes/daepack/solsy.f:55:72:

   55 |  320    wm(i+2) = 1.0d0/di
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/solsy.f:57:72:

   57 |  340    x(i) = wm(i+2)*x(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
scikits/odes/daepack/lsodi.f:1442:72:

 1442 |  260    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
scikits/odes/daepack/solsy.f:1:39:

    1 |       subroutine solsy (wm, iwm, x, tem)
      |                                       1
Warning: Unused dummy argument 'tem' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/lsodi.f:1518:72:

 1518 |  410    y(i) = rwork(i+lyh-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
scikits/odes/daepack/lsodi.f:1626:72:

 1626 |  585     y(i) = rwork(i+lyh-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 585 at (1)
scikits/odes/daepack/lsodi.f:1638:72:

 1638 |  592    y(i) = rwork(i+lyh-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 592 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/ainvg.f:27:72:

   27 |    10    pw(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ainvg.f:47:72:

   47 |   110    pw(i) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/prepji.f:70:72:

   70 |  110    wm(i+2) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/prepji.f:74:72:

   74 |  120    wm(i+2) = wm(i+2)*con
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
scikits/odes/daepack/prepji.f:93:72:

   93 |  220      wm(i+j1) = (rtem(i) - savr(i))*fac
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
scikits/odes/daepack/prepji.f:122:72:

  122 |  410    wm(i+2) = 0.0d0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
scikits/odes/daepack/prepji.f:126:72:

  126 |  420    wm(i+2) = wm(i+2)*con
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 420 at (1)
scikits/odes/daepack/prepji.f:146:72:

  146 |  530      y(i) = y(i) + r
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
scikits/odes/daepack/prepji.f:159:72:

  159 |  540        wm(ii+i) = (rtem(i) - savr(i))*fac
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 540 at (1)
scikits/odes/daepack/ddaspk.f:2765:72:

 2765 | 260         PHI(I,J)=BETA(J)*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
scikits/odes/daepack/ddaspk.f:2816:72:

 2816 | 405     DELTA(I) = PHI(I,KP1) + E(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 405 at (1)
scikits/odes/daepack/ddaspk.f:2824:72:

 2824 | 415     DELTA(I) = PHI(I,K) + DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 415 at (1)
scikits/odes/daepack/ddaspk.f:2873:72:

 2873 | 510      DELTA(I)=E(I)-PHI(I,KP2)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
scikits/odes/daepack/ddaspk.f:2923:72:

 2923 | 580      PHI(I,KP2)=E(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 580 at (1)
scikits/odes/daepack/ddaspk.f:2926:72:

 2926 | 590      PHI(I,KP1)=PHI(I,KP1)+E(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 590 at (1)
scikits/odes/daepack/ddaspk.f:2929:72:

 2929 |          DO 595 I=1,NEQ
      |                                                                        1
Warning: Fortran 2018 deleted feature: Shared DO termination label 595 at (1)
scikits/odes/daepack/ddaspk.f:2930:72:

 2930 | 595      PHI(I,J)=PHI(I,J)+PHI(I,J+1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 595 at (1)
scikits/odes/daepack/ddaspk.f:2955:72:

 2955 | 610         PHI(I,J)=TEMP1*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 610 at (1)
scikits/odes/daepack/ddaspk.f:2959:72:

 2959 | 640      PSI(I-1)=PSI(I)-H
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 640 at (1)
scikits/odes/daepack/ddaspk.f:3051:72:

 3051 | 695       PHI(I,2) = R*PHI(I,2)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 695 at (1)
scikits/odes/daepack/ddaspk.f:3302:72:

 3302 |  20     WT(I) = 1.0D0/WT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:3349:72:

 3349 | 10       YPOUT(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ddaspk.f:3359:72:

 3359 | 20          YPOUT(I)=YPOUT(I)+D*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:3391:72:

 3391 | 20      SUM = SUM + ((V(I)*RWT(I))/VMAX)**2
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:3826:72:

 3826 |  20           P(I) = P(I)*RATIO1
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:4136:72:

 4136 | 310      YPRIME(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
scikits/odes/daepack/ddaspk.f:4140:72:

 4140 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:4191:72:

 4191 | 377      DELTA(I) = MIN(Y(I),0.0D0)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 377 at (1)
scikits/odes/daepack/ddaspk.f:4195:72:

 4195 | 378      E(I) = E(I) - DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 378 at (1)
scikits/odes/daepack/ddaspk.f:4313:72:

 4313 | 100     E(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
scikits/odes/daepack/ddaspk.f:4324:72:

 4324 | 320        DELTA(I) = DELTA(I) * CONFAC
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:4337:72:

 4337 | 340      YPRIME(I)=YPRIME(I)-CJ*DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
scikits/odes/daepack/ddaspk.f:4461:72:

 4461 | 110      WM(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/ddaspk.f:4485:72:

 4485 | 220        WM(NROW+L)=(E(L)-DELTA(L))*DELINV
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
scikits/odes/daepack/ddaspk.f:4507:72:

 4507 | 410      WM(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
scikits/odes/daepack/ddaspk.f:4534:72:

 4534 | 510       YPRIME(N)=YPRIME(N)+CJ*DEL
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
scikits/odes/daepack/ddaspk.f:4551:72:

 4551 | 520         WM(II+I)=(E(I)-DELTA(I))*DELINV
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 520 at (1)
scikits/odes/daepack/ddaspk.f:5091:72:

 5091 |  20           P(I) = P(I)*RATIO1
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:5421:72:

 5421 | 310      YPRIME(I)=0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
scikits/odes/daepack/ddaspk.f:5425:72:

 5425 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:5475:72:

 5475 |  360    DELTA(I) = MIN(Y(I),0.0D0)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
scikits/odes/daepack/ddaspk.f:5479:72:

 5479 |  370    E(I) = E(I) - DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 370 at (1)
scikits/odes/daepack/ddaspk.f:5606:72:

 5606 | 100     E(I) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
scikits/odes/daepack/ddaspk.f:5617:72:

 5617 | 320       DELTA(I) = DELTA(I) * CONFAC
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
scikits/odes/daepack/ddaspk.f:5623:72:

 5623 | 340     SAVR(I) = DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
scikits/odes/daepack/ddaspk.f:5637:72:

 5637 | 360      YPRIME(I) = YPRIME(I) - CJ*DELTA(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
scikits/odes/daepack/ddaspk.f:5769:72:

 5769 |  110     X(I) = 0.D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
scikits/odes/daepack/ddaspk.f:5793:72:

 5793 |  120     X(I) = X(I) + WM(LZ+I-1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
scikits/odes/daepack/ddaspk.f:5964:72:

 5964 |  10     Z(I) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ddaspk.f:5975:72:

 5975 |  30         V(I,1) = R(I)*WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
scikits/odes/daepack/ddaspk.f:5978:72:

 5978 |  35         V(I,1) = R(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
scikits/odes/daepack/ddaspk.f:5997:72:

 5997 |  60       HES(I,J) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
scikits/odes/daepack/ddaspk.f:6038:72:

 6038 |  70             DL(K) = S*DL(K) + C*V(K,IP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
scikits/odes/daepack/ddaspk.f:6045:72:

 6045 |  80         DL(K) = S*DL(K) + C*V(K,LLP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
scikits/odes/daepack/ddaspk.f:6066:72:

 6066 |  130     Z(I) = 0.D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
scikits/odes/daepack/ddaspk.f:6088:72:

 6088 |  170              DL(K) = S*DL(K) + C*V(K,IP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 170 at (1)
scikits/odes/daepack/ddaspk.f:6093:72:

 6093 |  180           DL(K) = S*DL(K) + C*V(K,MAXLP1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
scikits/odes/daepack/ddaspk.f:6110:72:

 6110 |  210    R(K) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
scikits/odes/daepack/ddaspk.f:6114:72:

 6114 |  220    Z(K) = 0.0D0
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
scikits/odes/daepack/ddaspk.f:6119:72:

 6119 |  240    Z(I) = Z(I)/WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 240 at (1)
scikits/odes/daepack/ddaspk.f:6221:72:

 6221 |  10     VTEM(I) = V(I)/WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ddaspk.f:6229:72:

 6229 |  20     Z(I) = Y(I) + VTEM(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/ddaspk.f:6243:72:

 6243 |  70     Z(I) = VTEM(I) - SAVR(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
scikits/odes/daepack/ddaspk.f:6255:72:

 6255 |  90     Z(I) = Z(I)*WGHT(I)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 90 at (1)
scikits/odes/daepack/ddaspk.f:2369:3:

 2369 | 770   MSG = 'DASPK--  RUN TERMINATED. APPARENT INFINITE LOOP'
      |   1
Warning: Label 770 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/ddaspk.f:1685:3:

 1685 | 300   CONTINUE
      |   1
Warning: Label 300 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/ddaspk.f:4189:3:

 4189 | 375   IF(NONNEG .EQ. 0) GO TO 390
      |   1
Warning: Label 375 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/ddaspk.f:3365:58:

 3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
      |                                                          1
Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3365:53:

 3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
      |                                                     1
Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4982:22:

 4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
      |                      1
Warning: Unused dummy argument 'rhok' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4982:11:

 4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
      |           1
Warning: Unused dummy argument 'wm' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:48:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |                                                1
Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:43:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |                                           1
Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:38:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |                                      1
Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4221:15:

 4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
      |               1
Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4222:47:

 4222 |      *   S,CONFAC,TOLNEW,MULDEL,MAXIT,IRES,IDUM,IERNEW)
      |                                               1
Warning: Unused dummy argument 'idum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4220:45:

 4220 |       SUBROUTINE DNSD(X,Y,YPRIME,NEQ,RES,PDUM,WT,RPAR,IPAR,
      |                                             1
Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3248:56:

 3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
      |                                                        1
Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3248:51:

 3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
      |                                                   1
Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:19:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                   1
Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3399:57:

 3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
      |                                                         1
Warning: Unused dummy argument 'dumpwk' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:29:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                             1
Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:24:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                        1
Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3399:33:

 3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
      |                                 1
Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3400:55:

 3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
      |                                                       1
Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3399:16:

 3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
      |                1
Warning: Unused dummy argument 'jsdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3398:61:

 3398 |       SUBROUTINE DDASID(X,Y,YPRIME,NEQ,ICOPT,ID,RES,JACD,PDUM,H,TSCALE,
      |                                                             1
Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:3957:26:

 3957 |      *   EPCON,JCALC,JFDUM,KP1,NONNEG,NTYPE,IERNLS)
      |                          1
Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:4617:70:

 4617 |      *   WT,JSKIP,RPAR,IPAR,SAVR,DELTA,R,YIC,YPIC,PWK,WM,IWM,CJ,UROUND,
      |                                                                      1
Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/ddaspk.f:5237:40:

 5237 |      *   WM,IWM,CJ,CJOLD,CJLAST,S,UROUND,EPLI,SQRTN,RSQRTN,
      |                                        1
Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/ewset.f:17:72:

   17 |  15     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 15 at (1)
scikits/odes/daepack/ewset.f:21:72:

   21 |  25     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 25 at (1)
scikits/odes/daepack/ewset.f:25:72:

   25 |  35     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(1)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
scikits/odes/daepack/ewset.f:29:72:

   29 |  45     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 45 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/daux.f:2:46:

    2 |       DOUBLE PRECISION FUNCTION D1MACH (IDUMMY)
      |                                              1
Warning: Unused dummy argument 'idummy' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/daux.f:48:40:

   48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
      |                                        1
Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
scikits/odes/daepack/daux.f:48:34:

   48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
      |                                  1
Warning: Unused dummy argument 'nmes' at (1) [-Wunused-dummy-argument]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/daux.f:214:28:

  214 |       INTEGER FUNCTION IXSAV (IPAR, IVALUE, ISET)
      |                            ^
Warning: '__result_ixsav' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/dlinpk.f:763:72:

  763 |    10 assign 30 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:768:20:

  768 |    20    go to next,(30, 50, 70, 110)
      |                    1
Warning: Deleted feature: Assigned GOTO statement at (1)
scikits/odes/daepack/dlinpk.f:770:72:

  770 |       assign 50 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:779:72:

  779 |       assign 70 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:785:72:

  785 |       assign 110 to next
      |                                                                        1
Warning: Deleted feature: ASSIGN statement at (1)
scikits/odes/daepack/dlinpk.f:821:72:

  821 |    95    sum = sum + dx(j)**2
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
scikits/odes/daepack/dlinpk.f:798:5:

  798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
      |     1
Warning: Label 110 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/dlinpk.f:793:5:

  793 |    70 if( dabs(dx(i)) .gt. cutlo ) go to 75
      |     1
Warning: Label 70 at (1) defined but not used [-Wunused-label]
scikits/odes/daepack/dlinpk.f:775:5:

  775 |    50 if( dx(i) .eq. zero) go to 200
      |     1
Warning: Label 50 at (1) defined but not used [-Wunused-label]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/lsodi.f:1521:10:

 1521 |       if (ihit) t = tcrit
      |          ^
Warning: 'ihit' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/lsodi.f:1150:18:

 1150 |       logical ihit
      |                  ^
note: 'ihit' was declared here
scikits/odes/daepack/intdy.f:48:72:

   48 |  10     ic = ic*jj
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/intdy.f:51:72:

   51 |  20     dky(i) = c*yh(i,l)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
scikits/odes/daepack/intdy.f:61:72:

   61 |  30       ic = ic*jj
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
scikits/odes/daepack/intdy.f:64:72:

   64 |  40       dky(i) = c*yh(i,jp1) + s*dky(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 40 at (1)
scikits/odes/daepack/intdy.f:69:72:

   69 |  60     dky(i) = r*dky(i)
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
f951: Warning: Nonexistent include directory '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include' [-Wmissing-include-dirs]
scikits/odes/daepack/vnorm.f:14:72:

   14 |  10     sum = sum + (v(i)*w(i))**2
      |                                                                        1
Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
scikits/odes/daepack/ddaspk.f:6062:10:

 6062 |       IF (RHO .LT. RNRM) GO TO 150
      |          ^
Warning: 'rho' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:5951:44:

 5951 |       DOUBLE PRECISION RNRM,C,DLNRM,PROD,RHO,S,SNORMW,DNRM2,TEM
      |                                            ^
note: 'rho' was declared here
scikits/odes/daepack/dlinpk.f:798:10:

  798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
      |          ^
Warning: 'xmax' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/dlinpk.f:717:63:

  717 |       double precision   dx(1), cutlo, cuthi, hitest, sum, xmax,zero,one
      |                                                               ^
note: 'xmax' was declared here
scikits/odes/daepack/ddaspk.f:2908:72:

 2908 |       R=(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
      |                                                                        ^
Warning: 'erkm1' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:2817:11:

 2817 |       ERKM1=SIGMA(K)*DDWNRM(NEQ,DELTA,VT,RPAR,IPAR)
      |           ^
note: 'erkm1' was declared here
scikits/odes/daepack/ddaspk.f:2998:72:

 2998 |       R = 0.90D0*(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
      |                                                                        ^
Warning: 'est' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:2812:9:

 2812 |       EST = ERK
      |         ^
note: 'est' was declared here
scikits/odes/daepack/ddaspk.f:2997:72:

 2997 |       TEMP2 = K + 1
      |                                                                        ^
Warning: 'knew' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:2813:10:

 2813 |       KNEW=K
      |          ^
note: 'knew' was declared here
scikits/odes/daepack/ddaspk.f:1771:10:

 1771 |       IF (INDEX .EQ. 0) TSCALE = TDIST
      |          ^
Warning: 'index' may be used uninitialized [-Wmaybe-uninitialized]
scikits/odes/daepack/ddaspk.f:1572:11:

 1572 |       INDEX = 1
      |           ^
note: 'index' was declared here
ld: warning: ignoring duplicate libraries: '-lgfortran'
scikits/odes/sundials/common_defs.c:326:12: fatal error: 'longintrepr.h' file not found
  #include "longintrepr.h"
           ^~~~~~~~~~~~~~~
1 error generated.
error: Command "clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk -I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/.tmpJhBBWX/.tmpXjKg3Y/.venv/include -I/opt/homebrew/opt/python@3.11/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c scikits/odes/sundials/common_defs.c -o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/common_defs.o -MMD -MF build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/common_defs.o.d" failed with exit status 1
---
```

</details>

---

_Comment by @agriyakhetarpal on 2024-03-25 11:36_

<details>
<summary>Successful installation via pip on Python 3.11</summary>

```
pip install scikits.odes --verbose --no-cache-dir
Using pip 24.0 from /Users/agriyakhetarpal/Desktop/PyBaMM/venv/lib/python3.11/site-packages/pip (python 3.11)
Collecting scikits.odes
  Downloading scikits.odes-2.7.0.tar.gz (931 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 931.8/931.8 kB 23.6 MB/s eta 0:00:00
  Running command pip subprocess to install build dependencies
  Collecting setuptools<=64.0.0
    Using cached setuptools-64.0.0-py3-none-any.whl.metadata (6.2 kB)
  Collecting wheel
    Using cached wheel-0.43.0-py3-none-any.whl.metadata (2.2 kB)
  Collecting numpy
    Using cached numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata (114 kB)
  Collecting cython<3.0.0a8
    Using cached Cython-0.29.37-py2.py3-none-any.whl.metadata (3.1 kB)
  Using cached setuptools-64.0.0-py3-none-any.whl (1.2 MB)
  Using cached wheel-0.43.0-py3-none-any.whl (65 kB)
  Using cached numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl (14.0 MB)
  Using cached Cython-0.29.37-py2.py3-none-any.whl (989 kB)
  Installing collected packages: wheel, setuptools, numpy, cython
  Successfully installed cython-0.29.37 numpy-1.26.4 setuptools-64.0.0 wheel-0.43.0
  Installing build dependencies ... done
  Running command Getting requirements to build wheel
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/dist.py:771: UserWarning: Usage of dash-separated 'description-file' will not be supported in future versions. Please use the underscore name 'description_file' instead
    warnings.warn(
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/config/setupcfg.py:508: SetuptoolsDeprecationWarning: The license_file parameter is deprecated, use license_files instead.
    warnings.warn(msg, warning_class)
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/dist.py:286: SetuptoolsDeprecationWarning: The namespace_packages parameter is deprecated, consider using implicit namespaces instead (PEP 420).
    warnings.warn(msg, SetuptoolsDeprecationWarning)
  running egg_info
  writing scikits.odes.egg-info/PKG-INFO
  writing dependency_links to scikits.odes.egg-info/dependency_links.txt
  writing namespace_packages to scikits.odes.egg-info/namespace_packages.txt
  writing requirements to scikits.odes.egg-info/requires.txt
  writing top-level names to scikits.odes.egg-info/top_level.txt
  reading manifest file 'scikits.odes.egg-info/SOURCES.txt'
  reading manifest template 'MANIFEST.in'
  no previously-included directories found matching 'ci_support'
  no previously-included directories found matching 'docs'
  no previously-included directories found matching 'apidocs'
  no previously-included directories found matching 'paper'
  no previously-included directories found matching 'ipython_examples'
  warning: no previously-included files found matching '*.enc'
  warning: no previously-included files found matching 'upload_api_docs.sh'
  adding license file 'LICENSE.txt'
  writing manifest file 'scikits.odes.egg-info/SOURCES.txt'
  Getting requirements to build wheel ... done
  Running command Preparing metadata (pyproject.toml)
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/dist.py:286: SetuptoolsDeprecationWarning: The namespace_packages parameter is deprecated, consider using implicit namespaces instead (PEP 420).
    warnings.warn(msg, SetuptoolsDeprecationWarning)
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/dist.py:771: UserWarning: Usage of dash-separated 'description-file' will not be supported in future versions. Please use the underscore name 'description_file' instead
    warnings.warn(
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/config/setupcfg.py:508: SetuptoolsDeprecationWarning: The license_file parameter is deprecated, use license_files instead.
    warnings.warn(msg, warning_class)
  running dist_info
  creating /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info
  writing /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/PKG-INFO
  writing dependency_links to /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/dependency_links.txt
  writing namespace_packages to /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/namespace_packages.txt
  writing requirements to /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/requires.txt
  writing top-level names to /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/top_level.txt
  writing manifest file '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/SOURCES.txt'
  reading manifest file '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/SOURCES.txt'
  reading manifest template 'MANIFEST.in'
  no previously-included directories found matching 'ci_support'
  no previously-included directories found matching 'docs'
  no previously-included directories found matching 'apidocs'
  no previously-included directories found matching 'paper'
  no previously-included directories found matching 'ipython_examples'
  warning: no previously-included files found matching '*.enc'
  warning: no previously-included files found matching 'upload_api_docs.sh'
  adding license file 'LICENSE.txt'
  writing manifest file '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes.egg-info/SOURCES.txt'
  creating '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-modern-metadata-gyote6s2/scikits.odes-2.7.0.dist-info'
  Preparing metadata (pyproject.toml) ... done
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.10'): https://files.pythonhosted.org/packages/99/f1/c00d6be56e1a718a3068079e3ec8ce044d7179345280f6a3f5066068af0d/scipy-1.6.2.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.7,<3.10)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.10'): https://files.pythonhosted.org/packages/fe/fd/8704c7b7b34cdac850485e638346025ca57c5a859934b9aa1be5399b33b7/scipy-1.6.3.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.7,<3.10)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.10'): https://files.pythonhosted.org/packages/bb/bb/944f559d554df6c9adf037aa9fc982a9706ee0e96c0d5beac701cb158900/scipy-1.7.0.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.7,<3.10)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.10'): https://files.pythonhosted.org/packages/47/33/a24aec22b7be7fdb10ec117a95e1e4099890d8bbc6646902f443fc7719d1/scipy-1.7.1.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.7,<3.10)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.11'): https://files.pythonhosted.org/packages/0e/23/58c4f995475a2a97cb5f4a032aedaf881ad87cd976a7180c55118d105a1d/scipy-1.7.2.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.7,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.11'): https://files.pythonhosted.org/packages/61/67/1a654b96309c991762ee9bc39c363fc618076b155fe52d295211cf2536c7/scipy-1.7.3.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.7,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.8,<3.11'): https://files.pythonhosted.org/packages/c0/ad/e3c052ed4e0027a8abef0a5e8441a044427d252d17d9aee06d56e62fc698/scipy-1.8.0rc1.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.8,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.8,<3.11'): https://files.pythonhosted.org/packages/29/d2/151a54944b333e465f98804dced31dab1284f3c37b752b9cefa710b64681/scipy-1.8.0rc2.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.8,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.8,<3.11'): https://files.pythonhosted.org/packages/e4/26/83dd1c6378513a6241d984bda9f08c512b6e35fff13fba3acc1b3c195f02/scipy-1.8.0rc3.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.8,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.8,<3.11'): https://files.pythonhosted.org/packages/22/78/056cc43e7737811b6f50886788a940f852773dd9804f5365952805db9648/scipy-1.8.0rc4.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.8,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.8,<3.11'): https://files.pythonhosted.org/packages/b4/a2/4faa34bf0cdbefd5c706625f1234987795f368eb4e97bde9d6f46860843e/scipy-1.8.0.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.8,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.8,<3.11'): https://files.pythonhosted.org/packages/26/b5/9330f004b9a3b2b6a31f59f46f1617ce9ca15c0e7fe64288c20385a05c9d/scipy-1.8.1.tar.gz (from https://pypi.org/simple/scipy/) (requires-python:>=3.8,<3.11)
Collecting scipy (from scikits.odes)
  Obtaining dependency information for scipy from https://files.pythonhosted.org/packages/21/d4/e6c57acc61e59cd46acca27af1f400094d5dee218e372cc604b8162b97cb/scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl.metadata
  Downloading scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl.metadata (165 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 165.4/165.4 kB 64.5 MB/s eta 0:00:00
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.11'): https://files.pythonhosted.org/packages/3a/be/650f9c091ef71cb01d735775d554e068752d3ff63d7943b26316dc401749/numpy-1.21.2.zip (from https://pypi.org/simple/numpy/) (requires-python:>=3.7,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.11'): https://files.pythonhosted.org/packages/5f/d6/ad58ded26556eaeaa8c971e08b6466f17c4ac4d786cd3d800e26ce59cc01/numpy-1.21.3.zip (from https://pypi.org/simple/numpy/) (requires-python:>=3.7,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.11'): https://files.pythonhosted.org/packages/fb/48/b0708ebd7718a8933f0d3937513ef8ef2f4f04529f1f66ca86d873043921/numpy-1.21.4.zip (from https://pypi.org/simple/numpy/) (requires-python:>=3.7,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.11'): https://files.pythonhosted.org/packages/c2/a8/a924a09492bdfee8c2ec3094d0a13f2799800b4fdc9c890738aeeb12c72e/numpy-1.21.5.zip (from https://pypi.org/simple/numpy/) (requires-python:>=3.7,<3.11)
  Link requires a different Python (3.11.8 not in: '>=3.7,<3.11'): https://files.pythonhosted.org/packages/45/b7/de7b8e67f2232c26af57c205aaad29fe17754f793404f59c8a730c7a191a/numpy-1.21.6.zip (from https://pypi.org/simple/numpy/) (requires-python:>=3.7,<3.11)
Collecting numpy<1.29.0,>=1.22.4 (from scipy->scikits.odes)
  Obtaining dependency information for numpy<1.29.0,>=1.22.4 from https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata
  Downloading numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata (114 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 114.8/114.8 kB 54.0 MB/s eta 0:00:00
Downloading scipy-1.12.0-cp311-cp311-macosx_12_0_arm64.whl (31.4 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 31.4/31.4 MB 23.1 MB/s eta 0:00:00
Downloading numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl (14.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.0/14.0 MB 19.3 MB/s eta 0:00:00
Building wheels for collected packages: scikits.odes
  Running command Building wheel for scikits.odes (pyproject.toml)
  <string>:25: DeprecationWarning:

    `numpy.distutils` is deprecated since NumPy 1.23.0, as a result
    of the deprecation of `distutils` itself. It will be removed for
    Python >= 3.12. For older Python versions it will remain present.
    It is recommended to use `setuptools < 60.0` for those Python versions.
    For more details, see:
      https://numpy.org/devdocs/reference/distutils_status_migration.html


  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/dist.py:286: SetuptoolsDeprecationWarning: The namespace_packages parameter is deprecated, consider using implicit namespaces instead (PEP 420).
    warnings.warn(msg, SetuptoolsDeprecationWarning)
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/dist.py:771: UserWarning: Usage of dash-separated 'description-file' will not be supported in future versions. Please use the underscore name 'description_file' instead
    warnings.warn(
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/config/setupcfg.py:508: SetuptoolsDeprecationWarning: The license_file parameter is deprecated, use license_files instead.
    warnings.warn(msg, warning_class)
  running bdist_wheel
  running build
  running config_cc
  INFO: unifing config_cc, config, build_clib, build_ext, build commands --compiler options
  running config_fc
  INFO: unifing config_fc, config, build_clib, build_ext, build commands --fcompiler options
  running build_src
  INFO: build_src
  INFO: building extension "scikits.odes.ddaspk" sources
  creating build
  creating build/src.macosx-14.0-arm64-3.11
  creating build/src.macosx-14.0-arm64-3.11/scikits
  creating build/src.macosx-14.0-arm64-3.11/scikits/odes
  INFO: f2py options: []
  INFO: f2py: scikits/odes/ddaspk.pyf
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  Reading fortran codes...
        Reading file 'scikits/odes/ddaspk.pyf' (format:free)
  Post-processing...
        Block: ddaspk__user__routines
                Block: ddaspk_user_interface
                        Block: res
                        Block: jac
        Block: ddaspk
                        Block: ddaspk
  In: scikits/odes/ddaspk.pyf:ddaspk:unknown_interface:ddaspk
  get_useparameters: no module ddaspk__user__routines info used by ddaspk
  Applying post-processing hooks...
    character_backward_compatibility_hook
  Post-processing (stage 2)...
  Building modules...
      Constructing call-back function "cb_res_in_ddaspk__user__routines"
        def res(x,y,yprime): return delta,ires
      Constructing call-back function "cb_jac_in_ddaspk__user__routines"
        def jac(x,y,yprime,cj): return wm
      Building module "ddaspk"...
      Generating possibly empty wrappers"
      Maybe empty "ddaspk-f2pywrappers.f"
          Constructing wrapper function "ddaspk"...
  warning: callstatement is defined without callprotoargument
  getarrdims:warning: assumed shape array, using 0 instead of '*'
  getarrdims:warning: assumed shape array, using 0 instead of '*'
            y,yprime,t,idid = ddaspk(res,jac,y,yprime,t,tout,info,rtol,atol,rwork,iwork,[res_extra_args,jac_extra_args,overwrite_y,overwrite_yprime])
      Wrote C/API module "ddaspk" to file "build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c"
  INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.c' to sources.
  INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes' to include_dirs.
  creating build/src.macosx-14.0-arm64-3.11/build
  creating build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11
  creating build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits
  creating build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
  copying /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/f2py/src/fortranobject.c -> build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
  copying /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/f2py/src/fortranobject.h -> build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
  INFO:   adding 'build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspk-f2pywrappers.f' to sources.
  INFO: building extension "scikits.odes.lsodi" sources
  INFO: f2py options: []
  INFO: f2py: scikits/odes/lsodi.pyf
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  append_needs: unknown need 'int'
  append_needs: unknown need 'double'
  append_needs: unknown need 'int'
  Reading fortran codes...
        Reading file 'scikits/odes/lsodi.pyf' (format:free)
  Post-processing...
        Block: lsodi__user__routines
                Block: lsodi_user_interface
                        Block: res
                        Block: adda
                        Block: jac
        Block: lsodi
                        Block: lsodi
  In: scikits/odes/lsodi.pyf:lsodi:unknown_interface:lsodi
  get_useparameters: no module lsodi__user__routines info used by lsodi
                        Block: intdy
  Applying post-processing hooks...
    character_backward_compatibility_hook
  Post-processing (stage 2)...
  Building modules...
      Constructing call-back function "cb_res_in_lsodi__user__routines"
        def res(tn,y,s,ires): return r
      Constructing call-back function "cb_adda_in_lsodi__user__routines"
        def adda(t,y,ml,mu,p,[nrowp]): return p
      Constructing call-back function "cb_jac_in_lsodi__user__routines"
        def jac(t,y,s,ml,mu,[nrowp]): return p
      Building module "lsodi"...
      Generating possibly empty wrappers"
      Maybe empty "lsodi-f2pywrappers.f"
          Constructing wrapper function "lsodi"...
  getarrdims:warning: assumed shape array, using 0 instead of '*'
            y,ydoti,t,istate = lsodi(res,adda,jac,y,ydoti,t,tout,itol,rtol,atol,itask,istate,iopt,rwork,iwork,mf,[res_extra_args,adda_extra_args,jac_extra_args,overwrite_y,overwrite_ydoti])
      Generating possibly empty wrappers"
      Maybe empty "lsodi-f2pywrappers.f"
          Constructing wrapper function "intdy"...
  getarrdims:warning: assumed shape array, using 0 instead of '*'
            dky,iflag = intdy(t,k,yh,nyh)
      Wrote C/API module "lsodi" to file "build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodimodule.c"
  INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.c' to sources.
  INFO:   adding 'build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes' to include_dirs.
  INFO:   adding 'build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodi-f2pywrappers.f' to sources.
  INFO: build_src: building npy-pkg config files
  running build_py
  creating build/lib.macosx-14.0-arm64-cpython-311
  creating build/lib.macosx-14.0-arm64-cpython-311/scikits
  copying scikits/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits
  creating build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/ddaspkint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/odeint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/dae.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/ode.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/dopri5.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/info.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  copying scikits/odes/lsodiint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes
  creating build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_get_info.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_user_return_vals_ida.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_on_funcs_ida.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_user_return_vals_cvode.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_dop.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_odeint.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_on_funcs.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  copying scikits/odes/tests/test_dae.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/tests
  creating build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/__init__.py -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/ida.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/common_defs.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_cvode.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_ida.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_sundials.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_nvector_serial.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/idas.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/cvode.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_sunmatrix.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/cvodes.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_sunlinsol.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_cvodes.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_sunnonlinsol.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  copying scikits/odes/sundials/c_idas.pxd -> build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  running build_ext
  SUNDIALS installation path set to `/Users/agriyakhetarpal/.local/` via $SUNDIALS_INST.
  INFO: get_default_fcompiler: matching types: '['gnu95', 'nag', 'nagfor', 'absoft', 'ibm', 'intel', 'gnu', 'g95', 'pg']'
  INFO: customize Gnu95FCompiler
  INFO: Found executable /opt/homebrew/bin/gfortran
  INFO: customize Gnu95FCompiler
  INFO: customize Gnu95FCompiler using config
  compiling '_configtest.c':
  #include <sundials/sundials_config.h>


  int main(void)
  {
  #if SUNDIALS_DOUBLE_PRECISION
  #else
  #error false or undefined macro
  #endif
      ;
      return 0;
  }

  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: _configtest.c
  success!
  removing: _configtest.c _configtest.o _configtest.o.d
  Found sundials built with double precision.
  compiling '_configtest.c':
  #include <sundials/sundials_config.h>


  int main(void)
  {
  #if SUNDIALS_INT32_T
  #else
  #error false or undefined macro
  #endif
      ;
      return 0;
  }

  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: _configtest.c
  success!
  removing: _configtest.c _configtest.o _configtest.o.d
  Found sundials built with int32.
  compiling '_configtest.c':
  #include <sundials/sundials_config.h>


  int main(void)
  {
  #ifdef SUNDIALS_BLAS_LAPACK
  #else
  #error undefined macro
  #endif
      ;
      return 0;
  }

  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: _configtest.c
  _configtest.c:8:2: error: undefined macro
  #error undefined macro
   ^
  1 error generated.
  failure.
  removing: _configtest.c _configtest.o
  Compiling scikits/odes/sundials/common_defs.pyx because it changed.
  Compiling scikits/odes/sundials/cvode.pyx because it changed.
  Compiling scikits/odes/sundials/ida.pyx because it changed.
  Compiling scikits/odes/sundials/cvodes.pyx because it changed.
  Compiling scikits/odes/sundials/idas.pyx because it changed.
  [1/5] Cythonizing scikits/odes/sundials/common_defs.pyx
  [2/5] Cythonizing scikits/odes/sundials/cvode.pyx
  [3/5] Cythonizing scikits/odes/sundials/cvodes.pyx
  [4/5] Cythonizing scikits/odes/sundials/ida.pyx
  [5/5] Cythonizing scikits/odes/sundials/idas.pyx
  INFO: customize UnixCCompiler
  INFO: customize UnixCCompiler using build_ext
  INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=native)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/distutils
  creating /var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/tmpnwowc8r9/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/distutils/checks
  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-march=native'
  INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-O3)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-O3'
  INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-Werror=switch)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-Werror=switch'
  INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-Werror)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-Werror'
  INFO: CCompilerOpt.__init__[1795] : check requested baseline
  INFO: CCompilerOpt.feature_test[1560] : testing feature 'NEON' with flags ()
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-Werror=switch -Werror'
  INFO: CCompilerOpt.feature_test[1560] : testing feature 'NEON_FP16' with flags ()
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-Werror=switch -Werror'
  INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMD' with flags ()
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-Werror=switch -Werror'
  INFO: CCompilerOpt.feature_test[1560] : testing feature 'NEON_VFPV4' with flags ()
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-Werror=switch -Werror'
  INFO: CCompilerOpt.__init__[1804] : check requested dispatch-able features
  INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=armv8.2-a+fp16)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-march=armv8.2-a+fp16'
  INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMDHP' with flags (-march=armv8.2-a+fp16)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-march=armv8.2-a+fp16 -Werror=switch -Werror'
  INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=armv8.2-a+dotprod)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-march=armv8.2-a+dotprod'
  INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMDDP' with flags (-march=armv8.2-a+dotprod)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-march=armv8.2-a+dotprod -Werror=switch -Werror'
  INFO: CCompilerOpt.cc_test_flags[1086] : testing flags (-march=armv8.2-a+fp16fml)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-march=armv8.2-a+fp16fml'
  INFO: CCompilerOpt.feature_test[1560] : testing feature 'ASIMDFHM' with flags (-march=armv8.2-a+fp16+fp16fml)
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  extra options: '-march=armv8.2-a+fp16+fp16fml -Werror=switch -Werror'
  INFO: CCompilerOpt.__init__[1816] : skip features (NEON_VFPV4 NEON NEON_FP16 ASIMD) since its part of baseline
  INFO: CCompilerOpt.__init__[1820] : initialize targets groups
  INFO: CCompilerOpt.__init__[1822] : parse target group simd_test
  INFO: CCompilerOpt._parse_target_tokens[2033] : skip targets (VSX2 SSE2 VXE VSX3 VX VXE2 VSX AVX512F (FMA3 AVX2) XOP VSX4 AVX512_SKX FMA4 SSE42) not part of baseline or dispatch-able features
  INFO: CCompilerOpt._parse_policy_not_keepbase[2145] : skip baseline features (ASIMD)
  INFO: CCompilerOpt.generate_dispatch_header[2366] : generate CPU dispatch header: (build/src.macosx-14.0-arm64-3.11/numpy/distutils/include/npy_cpu_dispatch_config.h)
  WARN: CCompilerOpt.generate_dispatch_header[2375] : dispatch header dir build/src.macosx-14.0-arm64-3.11/numpy/distutils/include does not exist, creating it
  INFO: get_default_fcompiler: matching types: '['gnu95', 'nag', 'nagfor', 'absoft', 'ibm', 'intel', 'gnu', 'g95', 'pg']'
  INFO: customize Gnu95FCompiler
  INFO: customize Gnu95FCompiler
  INFO: customize Gnu95FCompiler using build_ext
  INFO: building 'scikits.odes.ddaspk' extension
  INFO: compiling C sources
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  creating build/temp.macosx-14.0-arm64-cpython-311/build
  creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11
  creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits
  creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes
  creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build
  creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11
  creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits
  creating build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes
  INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c
  INFO: clang: build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.c
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:466:12: warning: unused variable 'cj' [-Wunused-variable]
      double cj=(*cj_cb_capi);
             ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:467:9: warning: unused variable 'ires' [-Wunused-variable]
      int ires=(*ires_cb_capi);
          ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:468:12: warning: unused variable 'rpar' [-Wunused-variable]
      double rpar=(*rpar_cb_capi);
             ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:469:9: warning: unused variable 'ipar' [-Wunused-variable]
      int ipar=(*ipar_cb_capi);
          ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:685:14: warning: unused variable 'rpar_Dims' [-Wunused-variable]
      npy_intp rpar_Dims[1] = {-1};
               ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:686:14: warning: unused variable 'ipar_Dims' [-Wunused-variable]
      npy_intp ipar_Dims[1] = {-1};
               ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:1131:29: warning: passing arguments to a function without a prototype is deprecated in all versions of C and is not supported in C2x [-Wdeprecated-non-prototype]
                  (*f2py_func)(cb_res_in_ddaspk__user__routines,&neq,&t,y,yprime,&tout,info,rtol,atol,&idid,rwork,&lrw,iwork,&liw,&rpar,&ipar,cb_jac_in_ddaspk__user__routines,psol) ;
                              ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:907:46: warning: variable 'res_cptr' set but not used [-Wunused-but-set-variable]
      cb_res_in_ddaspk__user__routines_typedef res_cptr;
                                               ^
  build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.c:911:46: warning: variable 'jac_cptr' set but not used [-Wunused-but-set-variable]
      cb_jac_in_ddaspk__user__routines_typedef jac_cptr;
                                               ^
  9 warnings generated.
  INFO: compiling Fortran sources
  INFO: Fortran f77 compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -fPIC -O3 -funroll-loops
  Fortran f90 compiler: /opt/homebrew/bin/gfortran -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
  Fortran fix compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
  creating build/temp.macosx-14.0-arm64-cpython-311/scikits
  creating build/temp.macosx-14.0-arm64-cpython-311/scikits/odes
  creating build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack
  INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: gfortran:f77: scikits/odes/daepack/ddaspk.f
  INFO: gfortran:f77: scikits/odes/daepack/solsy.f
  INFO: gfortran:f77: scikits/odes/daepack/stodi.f
  INFO: gfortran:f77: scikits/odes/daepack/cfode.f
  INFO: gfortran:f77: scikits/odes/daepack/prepji.f
  INFO: gfortran:f77: scikits/odes/daepack/xerrwv.f
  INFO: gfortran:f77: scikits/odes/daepack/lsodi.f
  INFO: gfortran:f77: scikits/odes/daepack/ainvg.f
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/solsy.f:55:72:

     55 |  320    wm(i+2) = 1.0d0/di
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/solsy.f:57:72:

     57 |  340    x(i) = wm(i+2)*x(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
  scikits/odes/daepack/prepji.f:70:72:

     70 |  110    wm(i+2) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/prepji.f:74:72:

     74 |  120    wm(i+2) = wm(i+2)*con
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
  scikits/odes/daepack/prepji.f:93:72:

     93 |  220      wm(i+j1) = (rtem(i) - savr(i))*fac
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  scikits/odes/daepack/prepji.f:122:72:

    122 |  410    wm(i+2) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
  scikits/odes/daepack/stodi.f:162:72:

    162 |  125    el(i) = elco(i,nq)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
  scikits/odes/daepack/prepji.f:126:72:

    126 |  420    wm(i+2) = wm(i+2)*con
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 420 at (1)
  scikits/odes/daepack/stodi.f:183:72:

    183 |  155    el(i) = elco(i,nq)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 155 at (1)
  scikits/odes/daepack/prepji.f:146:72:

    146 |  530      y(i) = y(i) + r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
  scikits/odes/daepack/stodi.f:206:72:

    206 |         do 180 i = 1,n
        |                                                                        1
  Warning: Fortran 2018 deleted feature: Shared DO termination label 180 at (1)
  scikits/odes/daepack/prepji.f:159:72:

    159 |  540        wm(ii+i) = (rtem(i) - savr(i))*fac
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 540 at (1)
  scikits/odes/daepack/stodi.f:207:72:

    207 |  180      yh(i,j) = yh(i,j)*r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
  scikits/odes/daepack/stodi.f:228:72:

    228 |  210      yh1(i) = yh1(i) + yh1(i+nyh)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
  scikits/odes/daepack/stodi.f:239:72:

    239 |  230    y(i) = yh(i,1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 230 at (1)
  scikits/odes/daepack/stodi.f:262:72:

    262 |  260    acor(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
  scikits/odes/daepack/stodi.f:276:72:

    276 |  380    y(i) = yh(i,1) + el1h*acor(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
  scikits/odes/daepack/stodi.f:314:72:

    314 |  440      yh1(i) = yh1(i) - yh1(i+nyh)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 440 at (1)
  scikits/odes/daepack/stodi.f:353:72:

    353 |         do 470 i = 1,n
        |                                                                        1
  Warning: Fortran 2018 deleted feature: Shared DO termination label 470 at (1)
  scikits/odes/daepack/stodi.f:354:72:

    354 |  470      yh(i,j) = yh(i,j) + eljh*acor(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 470 at (1)
  scikits/odes/daepack/stodi.f:360:72:

    360 |  490    yh(i,lmax) = acor(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 490 at (1)
  scikits/odes/daepack/stodi.f:376:72:

    376 |  510      yh1(i) = yh1(i) - yh1(i+nyh)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
  scikits/odes/daepack/stodi.f:396:72:

    396 |  530    savf(i) = acor(i) - yh(i,lmax)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
  scikits/odes/daepack/solsy.f:1:39:

      1 |       subroutine solsy (wm, iwm, x, tem)
        |                                       1
  Warning: Unused dummy argument 'tem' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/stodi.f:423:72:

    423 |  600    yh(i,newq+1) = acor(i)*r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 600 at (1)
  scikits/odes/daepack/stodi.f:454:72:

    454 |  710    acor(i) = acor(i)*r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 710 at (1)
  scikits/odes/daepack/cfode.f:62:72:

     62 |  110      pc(i) = pc(i-1) + fnqm1*pc(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/cfode.f:71:72:

     71 |  120      xpin = xpin + tsign*pc(i)/dfloat(i+1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
  scikits/odes/daepack/cfode.f:76:72:

     76 |  130      elco(i+1,nq) = rq1fac*pc(i)/dfloat(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
  scikits/odes/daepack/cfode.f:99:72:

     99 |  210      pc(i) = pc(i-1) + fnq*pc(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
  scikits/odes/daepack/cfode.f:103:72:

    103 |  220      elco(i,nq) = pc(i)/pc(2)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ddaspk.f:1696:72:

   1696 |  305      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 305 at (1)
  scikits/odes/daepack/ddaspk.f:1822:72:

   1822 |  357      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 357 at (1)
  scikits/odes/daepack/ddaspk.f:1861:72:

   1861 | 380      RWORK(ITEMP + I - 1) = H*YPRIME(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ainvg.f:27:72:

     27 |    10    pw(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ainvg.f:47:72:

     47 |   110    pw(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/ddaspk.f:2018:72:

   2018 |  515      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 515 at (1)
  scikits/odes/daepack/ddaspk.f:2035:72:

   2035 | 524        ATOL(I)=R*ATOL(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 524 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/lsodi.f:1282:72:

   1282 |  80     ydoti(i) = rwork(i+lwm-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
  scikits/odes/daepack/lsodi.f:1291:72:

   1291 |  95     rwork(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
  scikits/odes/daepack/lsodi.f:1333:72:

   1333 |  115        rwork(i+lyh-1) = y(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 115 at (1)
  scikits/odes/daepack/lsodi.f:1338:72:

   1338 |  125        rwork(i+lyd0-1) = ydoti(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
  scikits/odes/daepack/lsodi.f:1346:72:

   1346 |  135    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 135 at (1)
  scikits/odes/daepack/lsodi.f:1370:72:

   1370 |  140    tol = dmax1(tol,rtol(i))
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 140 at (1)
  scikits/odes/daepack/lsodi.f:1391:72:

   1391 |  190    rwork(i+lyd0-1) = h0*rwork(i+lyd0-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 190 at (1)
  scikits/odes/daepack/xerrwv.f:1:40:

      1 |       subroutine xerrwv (msg, nmes, nerr, level, ni, i1, i2, nr, r1, r2)
        |                                        1
  Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/lsodi.f:1442:72:

   1442 |  260    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
  scikits/odes/daepack/lsodi.f:1518:72:

   1518 |  410    y(i) = rwork(i+lyh-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
  scikits/odes/daepack/lsodi.f:1626:72:

   1626 |  585     y(i) = rwork(i+lyh-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 585 at (1)
  scikits/odes/daepack/lsodi.f:1638:72:

   1638 |  592    y(i) = rwork(i+lyh-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 592 at (1)
  scikits/odes/daepack/ddaspk.f:2765:72:

   2765 | 260         PHI(I,J)=BETA(J)*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
  scikits/odes/daepack/ddaspk.f:2816:72:

   2816 | 405     DELTA(I) = PHI(I,KP1) + E(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 405 at (1)
  scikits/odes/daepack/ddaspk.f:2824:72:

   2824 | 415     DELTA(I) = PHI(I,K) + DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 415 at (1)
  scikits/odes/daepack/ddaspk.f:2873:72:

   2873 | 510      DELTA(I)=E(I)-PHI(I,KP2)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
  scikits/odes/daepack/ddaspk.f:2923:72:

   2923 | 580      PHI(I,KP2)=E(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 580 at (1)
  scikits/odes/daepack/ddaspk.f:2926:72:

   2926 | 590      PHI(I,KP1)=PHI(I,KP1)+E(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 590 at (1)
  scikits/odes/daepack/ddaspk.f:2929:72:

   2929 |          DO 595 I=1,NEQ
        |                                                                        1
  Warning: Fortran 2018 deleted feature: Shared DO termination label 595 at (1)
  scikits/odes/daepack/ddaspk.f:2930:72:

   2930 | 595      PHI(I,J)=PHI(I,J)+PHI(I,J+1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 595 at (1)
  scikits/odes/daepack/ddaspk.f:2955:72:

   2955 | 610         PHI(I,J)=TEMP1*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 610 at (1)
  scikits/odes/daepack/ddaspk.f:2959:72:

   2959 | 640      PSI(I-1)=PSI(I)-H
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 640 at (1)
  scikits/odes/daepack/ddaspk.f:3051:72:

   3051 | 695       PHI(I,2) = R*PHI(I,2)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 695 at (1)
  scikits/odes/daepack/ddaspk.f:3302:72:

   3302 |  20     WT(I) = 1.0D0/WT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:3349:72:

   3349 | 10       YPOUT(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ddaspk.f:3359:72:

   3359 | 20          YPOUT(I)=YPOUT(I)+D*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:3391:72:

   3391 | 20      SUM = SUM + ((V(I)*RWT(I))/VMAX)**2
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:3826:72:

   3826 |  20           P(I) = P(I)*RATIO1
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:4136:72:

   4136 | 310      YPRIME(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
  scikits/odes/daepack/ddaspk.f:4140:72:

   4140 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:4191:72:

   4191 | 377      DELTA(I) = MIN(Y(I),0.0D0)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 377 at (1)
  scikits/odes/daepack/ddaspk.f:4195:72:

   4195 | 378      E(I) = E(I) - DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 378 at (1)
  scikits/odes/daepack/ddaspk.f:4313:72:

   4313 | 100     E(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
  scikits/odes/daepack/ddaspk.f:4324:72:

   4324 | 320        DELTA(I) = DELTA(I) * CONFAC
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:4337:72:

   4337 | 340      YPRIME(I)=YPRIME(I)-CJ*DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
  scikits/odes/daepack/ddaspk.f:4461:72:

   4461 | 110      WM(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/ddaspk.f:4485:72:

   4485 | 220        WM(NROW+L)=(E(L)-DELTA(L))*DELINV
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  scikits/odes/daepack/ddaspk.f:4507:72:

   4507 | 410      WM(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
  scikits/odes/daepack/ddaspk.f:4534:72:

   4534 | 510       YPRIME(N)=YPRIME(N)+CJ*DEL
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
  scikits/odes/daepack/ddaspk.f:4551:72:

   4551 | 520         WM(II+I)=(E(I)-DELTA(I))*DELINV
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 520 at (1)
  scikits/odes/daepack/ddaspk.f:5091:72:

   5091 |  20           P(I) = P(I)*RATIO1
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:5421:72:

   5421 | 310      YPRIME(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
  scikits/odes/daepack/ddaspk.f:5425:72:

   5425 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:5475:72:

   5475 |  360    DELTA(I) = MIN(Y(I),0.0D0)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
  scikits/odes/daepack/ddaspk.f:5479:72:

   5479 |  370    E(I) = E(I) - DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 370 at (1)
  scikits/odes/daepack/ddaspk.f:5606:72:

   5606 | 100     E(I) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
  scikits/odes/daepack/ddaspk.f:5617:72:

   5617 | 320       DELTA(I) = DELTA(I) * CONFAC
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:5623:72:

   5623 | 340     SAVR(I) = DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
  scikits/odes/daepack/ddaspk.f:5637:72:

   5637 | 360      YPRIME(I) = YPRIME(I) - CJ*DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
  scikits/odes/daepack/ddaspk.f:5769:72:

   5769 |  110     X(I) = 0.D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/ddaspk.f:5793:72:

   5793 |  120     X(I) = X(I) + WM(LZ+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
  scikits/odes/daepack/ddaspk.f:5964:72:

   5964 |  10     Z(I) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ddaspk.f:5975:72:

   5975 |  30         V(I,1) = R(I)*WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
  scikits/odes/daepack/ddaspk.f:5978:72:

   5978 |  35         V(I,1) = R(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
  scikits/odes/daepack/ddaspk.f:5997:72:

   5997 |  60       HES(I,J) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
  scikits/odes/daepack/ddaspk.f:6038:72:

   6038 |  70             DL(K) = S*DL(K) + C*V(K,IP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
  scikits/odes/daepack/ddaspk.f:6045:72:

   6045 |  80         DL(K) = S*DL(K) + C*V(K,LLP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
  scikits/odes/daepack/ddaspk.f:6066:72:

   6066 |  130     Z(I) = 0.D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
  scikits/odes/daepack/ddaspk.f:6088:72:

   6088 |  170              DL(K) = S*DL(K) + C*V(K,IP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 170 at (1)
  scikits/odes/daepack/ddaspk.f:6093:72:

   6093 |  180           DL(K) = S*DL(K) + C*V(K,MAXLP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
  scikits/odes/daepack/ddaspk.f:6110:72:

   6110 |  210    R(K) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
  scikits/odes/daepack/ddaspk.f:6114:72:

   6114 |  220    Z(K) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  scikits/odes/daepack/ddaspk.f:6119:72:

   6119 |  240    Z(I) = Z(I)/WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 240 at (1)
  scikits/odes/daepack/ddaspk.f:6221:72:

   6221 |  10     VTEM(I) = V(I)/WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ddaspk.f:6229:72:

   6229 |  20     Z(I) = Y(I) + VTEM(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:6243:72:

   6243 |  70     Z(I) = VTEM(I) - SAVR(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
  scikits/odes/daepack/ddaspk.f:6255:72:

   6255 |  90     Z(I) = Z(I)*WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 90 at (1)
  scikits/odes/daepack/ddaspk.f:2369:3:

   2369 | 770   MSG = 'DASPK--  RUN TERMINATED. APPARENT INFINITE LOOP'
        |   1
  Warning: Label 770 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/ddaspk.f:1685:3:

   1685 | 300   CONTINUE
        |   1
  Warning: Label 300 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/ddaspk.f:4189:3:

   4189 | 375   IF(NONNEG .EQ. 0) GO TO 390
        |   1
  Warning: Label 375 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/ddaspk.f:3365:58:

   3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
        |                                                          1
  Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3365:53:

   3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
        |                                                     1
  Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4982:22:

   4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
        |                      1
  Warning: Unused dummy argument 'rhok' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4982:11:

   4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
        |           1
  Warning: Unused dummy argument 'wm' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:48:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |                                                1
  Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:43:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |                                           1
  Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:38:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |                                      1
  Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:15:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |               1
  Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4222:47:

   4222 |      *   S,CONFAC,TOLNEW,MULDEL,MAXIT,IRES,IDUM,IERNEW)
        |                                               1
  Warning: Unused dummy argument 'idum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4220:45:

   4220 |       SUBROUTINE DNSD(X,Y,YPRIME,NEQ,RES,PDUM,WT,RPAR,IPAR,
        |                                             1
  Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3248:56:

   3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
        |                                                        1
  Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3248:51:

   3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
        |                                                   1
  Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:19:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                   1
  Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3399:57:

   3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
        |                                                         1
  Warning: Unused dummy argument 'dumpwk' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:29:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                             1
  Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:24:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                        1
  Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3399:33:

   3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
        |                                 1
  Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:55:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                                                       1
  Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3399:16:

   3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
        |                1
  Warning: Unused dummy argument 'jsdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3398:61:

   3398 |       SUBROUTINE DDASID(X,Y,YPRIME,NEQ,ICOPT,ID,RES,JACD,PDUM,H,TSCALE,
        |                                                             1
  Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3957:26:

   3957 |      *   EPCON,JCALC,JFDUM,KP1,NONNEG,NTYPE,IERNLS)
        |                          1
  Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4617:70:

   4617 |      *   WT,JSKIP,RPAR,IPAR,SAVR,DELTA,R,YIC,YPIC,PWK,WM,IWM,CJ,UROUND,
        |                                                                      1
  Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:5237:40:

   5237 |      *   WM,IWM,CJ,CJOLD,CJLAST,S,UROUND,EPLI,SQRTN,RSQRTN,
        |                                        1
  Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/lsodi.f:1521:10:

   1521 |       if (ihit) t = tcrit
        |          ^
  Warning: 'ihit' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/lsodi.f:1150:18:

   1150 |       logical ihit
        |                  ^
  note: 'ihit' was declared here
  INFO: gfortran:f77: scikits/odes/daepack/ewset.f
  INFO: gfortran:f77: scikits/odes/daepack/dlinpk.f
  INFO: gfortran:f77: scikits/odes/daepack/daux.f
  INFO: gfortran:f77: scikits/odes/daepack/intdy.f
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ewset.f:17:72:

     17 |  15     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 15 at (1)
  scikits/odes/daepack/ewset.f:21:72:

     21 |  25     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 25 at (1)
  scikits/odes/daepack/ewset.f:25:72:

     25 |  35     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
  scikits/odes/daepack/ewset.f:29:72:

     29 |  45     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 45 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/daux.f:2:46:

      2 |       DOUBLE PRECISION FUNCTION D1MACH (IDUMMY)
        |                                              1
  Warning: Unused dummy argument 'idummy' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/daux.f:48:40:

     48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
        |                                        1
  Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/daux.f:48:34:

     48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
        |                                  1
  Warning: Unused dummy argument 'nmes' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/intdy.f:48:72:

     48 |  10     ic = ic*jj
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/intdy.f:51:72:

     51 |  20     dky(i) = c*yh(i,l)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/intdy.f:61:72:

     61 |  30       ic = ic*jj
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
  scikits/odes/daepack/intdy.f:64:72:

     64 |  40       dky(i) = c*yh(i,jp1) + s*dky(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 40 at (1)
  scikits/odes/daepack/intdy.f:69:72:

     69 |  60     dky(i) = r*dky(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
  scikits/odes/daepack/daux.f:214:28:

    214 |       INTEGER FUNCTION IXSAV (IPAR, IVALUE, ISET)
        |                            ^
  Warning: '__result_ixsav' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/dlinpk.f:763:72:

    763 |    10 assign 30 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:768:20:

    768 |    20    go to next,(30, 50, 70, 110)
        |                    1
  Warning: Deleted feature: Assigned GOTO statement at (1)
  scikits/odes/daepack/dlinpk.f:770:72:

    770 |       assign 50 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:779:72:

    779 |       assign 70 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:785:72:

    785 |       assign 110 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:821:72:

    821 |    95    sum = sum + dx(j)**2
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
  scikits/odes/daepack/dlinpk.f:798:5:

    798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
        |     1
  Warning: Label 110 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/dlinpk.f:793:5:

    793 |    70 if( dabs(dx(i)) .gt. cutlo ) go to 75
        |     1
  Warning: Label 70 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/dlinpk.f:775:5:

    775 |    50 if( dx(i) .eq. zero) go to 200
        |     1
  Warning: Label 50 at (1) defined but not used [-Wunused-label]
  INFO: gfortran:f77: scikits/odes/daepack/vnorm.f
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/vnorm.f:14:72:

     14 |  10     sum = sum + (v(i)*w(i))**2
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  INFO: gfortran:f77: build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspk-f2pywrappers.f
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ddaspk.f:6062:10:

   6062 |       IF (RHO .LT. RNRM) GO TO 150
        |          ^
  Warning: 'rho' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:5951:44:

   5951 |       DOUBLE PRECISION RNRM,C,DLNRM,PROD,RHO,S,SNORMW,DNRM2,TEM
        |                                            ^
  note: 'rho' was declared here
  scikits/odes/daepack/dlinpk.f:798:10:

    798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
        |          ^
  Warning: 'xmax' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/dlinpk.f:717:63:

    717 |       double precision   dx(1), cutlo, cuthi, hitest, sum, xmax,zero,one
        |                                                               ^
  note: 'xmax' was declared here
  scikits/odes/daepack/ddaspk.f:2908:72:

   2908 |       R=(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
        |                                                                        ^
  Warning: 'erkm1' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:2817:11:

   2817 |       ERKM1=SIGMA(K)*DDWNRM(NEQ,DELTA,VT,RPAR,IPAR)
        |           ^
  note: 'erkm1' was declared here
  scikits/odes/daepack/ddaspk.f:2998:72:

   2998 |       R = 0.90D0*(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
        |                                                                        ^
  Warning: 'est' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:2812:9:

   2812 |       EST = ERK
        |         ^
  note: 'est' was declared here
  scikits/odes/daepack/ddaspk.f:2997:72:

   2997 |       TEMP2 = K + 1
        |                                                                        ^
  Warning: 'knew' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:2813:10:

   2813 |       KNEW=K
        |          ^
  note: 'knew' was declared here
  scikits/odes/daepack/ddaspk.f:1771:10:

   1771 |       IF (INDEX .EQ. 0) TSCALE = TDIST
        |          ^
  Warning: 'index' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:1572:11:

   1572 |       INDEX = 1
        |           ^
  note: 'index' was declared here
  INFO: /opt/homebrew/bin/gfortran -Wall -g -Wall -g -undefined dynamic_lookup -bundle build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspkmodule.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ddaspk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/solsy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/stodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/cfode.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/xerrwv.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/prepji.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/lsodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ainvg.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ewset.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/daux.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/dlinpk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/intdy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/vnorm.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/ddaspk-f2pywrappers.o -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13 -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -lgfortran -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/ddaspk.cpython-311-darwin.so
  ld: warning: ignoring duplicate libraries: '-lgfortran'
  INFO: building 'scikits.odes.lsodi' extension
  INFO: compiling C sources
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodimodule.c
  INFO: compiling Fortran sources
  INFO: Fortran f77 compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -fPIC -O3 -funroll-loops
  Fortran f90 compiler: /opt/homebrew/bin/gfortran -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
  Fortran fix compiler: /opt/homebrew/bin/gfortran -Wall -g -ffixed-form -fno-second-underscore -Wall -g -fno-second-underscore -fPIC -O3 -funroll-loops
  INFO: compile options: '-Ibuild/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: gfortran:f77: scikits/odes/daepack/ddaspk.f
  INFO: gfortran:f77: scikits/odes/daepack/solsy.f
  INFO: gfortran:f77: scikits/odes/daepack/stodi.f
  INFO: gfortran:f77: scikits/odes/daepack/xerrwv.f
  INFO: gfortran:f77: scikits/odes/daepack/cfode.f
  INFO: gfortran:f77: scikits/odes/daepack/lsodi.f
  INFO: gfortran:f77: scikits/odes/daepack/prepji.f
  INFO: gfortran:f77: scikits/odes/daepack/ainvg.f
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/stodi.f:162:72:

    162 |  125    el(i) = elco(i,nq)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
  scikits/odes/daepack/stodi.f:183:72:

    183 |  155    el(i) = elco(i,nq)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 155 at (1)
  scikits/odes/daepack/stodi.f:206:72:

    206 |         do 180 i = 1,n
        |                                                                        1
  Warning: Fortran 2018 deleted feature: Shared DO termination label 180 at (1)
  scikits/odes/daepack/stodi.f:207:72:

    207 |  180      yh(i,j) = yh(i,j)*r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
  scikits/odes/daepack/stodi.f:228:72:

    228 |  210      yh1(i) = yh1(i) + yh1(i+nyh)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
  scikits/odes/daepack/stodi.f:239:72:

    239 |  230    y(i) = yh(i,1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 230 at (1)
  scikits/odes/daepack/stodi.f:262:72:

    262 |  260    acor(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
  scikits/odes/daepack/stodi.f:276:72:

    276 |  380    y(i) = yh(i,1) + el1h*acor(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
  scikits/odes/daepack/stodi.f:314:72:

    314 |  440      yh1(i) = yh1(i) - yh1(i+nyh)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 440 at (1)
  scikits/odes/daepack/stodi.f:353:72:

    353 |         do 470 i = 1,n
        |                                                                        1
  Warning: Fortran 2018 deleted feature: Shared DO termination label 470 at (1)
  scikits/odes/daepack/stodi.f:354:72:

    354 |  470      yh(i,j) = yh(i,j) + eljh*acor(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 470 at (1)
  scikits/odes/daepack/stodi.f:360:72:

    360 |  490    yh(i,lmax) = acor(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 490 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/stodi.f:376:72:

    376 |  510      yh1(i) = yh1(i) - yh1(i+nyh)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
  scikits/odes/daepack/stodi.f:396:72:

    396 |  530    savf(i) = acor(i) - yh(i,lmax)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
  scikits/odes/daepack/stodi.f:423:72:

    423 |  600    yh(i,newq+1) = acor(i)*r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 600 at (1)
  scikits/odes/daepack/stodi.f:454:72:

    454 |  710    acor(i) = acor(i)*r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 710 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/solsy.f:55:72:

     55 |  320    wm(i+2) = 1.0d0/di
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/solsy.f:57:72:

     57 |  340    x(i) = wm(i+2)*x(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
  scikits/odes/daepack/solsy.f:1:39:

      1 |       subroutine solsy (wm, iwm, x, tem)
        |                                       1
  Warning: Unused dummy argument 'tem' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/cfode.f:62:72:

     62 |  110      pc(i) = pc(i-1) + fnqm1*pc(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/cfode.f:71:72:

     71 |  120      xpin = xpin + tsign*pc(i)/dfloat(i+1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
  scikits/odes/daepack/cfode.f:76:72:

     76 |  130      elco(i+1,nq) = rq1fac*pc(i)/dfloat(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
  scikits/odes/daepack/cfode.f:99:72:

     99 |  210      pc(i) = pc(i-1) + fnq*pc(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
  scikits/odes/daepack/cfode.f:103:72:

    103 |  220      elco(i,nq) = pc(i)/pc(2)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  scikits/odes/daepack/xerrwv.f:1:40:

      1 |       subroutine xerrwv (msg, nmes, nerr, level, ni, i1, i2, nr, r1, r2)
        |                                        1
  Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:1696:72:

   1696 |  305      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 305 at (1)
  scikits/odes/daepack/ddaspk.f:1822:72:

   1822 |  357      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 357 at (1)
  scikits/odes/daepack/ddaspk.f:1861:72:

   1861 | 380      RWORK(ITEMP + I - 1) = H*YPRIME(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 380 at (1)
  scikits/odes/daepack/ddaspk.f:2018:72:

   2018 |  515      RWORK(LVT+I-1) = MAX(IWORK(LID+I-1),0)*RWORK(LWT+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 515 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ddaspk.f:2035:72:

   2035 | 524        ATOL(I)=R*ATOL(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 524 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ainvg.f:27:72:

     27 |    10    pw(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ainvg.f:47:72:

     47 |   110    pw(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/prepji.f:70:72:

     70 |  110    wm(i+2) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/prepji.f:74:72:

     74 |  120    wm(i+2) = wm(i+2)*con
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
  scikits/odes/daepack/prepji.f:93:72:

     93 |  220      wm(i+j1) = (rtem(i) - savr(i))*fac
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  scikits/odes/daepack/prepji.f:122:72:

    122 |  410    wm(i+2) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
  scikits/odes/daepack/prepji.f:126:72:

    126 |  420    wm(i+2) = wm(i+2)*con
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 420 at (1)
  scikits/odes/daepack/prepji.f:146:72:

    146 |  530      y(i) = y(i) + r
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 530 at (1)
  scikits/odes/daepack/prepji.f:159:72:

    159 |  540        wm(ii+i) = (rtem(i) - savr(i))*fac
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 540 at (1)
  scikits/odes/daepack/lsodi.f:1282:72:

   1282 |  80     ydoti(i) = rwork(i+lwm-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
  scikits/odes/daepack/lsodi.f:1291:72:

   1291 |  95     rwork(i) = 0.0d0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
  scikits/odes/daepack/lsodi.f:1333:72:

   1333 |  115        rwork(i+lyh-1) = y(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 115 at (1)
  scikits/odes/daepack/lsodi.f:1338:72:

   1338 |  125        rwork(i+lyd0-1) = ydoti(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 125 at (1)
  scikits/odes/daepack/lsodi.f:1346:72:

   1346 |  135    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 135 at (1)
  scikits/odes/daepack/lsodi.f:1370:72:

   1370 |  140    tol = dmax1(tol,rtol(i))
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 140 at (1)
  scikits/odes/daepack/lsodi.f:1391:72:

   1391 |  190    rwork(i+lyd0-1) = h0*rwork(i+lyd0-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 190 at (1)
  scikits/odes/daepack/lsodi.f:1442:72:

   1442 |  260    rwork(i+lewt-1) = 1.0d0/rwork(i+lewt-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
  scikits/odes/daepack/lsodi.f:1518:72:

   1518 |  410    y(i) = rwork(i+lyh-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
  scikits/odes/daepack/lsodi.f:1626:72:

   1626 |  585     y(i) = rwork(i+lyh-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 585 at (1)
  scikits/odes/daepack/lsodi.f:1638:72:

   1638 |  592    y(i) = rwork(i+lyh-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 592 at (1)
  scikits/odes/daepack/ddaspk.f:2765:72:

   2765 | 260         PHI(I,J)=BETA(J)*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 260 at (1)
  scikits/odes/daepack/ddaspk.f:2816:72:

   2816 | 405     DELTA(I) = PHI(I,KP1) + E(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 405 at (1)
  scikits/odes/daepack/ddaspk.f:2824:72:

   2824 | 415     DELTA(I) = PHI(I,K) + DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 415 at (1)
  scikits/odes/daepack/ddaspk.f:2873:72:

   2873 | 510      DELTA(I)=E(I)-PHI(I,KP2)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
  scikits/odes/daepack/ddaspk.f:2923:72:

   2923 | 580      PHI(I,KP2)=E(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 580 at (1)
  scikits/odes/daepack/ddaspk.f:2926:72:

   2926 | 590      PHI(I,KP1)=PHI(I,KP1)+E(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 590 at (1)
  scikits/odes/daepack/ddaspk.f:2929:72:

   2929 |          DO 595 I=1,NEQ
        |                                                                        1
  Warning: Fortran 2018 deleted feature: Shared DO termination label 595 at (1)
  scikits/odes/daepack/ddaspk.f:2930:72:

   2930 | 595      PHI(I,J)=PHI(I,J)+PHI(I,J+1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 595 at (1)
  scikits/odes/daepack/ddaspk.f:2955:72:

   2955 | 610         PHI(I,J)=TEMP1*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 610 at (1)
  scikits/odes/daepack/ddaspk.f:2959:72:

   2959 | 640      PSI(I-1)=PSI(I)-H
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 640 at (1)
  scikits/odes/daepack/ddaspk.f:3051:72:

   3051 | 695       PHI(I,2) = R*PHI(I,2)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 695 at (1)
  scikits/odes/daepack/ddaspk.f:3302:72:

   3302 |  20     WT(I) = 1.0D0/WT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:3349:72:

   3349 | 10       YPOUT(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ddaspk.f:3359:72:

   3359 | 20          YPOUT(I)=YPOUT(I)+D*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:3391:72:

   3391 | 20      SUM = SUM + ((V(I)*RWT(I))/VMAX)**2
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:3826:72:

   3826 |  20           P(I) = P(I)*RATIO1
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:4136:72:

   4136 | 310      YPRIME(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
  scikits/odes/daepack/ddaspk.f:4140:72:

   4140 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:4191:72:

   4191 | 377      DELTA(I) = MIN(Y(I),0.0D0)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 377 at (1)
  scikits/odes/daepack/ddaspk.f:4195:72:

   4195 | 378      E(I) = E(I) - DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 378 at (1)
  scikits/odes/daepack/ddaspk.f:4313:72:

   4313 | 100     E(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
  scikits/odes/daepack/ddaspk.f:4324:72:

   4324 | 320        DELTA(I) = DELTA(I) * CONFAC
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:4337:72:

   4337 | 340      YPRIME(I)=YPRIME(I)-CJ*DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
  scikits/odes/daepack/ddaspk.f:4461:72:

   4461 | 110      WM(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/ddaspk.f:4485:72:

   4485 | 220        WM(NROW+L)=(E(L)-DELTA(L))*DELINV
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  scikits/odes/daepack/ddaspk.f:4507:72:

   4507 | 410      WM(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 410 at (1)
  scikits/odes/daepack/ddaspk.f:4534:72:

   4534 | 510       YPRIME(N)=YPRIME(N)+CJ*DEL
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 510 at (1)
  scikits/odes/daepack/ddaspk.f:4551:72:

   4551 | 520         WM(II+I)=(E(I)-DELTA(I))*DELINV
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 520 at (1)
  scikits/odes/daepack/ddaspk.f:5091:72:

   5091 |  20           P(I) = P(I)*RATIO1
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:5421:72:

   5421 | 310      YPRIME(I)=0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 310 at (1)
  scikits/odes/daepack/ddaspk.f:5425:72:

   5425 | 320         YPRIME(I)=YPRIME(I)+GAMMA(J)*PHI(I,J)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:5475:72:

   5475 |  360    DELTA(I) = MIN(Y(I),0.0D0)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
  scikits/odes/daepack/ddaspk.f:5479:72:

   5479 |  370    E(I) = E(I) - DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 370 at (1)
  scikits/odes/daepack/ddaspk.f:5606:72:

   5606 | 100     E(I) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 100 at (1)
  scikits/odes/daepack/ddaspk.f:5617:72:

   5617 | 320       DELTA(I) = DELTA(I) * CONFAC
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 320 at (1)
  scikits/odes/daepack/ddaspk.f:5623:72:

   5623 | 340     SAVR(I) = DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 340 at (1)
  scikits/odes/daepack/ddaspk.f:5637:72:

   5637 | 360      YPRIME(I) = YPRIME(I) - CJ*DELTA(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 360 at (1)
  scikits/odes/daepack/ddaspk.f:5769:72:

   5769 |  110     X(I) = 0.D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 110 at (1)
  scikits/odes/daepack/ddaspk.f:5793:72:

   5793 |  120     X(I) = X(I) + WM(LZ+I-1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 120 at (1)
  scikits/odes/daepack/ddaspk.f:5964:72:

   5964 |  10     Z(I) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ddaspk.f:5975:72:

   5975 |  30         V(I,1) = R(I)*WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
  scikits/odes/daepack/ddaspk.f:5978:72:

   5978 |  35         V(I,1) = R(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
  scikits/odes/daepack/ddaspk.f:5997:72:

   5997 |  60       HES(I,J) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
  scikits/odes/daepack/ddaspk.f:6038:72:

   6038 |  70             DL(K) = S*DL(K) + C*V(K,IP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
  scikits/odes/daepack/ddaspk.f:6045:72:

   6045 |  80         DL(K) = S*DL(K) + C*V(K,LLP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 80 at (1)
  scikits/odes/daepack/ddaspk.f:6066:72:

   6066 |  130     Z(I) = 0.D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 130 at (1)
  scikits/odes/daepack/ddaspk.f:6088:72:

   6088 |  170              DL(K) = S*DL(K) + C*V(K,IP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 170 at (1)
  scikits/odes/daepack/ddaspk.f:6093:72:

   6093 |  180           DL(K) = S*DL(K) + C*V(K,MAXLP1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 180 at (1)
  scikits/odes/daepack/ddaspk.f:6110:72:

   6110 |  210    R(K) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 210 at (1)
  scikits/odes/daepack/ddaspk.f:6114:72:

   6114 |  220    Z(K) = 0.0D0
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 220 at (1)
  scikits/odes/daepack/ddaspk.f:6119:72:

   6119 |  240    Z(I) = Z(I)/WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 240 at (1)
  scikits/odes/daepack/ddaspk.f:6221:72:

   6221 |  10     VTEM(I) = V(I)/WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/ddaspk.f:6229:72:

   6229 |  20     Z(I) = Y(I) + VTEM(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/ddaspk.f:6243:72:

   6243 |  70     Z(I) = VTEM(I) - SAVR(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 70 at (1)
  scikits/odes/daepack/ddaspk.f:6255:72:

   6255 |  90     Z(I) = Z(I)*WGHT(I)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 90 at (1)
  scikits/odes/daepack/ddaspk.f:2369:3:

   2369 | 770   MSG = 'DASPK--  RUN TERMINATED. APPARENT INFINITE LOOP'
        |   1
  Warning: Label 770 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/ddaspk.f:1685:3:

   1685 | 300   CONTINUE
        |   1
  Warning: Label 300 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/ddaspk.f:4189:3:

   4189 | 375   IF(NONNEG .EQ. 0) GO TO 390
        |   1
  Warning: Label 375 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/ddaspk.f:3365:58:

   3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
        |                                                          1
  Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3365:53:

   3365 |       DOUBLE PRECISION FUNCTION DDWNRM(NEQ,V,RWT,RPAR,IPAR)
        |                                                     1
  Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4982:22:

   4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
        |                      1
  Warning: Unused dummy argument 'rhok' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4982:11:

   4982 |      *   WM, IWM, RHOK, FNRM, ICOPT, ID, WP, IWP, R, EPLIN, YNEW, YPNEW,
        |           1
  Warning: Unused dummy argument 'wm' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:48:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |                                                1
  Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:43:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |                                           1
  Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:38:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |                                      1
  Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4221:15:

   4221 |      *   DUMSVR,DELTA,E,WM,IWM,CJ,DUMS,DUMR,DUME,EPCON,
        |               1
  Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4222:47:

   4222 |      *   S,CONFAC,TOLNEW,MULDEL,MAXIT,IRES,IDUM,IERNEW)
        |                                               1
  Warning: Unused dummy argument 'idum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4220:45:

   4220 |       SUBROUTINE DNSD(X,Y,YPRIME,NEQ,RES,PDUM,WT,RPAR,IPAR,
        |                                             1
  Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
  INFO: gfortran:f77: scikits/odes/daepack/ewset.f
  scikits/odes/daepack/ddaspk.f:3248:56:

   3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
        |                                                        1
  Warning: Unused dummy argument 'ipar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3248:51:

   3248 |       SUBROUTINE DDAWTS(NEQ,IWT,RTOL,ATOL,Y,WT,RPAR,IPAR)
        |                                                   1
  Warning: Unused dummy argument 'rpar' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:19:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                   1
  Warning: Unused dummy argument 'dume' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3399:57:

   3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
        |                                                         1
  Warning: Unused dummy argument 'dumpwk' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:29:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                             1
  Warning: Unused dummy argument 'dumr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:24:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                        1
  Warning: Unused dummy argument 'dums' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3399:33:

   3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
        |                                 1
  Warning: Unused dummy argument 'dumsvr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3400:55:

   3400 |      *  UROUND,DUME,DUMS,DUMR,EPCON,RATEMX,STPTOL,JFDUM,
        |                                                       1
  Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3399:16:

   3399 |      *  WT,JSDUM,RPAR,IPAR,DUMSVR,DELTA,R,YIC,YPIC,DUMPWK,WM,IWM,CJ,
        |                1
  Warning: Unused dummy argument 'jsdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3398:61:

   3398 |       SUBROUTINE DDASID(X,Y,YPRIME,NEQ,ICOPT,ID,RES,JACD,PDUM,H,TSCALE,
        |                                                             1
  Warning: Unused dummy argument 'pdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:3957:26:

   3957 |      *   EPCON,JCALC,JFDUM,KP1,NONNEG,NTYPE,IERNLS)
        |                          1
  Warning: Unused dummy argument 'jfdum' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:4617:70:

   4617 |      *   WT,JSKIP,RPAR,IPAR,SAVR,DELTA,R,YIC,YPIC,PWK,WM,IWM,CJ,UROUND,
        |                                                                      1
  Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/ddaspk.f:5237:40:

   5237 |      *   WM,IWM,CJ,CJOLD,CJLAST,S,UROUND,EPLI,SQRTN,RSQRTN,
        |                                        1
  Warning: Unused dummy argument 'uround' at (1) [-Wunused-dummy-argument]
  INFO: gfortran:f77: scikits/odes/daepack/daux.f
  INFO: gfortran:f77: scikits/odes/daepack/dlinpk.f
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ewset.f:17:72:

     17 |  15     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 15 at (1)
  scikits/odes/daepack/ewset.f:21:72:

     21 |  25     ewt(i) = rtol(1)*dabs(ycur(i)) + atol(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 25 at (1)
  scikits/odes/daepack/ewset.f:25:72:

     25 |  35     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(1)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 35 at (1)
  scikits/odes/daepack/ewset.f:29:72:

     29 |  45     ewt(i) = rtol(i)*dabs(ycur(i)) + atol(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 45 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/daux.f:2:46:

      2 |       DOUBLE PRECISION FUNCTION D1MACH (IDUMMY)
        |                                              1
  Warning: Unused dummy argument 'idummy' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/daux.f:48:40:

     48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
        |                                        1
  Warning: Unused dummy argument 'nerr' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/daux.f:48:34:

     48 |       SUBROUTINE XERRWD (MSG, NMES, NERR, LEVEL, NI, I1, I2, NR, R1, R2)
        |                                  1
  Warning: Unused dummy argument 'nmes' at (1) [-Wunused-dummy-argument]
  scikits/odes/daepack/daux.f:214:28:

    214 |       INTEGER FUNCTION IXSAV (IPAR, IVALUE, ISET)
        |                            ^
  Warning: '__result_ixsav' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/dlinpk.f:763:72:

    763 |    10 assign 30 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:768:20:

    768 |    20    go to next,(30, 50, 70, 110)
        |                    1
  Warning: Deleted feature: Assigned GOTO statement at (1)
  scikits/odes/daepack/dlinpk.f:770:72:

    770 |       assign 50 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:779:72:

    779 |       assign 70 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:785:72:

    785 |       assign 110 to next
        |                                                                        1
  Warning: Deleted feature: ASSIGN statement at (1)
  scikits/odes/daepack/dlinpk.f:821:72:

    821 |    95    sum = sum + dx(j)**2
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 95 at (1)
  scikits/odes/daepack/dlinpk.f:798:5:

    798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
        |     1
  Warning: Label 110 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/dlinpk.f:793:5:

    793 |    70 if( dabs(dx(i)) .gt. cutlo ) go to 75
        |     1
  Warning: Label 70 at (1) defined but not used [-Wunused-label]
  scikits/odes/daepack/dlinpk.f:775:5:

    775 |    50 if( dx(i) .eq. zero) go to 200
        |     1
  Warning: Label 50 at (1) defined but not used [-Wunused-label]
  INFO: gfortran:f77: scikits/odes/daepack/intdy.f
  scikits/odes/daepack/lsodi.f:1521:10:

   1521 |       if (ihit) t = tcrit
        |          ^
  Warning: 'ihit' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/lsodi.f:1150:18:

   1150 |       logical ihit
        |                  ^
  note: 'ihit' was declared here
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/intdy.f:48:72:

     48 |  10     ic = ic*jj
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  scikits/odes/daepack/intdy.f:51:72:

     51 |  20     dky(i) = c*yh(i,l)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 20 at (1)
  scikits/odes/daepack/intdy.f:61:72:

     61 |  30       ic = ic*jj
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 30 at (1)
  scikits/odes/daepack/intdy.f:64:72:

     64 |  40       dky(i) = c*yh(i,jp1) + s*dky(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 40 at (1)
  scikits/odes/daepack/intdy.f:69:72:

     69 |  60     dky(i) = r*dky(i)
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 60 at (1)
  INFO: gfortran:f77: scikits/odes/daepack/vnorm.f
  INFO: gfortran:f77: build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodi-f2pywrappers.f
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/vnorm.f:14:72:

     14 |  10     sum = sum + (v(i)*w(i))**2
        |                                                                        1
  Warning: Fortran 2018 deleted feature: DO termination statement which is not END DO or CONTINUE with label 10 at (1)
  f951: Warning: Nonexistent include directory '/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include' [-Wmissing-include-dirs]
  scikits/odes/daepack/ddaspk.f:6062:10:

   6062 |       IF (RHO .LT. RNRM) GO TO 150
        |          ^
  Warning: 'rho' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:5951:44:

   5951 |       DOUBLE PRECISION RNRM,C,DLNRM,PROD,RHO,S,SNORMW,DNRM2,TEM
        |                                            ^
  note: 'rho' was declared here
  scikits/odes/daepack/dlinpk.f:798:10:

    798 |   110 if( dabs(dx(i)) .le. xmax ) go to 115
        |          ^
  Warning: 'xmax' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/dlinpk.f:717:63:

    717 |       double precision   dx(1), cutlo, cuthi, hitest, sum, xmax,zero,one
        |                                                               ^
  note: 'xmax' was declared here
  scikits/odes/daepack/ddaspk.f:2908:72:

   2908 |       R=(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
        |                                                                        ^
  Warning: 'erkm1' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:2817:11:

   2817 |       ERKM1=SIGMA(K)*DDWNRM(NEQ,DELTA,VT,RPAR,IPAR)
        |           ^
  note: 'erkm1' was declared here
  scikits/odes/daepack/ddaspk.f:2998:72:

   2998 |       R = 0.90D0*(2.0D0*EST+0.0001D0)**(-1.0D0/TEMP2)
        |                                                                        ^
  Warning: 'est' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:2812:9:

   2812 |       EST = ERK
        |         ^
  note: 'est' was declared here
  scikits/odes/daepack/ddaspk.f:2997:72:

   2997 |       TEMP2 = K + 1
        |                                                                        ^
  Warning: 'knew' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:2813:10:

   2813 |       KNEW=K
        |          ^
  note: 'knew' was declared here
  scikits/odes/daepack/ddaspk.f:1771:10:

   1771 |       IF (INDEX .EQ. 0) TSCALE = TDIST
        |          ^
  Warning: 'index' may be used uninitialized [-Wmaybe-uninitialized]
  scikits/odes/daepack/ddaspk.f:1572:11:

   1572 |       INDEX = 1
        |           ^
  note: 'index' was declared here
  INFO: /opt/homebrew/bin/gfortran -Wall -g -Wall -g -undefined dynamic_lookup -bundle build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodimodule.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/build/src.macosx-14.0-arm64-3.11/scikits/odes/fortranobject.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ddaspk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/solsy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/stodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/cfode.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/xerrwv.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/prepji.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/lsodi.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ainvg.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/ewset.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/daux.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/dlinpk.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/intdy.o build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/daepack/vnorm.o build/temp.macosx-14.0-arm64-cpython-311/build/src.macosx-14.0-arm64-3.11/scikits/odes/lsodi-f2pywrappers.o -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13 -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -L/opt/homebrew/Cellar/gcc/13.2.0/bin/../lib/gcc/current/gcc/aarch64-apple-darwin23/13/../../.. -lgfortran -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/lsodi.cpython-311-darwin.so
  ld: warning: ignoring duplicate libraries: '-lgfortran'
  INFO: building 'scikits.odes.sundials.common_defs' extension
  INFO: compiling C sources
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  creating build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials
  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: scikits/odes/sundials/common_defs.c
  In file included from scikits/odes/sundials/common_defs.c:800:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/arrayobject.h:5:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarrayobject.h:12:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarraytypes.h:1929:
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:17:2: warning: "Using deprecated NumPy API, disable it with "          "#define NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings]
  #warning "Using deprecated NumPy API, disable it with " \
   ^
  1 warning generated.
  INFO: clang -bundle -undefined dynamic_lookup -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/common_defs.o -L/Users/agriyakhetarpal/.local/lib -lsundials_nvecserial -lsundials_sunlinsolband -lsundials_sunlinsoldense -lsundials_sunlinsolpcg -lsundials_sunlinsolspbcgs -lsundials_sunlinsolspfgmr -lsundials_sunlinsolspgmr -lsundials_sunlinsolsptfqmr -lsundials_sunmatrixband -lsundials_sunmatrixdense -lsundials_sunmatrixsparse -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/common_defs.cpython-311-darwin.so
  INFO: building 'scikits.odes.sundials.cvode' extension
  INFO: compiling C sources
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: scikits/odes/sundials/cvode.c
  In file included from scikits/odes/sundials/cvode.c:819:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/arrayobject.h:5:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarrayobject.h:12:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarraytypes.h:1929:
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:17:2: warning: "Using deprecated NumPy API, disable it with "          "#define NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings]
  #warning "Using deprecated NumPy API, disable it with " \
   ^
  scikits/odes/sundials/cvode.c:52703:21: warning: fallthrough annotation in unreachable code [-Wunreachable-code-fallthrough]
                      CYTHON_FALLTHROUGH;
                      ^
  scikits/odes/sundials/cvode.c:412:34: note: expanded from macro 'CYTHON_FALLTHROUGH'
        #define CYTHON_FALLTHROUGH __attribute__((fallthrough))
                                   ^
  scikits/odes/sundials/cvode.c:52714:21: warning: fallthrough annotation in unreachable code [-Wunreachable-code-fallthrough]
                      CYTHON_FALLTHROUGH;
                      ^
  scikits/odes/sundials/cvode.c:412:34: note: expanded from macro 'CYTHON_FALLTHROUGH'
        #define CYTHON_FALLTHROUGH __attribute__((fallthrough))
                                   ^
  3 warnings generated.
  INFO: clang -bundle -undefined dynamic_lookup -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/cvode.o -L/Users/agriyakhetarpal/.local/lib -lsundials_cvode -lsundials_nvecserial -lsundials_sunlinsolband -lsundials_sunlinsoldense -lsundials_sunlinsolpcg -lsundials_sunlinsolspbcgs -lsundials_sunlinsolspfgmr -lsundials_sunlinsolspgmr -lsundials_sunlinsolsptfqmr -lsundials_sunmatrixband -lsundials_sunmatrixdense -lsundials_sunmatrixsparse -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/cvode.cpython-311-darwin.so
  INFO: building 'scikits.odes.sundials.ida' extension
  INFO: compiling C sources
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: scikits/odes/sundials/ida.c
  In file included from scikits/odes/sundials/ida.c:817:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/arrayobject.h:5:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarrayobject.h:12:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarraytypes.h:1929:
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:17:2: warning: "Using deprecated NumPy API, disable it with "          "#define NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings]
  #warning "Using deprecated NumPy API, disable it with " \
   ^
  1 warning generated.
  INFO: clang -bundle -undefined dynamic_lookup -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/ida.o -L/Users/agriyakhetarpal/.local/lib -lsundials_ida -lsundials_nvecserial -lsundials_sunlinsolband -lsundials_sunlinsoldense -lsundials_sunlinsolpcg -lsundials_sunlinsolspbcgs -lsundials_sunlinsolspfgmr -lsundials_sunlinsolspgmr -lsundials_sunlinsolsptfqmr -lsundials_sunmatrixband -lsundials_sunmatrixdense -lsundials_sunmatrixsparse -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/ida.cpython-311-darwin.so
  INFO: building 'scikits.odes.sundials.cvodes' extension
  INFO: compiling C sources
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: scikits/odes/sundials/cvodes.c
  In file included from scikits/odes/sundials/cvodes.c:819:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/arrayobject.h:5:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarrayobject.h:12:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarraytypes.h:1929:
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:17:2: warning: "Using deprecated NumPy API, disable it with "          "#define NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings]
  #warning "Using deprecated NumPy API, disable it with " \
   ^
  1 warning generated.
  INFO: clang -bundle -undefined dynamic_lookup -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/cvodes.o -L/Users/agriyakhetarpal/.local/lib -lsundials_cvodes -lsundials_nvecserial -lsundials_sunlinsolband -lsundials_sunlinsoldense -lsundials_sunlinsolpcg -lsundials_sunlinsolspbcgs -lsundials_sunlinsolspfgmr -lsundials_sunlinsolspgmr -lsundials_sunlinsolsptfqmr -lsundials_sunmatrixband -lsundials_sunmatrixdense -lsundials_sunmatrixsparse -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/cvodes.cpython-311-darwin.so
  INFO: building 'scikits.odes.sundials.idas' extension
  INFO: compiling C sources
  INFO: C compiler: clang -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk

  INFO: compile options: '-I/Users/agriyakhetarpal/.local/include -I/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include -Ibuild/src.macosx-14.0-arm64-3.11/numpy/distutils/include -I/Users/agriyakhetarpal/Desktop/PyBaMM/venv/include -I/opt/homebrew/Cellar/python@3.11/3.11.8/Frameworks/Python.framework/Versions/3.11/include/python3.11 -c'
  INFO: clang: scikits/odes/sundials/idas.c
  In file included from scikits/odes/sundials/idas.c:817:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/arrayobject.h:5:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarrayobject.h:12:
  In file included from /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/ndarraytypes.h:1929:
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:17:2: warning: "Using deprecated NumPy API, disable it with "          "#define NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings]
  #warning "Using deprecated NumPy API, disable it with " \
   ^
  1 warning generated.
  INFO: clang -bundle -undefined dynamic_lookup -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX14.sdk build/temp.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/idas.o -L/Users/agriyakhetarpal/.local/lib -lsundials_idas -lsundials_nvecserial -lsundials_sunlinsolband -lsundials_sunlinsoldense -lsundials_sunlinsolpcg -lsundials_sunlinsolspbcgs -lsundials_sunlinsolspfgmr -lsundials_sunlinsolspgmr -lsundials_sunlinsolsptfqmr -lsundials_sunmatrixband -lsundials_sunmatrixdense -lsundials_sunmatrixsparse -o build/lib.macosx-14.0-arm64-cpython-311/scikits/odes/sundials/idas.cpython-311-darwin.so
  installing to build/bdist.macosx-14.0-arm64/wheel
  running install
  running install_lib
  Skipping installation of build/bdist.macosx-14.0-arm64/wheel/scikits/__init__.py (namespace package)
  copying scikits/odes/ddaspkint.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/odeint.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/__init__.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/lsodi.cpython-311-darwin.so -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/dae.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/ddaspk.cpython-311-darwin.so -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/ode.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/dopri5.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/info.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/lsodiint.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes
  copying scikits/odes/tests/test_get_info.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/test_user_return_vals_ida.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/test_on_funcs_ida.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/test_user_return_vals_cvode.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/__init__.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/test_dop.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/test_odeint.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/test_on_funcs.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/tests/test_dae.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/tests
  copying scikits/odes/sundials/idas.cpython-311-darwin.so -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/cvodes.cpython-311-darwin.so -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/ida.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/common_defs.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_cvode.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_ida.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/__init__.py -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/cvode.cpython-311-darwin.so -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/common_defs.cpython-311-darwin.so -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_sundials.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_nvector_serial.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/ida.cpython-311-darwin.so -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/idas.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/cvode.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_sunmatrix.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/cvodes.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_sunlinsol.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_cvodes.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_sunnonlinsol.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  copying scikits/odes/sundials/c_idas.pxd -> build/bdist.macosx-14.0-arm64/wheel/scikits/odes/sundials
  running install_egg_info
  running egg_info
  writing scikits.odes.egg-info/PKG-INFO
  writing dependency_links to scikits.odes.egg-info/dependency_links.txt
  writing namespace_packages to scikits.odes.egg-info/namespace_packages.txt
  writing requirements to scikits.odes.egg-info/requires.txt
  writing top-level names to scikits.odes.egg-info/top_level.txt
  /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-build-env-t0_qydkd/overlay/lib/python3.11/site-packages/setuptools/command/egg_info.py:643: SetuptoolsDeprecationWarning: Custom 'build_py' does not implement 'get_data_files_without_manifest'.
  Please extend command classes from setuptools instead of distutils.
    warnings.warn(
  reading manifest file 'scikits.odes.egg-info/SOURCES.txt'
  reading manifest template 'MANIFEST.in'
  no previously-included directories found matching 'ci_support'
  no previously-included directories found matching 'docs'
  no previously-included directories found matching 'apidocs'
  no previously-included directories found matching 'paper'
  no previously-included directories found matching 'ipython_examples'
  warning: no previously-included files found matching '*.enc'
  warning: no previously-included files found matching 'upload_api_docs.sh'
  adding license file 'LICENSE.txt'
  writing manifest file 'scikits.odes.egg-info/SOURCES.txt'
  Copying scikits.odes.egg-info to build/bdist.macosx-14.0-arm64/wheel/scikits.odes-2.7.0-py3.11.egg-info
  Installing build/bdist.macosx-14.0-arm64/wheel/scikits.odes-2.7.0-py3.11-nspkg.pth
  running install_scripts
  running install_clib
  INFO: customize UnixCCompiler
  creating build/bdist.macosx-14.0-arm64/wheel/scikits.odes-2.7.0.dist-info/WHEEL
  creating '/private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-wheel-fioavx46/tmp34ngaxd2/scikits.odes-2.7.0-cp311-cp311-macosx_14_0_arm64.whl' and adding 'build/bdist.macosx-14.0-arm64/wheel' to it
  adding 'scikits.odes-2.7.0-py3.11-nspkg.pth'
  adding 'scikits/odes/__init__.py'
  adding 'scikits/odes/dae.py'
  adding 'scikits/odes/ddaspk.cpython-311-darwin.so'
  adding 'scikits/odes/ddaspkint.py'
  adding 'scikits/odes/dopri5.py'
  adding 'scikits/odes/info.py'
  adding 'scikits/odes/lsodi.cpython-311-darwin.so'
  adding 'scikits/odes/lsodiint.py'
  adding 'scikits/odes/ode.py'
  adding 'scikits/odes/odeint.py'
  adding 'scikits/odes/sundials/__init__.py'
  adding 'scikits/odes/sundials/c_cvode.pxd'
  adding 'scikits/odes/sundials/c_cvodes.pxd'
  adding 'scikits/odes/sundials/c_ida.pxd'
  adding 'scikits/odes/sundials/c_idas.pxd'
  adding 'scikits/odes/sundials/c_nvector_serial.pxd'
  adding 'scikits/odes/sundials/c_sundials.pxd'
  adding 'scikits/odes/sundials/c_sunlinsol.pxd'
  adding 'scikits/odes/sundials/c_sunmatrix.pxd'
  adding 'scikits/odes/sundials/c_sunnonlinsol.pxd'
  adding 'scikits/odes/sundials/common_defs.cpython-311-darwin.so'
  adding 'scikits/odes/sundials/common_defs.pxd'
  adding 'scikits/odes/sundials/cvode.cpython-311-darwin.so'
  adding 'scikits/odes/sundials/cvode.pxd'
  adding 'scikits/odes/sundials/cvodes.cpython-311-darwin.so'
  adding 'scikits/odes/sundials/cvodes.pxd'
  adding 'scikits/odes/sundials/ida.cpython-311-darwin.so'
  adding 'scikits/odes/sundials/ida.pxd'
  adding 'scikits/odes/sundials/idas.cpython-311-darwin.so'
  adding 'scikits/odes/sundials/idas.pxd'
  adding 'scikits/odes/tests/__init__.py'
  adding 'scikits/odes/tests/test_dae.py'
  adding 'scikits/odes/tests/test_dop.py'
  adding 'scikits/odes/tests/test_get_info.py'
  adding 'scikits/odes/tests/test_odeint.py'
  adding 'scikits/odes/tests/test_on_funcs.py'
  adding 'scikits/odes/tests/test_on_funcs_ida.py'
  adding 'scikits/odes/tests/test_user_return_vals_cvode.py'
  adding 'scikits/odes/tests/test_user_return_vals_ida.py'
  adding 'scikits.odes-2.7.0.dist-info/LICENSE.txt'
  adding 'scikits.odes-2.7.0.dist-info/METADATA'
  adding 'scikits.odes-2.7.0.dist-info/WHEEL'
  adding 'scikits.odes-2.7.0.dist-info/namespace_packages.txt'
  adding 'scikits.odes-2.7.0.dist-info/top_level.txt'
  adding 'scikits.odes-2.7.0.dist-info/RECORD'
  removing build/bdist.macosx-14.0-arm64/wheel
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
  INFO: CCompilerOpt.cache_flush[864] : write cache to path -> /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-install-0lgmxnh1/scikits-odes_42000b7077564b98b4ecaee0e5c51ab3/build/temp.macosx-14.0-arm64-cpython-311/ccompiler_opt_cache_ext.py
  Building wheel for scikits.odes (pyproject.toml) ... done
  Created wheel for scikits.odes: filename=scikits.odes-2.7.0-cp311-cp311-macosx_14_0_arm64.whl size=853624 sha256=b1af4d058efe98896f9020aa63bc2f34ba2d81ffb2aaa216d030ea42f2b4153a
  Stored in directory: /private/var/folders/b3/2bq1m1_50bs4c7305j8vxcqr0000gn/T/pip-ephem-wheel-cache-xk6170b4/wheels/41/7b/df/a45628ef0daaa6d2c60461a092198e27eaa4f72d995ed524a4
Successfully built scikits.odes
Installing collected packages: numpy, scipy, scikits.odes
  changing mode of /Users/agriyakhetarpal/Desktop/PyBaMM/venv/bin/f2py to 755
Successfully installed numpy-1.26.4 scikits.odes-2.7.0 scipy-1.12.0
```

</details>

---

_Comment by @agriyakhetarpal on 2024-03-25 11:37_

I posted the logs above in separate comments since they were too long to include in one comment, being greater than 65536 characters – first time I broke that barrier ;)

---

_Renamed from "`uv` fails to install `scikits.odes` on Python 3.11, works on Python 3.10 and earlier (cannot find `longintrepr.h` or possible `Cython` dependency resolution mismatch?)" to "`uv` fails to install `scikits.odes` on Python 3.11 due to Cython dependency resolution mismatch, works on Python 3.10 and earlier (cannot find `longintrepr.h`?)" by @agriyakhetarpal on 2024-03-25 11:38_

---

_Comment by @agriyakhetarpal on 2024-03-25 12:16_

Scourging through the incessantly long logs: `pip` chooses to use  `Cython-0.29.37` based on the `Cython<3.0.0a8` requirement, but `uv` interprets `cython<3.0.0a8` as `Selecting: cython==3.0a7 (Cython-3.0a7-py2.py3-none-any.whl)` – which I think is the bug, traced down to patient zero.

Based on https://pypi.org/project/Cython/#history we can say that `pip` prefers to download a non-alpha release but `uv` still prefers alpha releases. I suppose feature parity with `pip` would be the recommended solution to the bug, so maybe the resolver should match that (even though I think `uv` has the more correct idea here in terms of the transaction, strictly speaking).


---

_Comment by @charliermarsh on 2024-03-25 14:24_

Thanks for looking into this! I honestly think our behavior here is okay. Could you add a constraint on your side to pin `cython` to `<3.0.0`?

---

_Comment by @agriyakhetarpal on 2024-03-25 14:55_

> Thanks for looking into this! I honestly think our behavior here is okay.

Yes, I do too! Maybe this is a bug with `pip`, rather? I see that after `3.0a7` they disrupted their versioning scheme to publish `3.0.0a8`, i.e., the extra `0` is what is causing the trouble. Let me see how `packaging` parses this version and come back here. If `uv` follows PEP 440, it should be possible to correct this on the `pip` side of things.

> Could you add a constraint on your side to pin `cython` to `<3.0.0`?

I actually don't maintain `scikits.odes`, do you mean adding the constraint downstream instead? I can request them to do it too, but this dependency is unmaintained plus inactive at this time and I'm not sure it would be worth the effort. Also, wouldn't upper-pinning `cython` cause issues for us in the longer term as a library?

---

_Comment by @charliermarsh on 2024-03-25 14:56_

I think you could add a constraints file:

```txt
cython<3
```

And then pass it when you install, like: `uv pip install -r requirements.txt -c constraints.txt`


---

_Comment by @agriyakhetarpal on 2024-03-25 17:18_

> [!TIP]
> Scroll below to read the TL;DR ;)

Okay, I spent a bit of time digging and here is what the [PEP-440](https://packaging.python.org/en/latest/specifications/version-specifiers)-compliant `packaging` dependency does:

```python
from packaging.version import parse
x = parse("3.0.0a8")
y = parse("3.0a7")

print(y < x)
```

returns `True`, which I think makes sense here, and, to me, `pip`'s behaviour seems to be the correct one – this is because `3.0a7` means that it is an alpha-release for `3.0`[^1]. But, with `3.0.0a8` (i.e., the extra `0`) it is implied that there has been a release after `3.0a7`, and `a8` is a pre-release marker for version `3.0.0` which came later on (also, `packaging` doesn't know anything about the versions between two releases, it just compares these two nodes).

Here is how one can verify it:

```python
from packaging.version import parse
a = parse("3.0a7")
b = parse("3.0.0")
print(a < b)
```

returns `True`.

It was a bit confusing to get this at first, but I think maybe the Cython devs realised this mishap in the middle of getting to a stable release for `3.0.0` and tried to correct it between the release cycle (which produced this outlier) – so honestly they are the ones that seem to have brought the trouble with a confusing edge case.

A better explainer for this would be to imagine this situation, instead:

```python
from packaging.version import parse
c = parse("6.0.0a7")
d = parse("6.0a7")
print(c == d)
```

actually returns `True`, and this normalisation of the parsing is explained in the [Pre-release separators](https://packaging.python.org/en/latest/specifications/version-specifiers/#pre-release-separators) section.

## To summarise

Because `3.0.0a8` _equals_ `3.0a8`, `3.0a7` therefore _must_ be less than `3.0a8`; `pip` ignores `3.0a7`, `3.0a6`, `3.0a5`, and so on to `3.0a1` (all of the PyPI pre-releases) and then finally chooses to download `0.29.37`, because the `--pre` flag was not specified. But, `uv` chooses to download `3.0a7` because `cython<3.0.0a8` is specified, but it's downloading a pre-release, which I have not asked it to do. To put this in commands,

### `pip`'s behaviour

```bash
pip install "cython<3.0.0a8" # or pip install "cython<3.0a8", both mean the same thing as explained above
```

installs `cython-0.29.37` because I did not request for a pre-release

but

```bash
pip install --pre "cython<3.0.0a8"
```

installs `cython-3.0a7` because I _did_ request for a pre-release.

### `uv`'s behaviour

```bash
uv pip install "cython<3.0.0a8"
```

gets me `cython-3.0a7` rather than a stable release, which is the cause of the bug at hand.

[^1]: they used an implicit pre-release number, but that didn't translate to the next release they did after that: https://packaging.python.org/en/latest/specifications/version-specifiers/#implicit-pre-release-number



---

_Comment by @notatallshaw on 2024-03-25 17:19_

Pip doesn't include prereleases when using exclusive ordered comparison, e.g. `<`.

It does include prereleases when using inclusive ordered comparison, e.g. `<=`.

I beleive pip is following the spec here and uv is probably wrong, but the spec is open to some interpretation. I wrote about this issue here: https://github.com/astral-sh/packse/issues/161, but hadn't got round to making a specific uv issue yet.

---

_Comment by @charliermarsh on 2024-03-25 17:23_

I'm a little confused -- `uv pip install "cython<3.0.0a8"` returning `cython-3.0a7` is right, per the spec, isn't it?

> The exclusive ordered comparison <V MUST NOT allow a pre-release of the specified version unless the specified version is itself a pre-release.


---

_Comment by @agriyakhetarpal on 2024-03-25 17:23_

Yeah, the semantics of the specification are always at play, and the difference between `<=` and `<` in comparisons is also on the part of package maintainers to do correctly

---

_Comment by @notatallshaw on 2024-03-25 17:26_

> I'm a little confused -- `uv pip install "cython<3.0.0a8"` returning `cython-3.0a7` is right, per the spec, isn't it?

Oh you're right, I misread things (I probably shouldn't try and investigate issues from my phone), this is one of the many areas pip isn't following the prerelease spec, I'll check later if there's an open issue or not.

---

_Comment by @charliermarsh on 2024-03-25 17:28_

The spec is ambiguous though. Arguably they should still be excluded based on this, if you don't consider `cython<3.0.0a8` to be "explicitly requested by the user".

> Pre-releases of any kind, including developmental releases, are implicitly excluded from all version specifiers, unless they are already present on the system, explicitly requested by the user, or if the only available version that satisfies the version specifier is a pre-release.

But, IDK, I do consider this "explicitly requested by the user".


---

_Comment by @notatallshaw on 2024-03-25 17:30_

Yes, I would consider a prerelease marker in the version to mean explicitly requested by the user, this is also generally what pypa tools and other Python packaging tools also take it to mean.

---

_Comment by @agriyakhetarpal on 2024-03-25 17:34_

> Arguably they should still be excluded based on this, if you don't consider `cython<3.0.0a8` to be "explicitly requested by the user".

Okay I'm starting to get (a bit) lost here – when asking the `cython<3.0.0a8` question, doesn't this imply that the user does _not_ want `3.0.0a8`? And therefore, it's not explicitly "requested" per se, but more like explicitly "excluded" to be downloaded/installed? If I were to ask for `cython<=3.0.0a8`, I'm surely going to get `3.0.0a8`, as expected.

Might be worth looking at: https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/22

---

_Comment by @notatallshaw on 2024-03-25 18:29_

> Might be worth looking at: https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/22

Being the person who opened that thread, I strongly came away with the opinion of "the spec is ambiguous" in a lot of these prerelease cases 😔, and pip has not reviewed if it is closely following it where it's not ambiguous (there's several open issues on pip side, both before this thread and after it that I've created).

---

_Comment by @charliermarsh on 2024-03-25 19:25_

> Okay I'm starting to get (a bit) lost here – when asking the cython<3.0.0a8 question, doesn't this imply that the user does not want 3.0.0a8? And therefore, it's not explicitly "requested" per se, but more like explicitly "excluded" to be downloaded/installed? If I were to ask for cython<=3.0.0a8, I'm surely going to get 3.0.0a8, as expected.

Yes, `3.0.0a8` should _absolutely_ be excluded given `cython<3.0.0a8`. What's being discussed here is whether `3.0.0a7` should be included, given `cython<3.0.0a8`. The PEP says: yes, it should be included in the specifier.

---

_Comment by @charliermarsh on 2024-03-25 19:25_

I'm going to close as "working as intended".

---

_Closed by @charliermarsh on 2024-03-25 19:25_

---

_Comment by @agriyakhetarpal on 2024-03-25 19:57_

Thanks for the speedy resolution here, @charliermarsh and @notatallshaw! I guess this is a bug to file with `pip`, then – if it doesn't exist there already, of course.

---

_Comment by @notatallshaw on 2024-03-25 23:14_

Sorry for doing a second u-turn on this conversation, I really should have waited until I had time to carefully read through everything before posting, I will try and learn from this.

But I remember why I linked the packse issue now, it linked to an important comment that was in a conversation I was involved in on this: https://github.com/pypa/packaging/issues/776#issuecomment-1900515985

In particular from @dstufft  

> I think this makes general sense, !=1.0a1 is explicitly saying you don't want that version, so it not requesting prereleases, while ==1.0a1 is explicitly saying you do want that version, so it is requesting prereleases.
> 
> The > and < operators kind of sit in a weird place though, >1.0a1 is not requesting 1.0a1, it's requesting anything of a higher version than that, so it currently does not trigger the "user has requested prereleases" logic, but that creates a weird asymmetry with >=1.0a1. To make matters worse, It's not possible to get a prerelease version that is less than 1.0a0.dev0, so does <1.0a0.dev0 include or exclude prereleases?

So it is intentional that packaging does *not* include `3.0.0a7` here:

```python
>>> from packaging.specifiers import SpecifierSet
>>> SpecifierSet('<3.0.0a8').contains("3.0.0a7")
False
>>> SpecifierSet('<=3.0.0a8').contains("3.0.0a7")
True
```

And this is where pip inherits it's behavior from, therefore at least according to the person who wrote the spec, uv would not be following it.

---

_Referenced in [pypa/packaging#788](../../pypa/packaging/issues/788.md) on 2024-03-26 22:35_

---

_Comment by @notatallshaw on 2024-03-26 22:35_

I've created an issue on packaging side, let's see if there's any feedback there: https://github.com/pypa/packaging/issues/788

---

_Comment by @charliermarsh on 2024-03-26 22:35_

Thanks @notatallshaw!

---

_Comment by @pradyunsg on 2024-04-07 21:54_

This is a bug in packaging.

---

_Comment by @notatallshaw on 2024-04-13 17:03_

FWIW I've made a PR: https://github.com/pypa/packaging/pull/794

---

_Referenced in [astral-sh/packse#172](../../astral-sh/packse/pulls/172.md) on 2024-04-14 17:28_

---

_Comment by @notatallshaw on 2025-01-26 15:42_

Pip should now match this behavior: https://github.com/pypa/pip/blob/main/NEWS.rst#bug-fixes

> - The inclusion of ``packaging`` 24.2 changes how pre-release specifiers with ``<`` and ``>`` behave. Including a pre-release version with these specifiers now implies accepting pre-releases (e.g., ``<2.0dev`` can include ``1.0rc1``). To avoid implying pre-releases, avoid specifying them (e.g., use ``<2.0``).  The exception is ``!=``, which never implies pre-releases. (https://github.com/pypa/pip/issues/13163)

Pre-releases are still hard though, and there will be many edge cases that uv and pip don't match, in some cases bugs, in some cases design choices. I'm hoping a PEP overhauling the language of pre-releases this year (see some recent discussion on the topic: https://discuss.python.org/t/proposal-intersect-and-disjoint-operations-for-python-version-specifiers/71888).

---

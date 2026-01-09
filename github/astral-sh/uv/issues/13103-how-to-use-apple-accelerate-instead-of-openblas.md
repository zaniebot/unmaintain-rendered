---
number: 13103
title: How to use Apple Accelerate instead of OpenBlas when installing Numpy on Apple Silicon?
type: issue
state: closed
author: Eureka-0
labels:
  - question
assignees: []
created_at: 2025-04-25T08:42:26Z
updated_at: 2025-04-25T12:49:51Z
url: https://github.com/astral-sh/uv/issues/13103
synced_at: 2026-01-07T13:12:18-06:00
---

# How to use Apple Accelerate instead of OpenBlas when installing Numpy on Apple Silicon?

---

_Issue opened by @Eureka-0 on 2025-04-25 08:42_

### Question

Hi, in the past year, I have used `no-binary-package = ["numpy"]` config for installing numpy (just build it on my laptop), and it works well with Accelerate. This is the output of `numpy.__config__.show()`:

```
Build Dependencies:
  blas:
    detection method: system
    found: true
    include directory: unknown
    lib directory: unknown
    name: accelerate
    openblas configuration: unknown
    pc file directory: unknown
    version: unknown
  lapack:
    detection method: internal
    found: true
    include directory: unknown
    lib directory: unknown
    name: dep4396855664
    openblas configuration: unknown
    pc file directory: unknown
    version: 1.26.4
```

While if I install pre-built wheel using `uv add numpy` (remove `no-binary-package = ["numpy"]` config), it will use OpenBlas instead of Accelerate:

```
Build Dependencies:
  blas:
    detection method: pkgconfig
    found: true
    include directory: /opt/arm64-builds/include
    lib directory: /opt/arm64-builds/lib
    name: openblas64
    openblas configuration: USE_64BITINT=1 DYNAMIC_ARCH=1 DYNAMIC_OLDER= NO_CBLAS=
      NO_LAPACK= NO_LAPACKE= NO_AFFINITY=1 USE_OPENMP= SANDYBRIDGE MAX_THREADS=3
    pc file directory: /usr/local/lib/pkgconfig
    version: 0.3.23.dev
  lapack:
    detection method: pkgconfig
    found: true
    include directory: /opt/arm64-builds/include
    lib directory: /opt/arm64-builds/lib
    name: openblas64
    openblas configuration: USE_64BITINT=1 DYNAMIC_ARCH=1 DYNAMIC_OLDER= NO_CBLAS=
      NO_LAPACK= NO_LAPACKE= NO_AFFINITY=1 USE_OPENMP= SANDYBRIDGE MAX_THREADS=3
    pc file directory: /usr/local/lib/pkgconfig
    version: 0.3.23.dev
```

OpenBlas is much slower and has heavier CPU usage than Accelerate (there are many benchmarks on the Internet). So I prefer to build numpy locally to use Accelerate. However, I encountered a problem when installing a package `ChaosMagPy ` (which relies on Numpy 1.26.0). The error are as follows.

```
Found CMake: /opt/homebrew/bin/cmake (4.0.0)
WARNING: CMake Toolchain: Failed to determine CMake compilers state
Run-time dependency openblas found: NO (tried pkgconfig, framework and cmake)
Run-time dependency openblas found: NO (tried pkgconfig, framework and cmake)

../../numpy/meson.build:207:4: ERROR: Problem encountered: No BLAS library detected! Install one, or use the `allow-noblas` build
option (note, this may be up to 100x slower for some linear algebra operations).
```

It seems that OpenBlas can not be found and Accelerate was not used, but [Numpy 1.26.0 should support Accelerate](https://numpy.org/doc/stable/release/1.26.0-notes.html#support-for-the-updated-accelerate-blas-lapack-library). So where is the problem here? Does uv has pre-built Numpy wheels for Accelerate and how to install it properly?

### Platform

macOS 15 arm64

### Version

uv 0.6.16 (Homebrew 2025-04-22)

---

_Label `question` added by @Eureka-0 on 2025-04-25 08:42_

---

_Comment by @konstin on 2025-04-25 10:04_

This is problem with numpy rather than uv and should be discussed with the numpy developers rather than uv.

---

_Comment by @Eureka-0 on 2025-04-25 11:53_

> This is problem with numpy rather than uv and should be discussed with the numpy developers rather than uv.

Thank you for reply. Finally I figured out. Accelerate was supported from Numpy 1.26.0, but the auto-detection for Accelerate was added from [1.26.1](https://numpy.org/devdocs/release/1.26.1-notes.html#improved-blas-lapack-detection-and-control). So I have to specify the blas to use when building. This command gave me Numpy 1.26.0 with Accelerate:

```
uv add numpy==1.26.0 --config-setting setup-args=-Dblas=accelerate --config-setting setup-args=-Dlapack=accelerate --no-binary-package numpy
```

---

_Closed by @konstin on 2025-04-25 12:49_

---

```yaml
number: 15693
title: Cannot install polars-lts-cpu on a machine with a Snapdragon X Plus (ARM) processor
type: issue
state: open
author: laurafroelich
labels:
  - bug
assignees: []
created_at: 2025-09-05T06:58:05Z
updated_at: 2025-09-10T16:18:38Z
url: https://github.com/astral-sh/uv/issues/15693
synced_at: 2026-01-12T16:02:15Z
```

# Cannot install polars-lts-cpu on a machine with a Snapdragon X Plus (ARM) processor

---

_@laurafroelich_

### Summary

Hi,

A quick summary of my issue is that `uv sync` fails when trying to install a project including polars-lts-cpu as a dependency, whereas it installs and works as expected with pip.

A bit of context: `polars-lts-cpu` is a version of the `polars` package, but for CPUs that don't provide standard features such as avx, avx2, fma, bmi1, bmi2, lzcnt, and movbe. The `polars` (or `polars-lts-cpu`) package is Rust based, enabling it to dramatically speed up certain typical Pandas operations.

CPU: Snapdragon X Plus (ARM-based), exact name: Snapdragon(R) X Plus - X1P42100 - Qualcomm(R) Oryon(TM) CPU (3.24 GHz)
OS: Windows 11 Pro, 24H2
uv version: uv 0.8.12 (36151df0e 2025-08-18)
Python: Python 3.13.5
rustup: rustup 1.28.2 (e4f3ad6f8 2025-04-28)
rustc: rustc 1.89.0 (29483883e 2025-08-04)

A small pyproject.toml that shows the issue is:
```
[project]
name = "small-example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [    
    "polars; platform_machine=='x86_64' or platform_machine == 'AMD64'",
    "polars-lts-cpu; platform_machine=='ARM64'"]
```

With pip, in a new virtual environment, installation works:
```
> pip install --use-pep517 --no-cache --force-reinstall .
Processing c:\users\<username>\documents\projects\small-example
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting polars-lts-cpu (from small-example==0.1.0)
  Downloading polars_lts_cpu-1.33.0-cp39-abi3-win_amd64.whl.metadata (15 kB)
Downloading polars_lts_cpu-1.33.0-cp39-abi3-win_amd64.whl (38.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 38.6/38.6 MB 11.4 MB/s eta 0:00:00
Building wheels for collected packages: small-example
  Building wheel for small-example (pyproject.toml) ... done
  Created wheel for small-example: filename=small_example-0.1.0-py3-none-any.whl size=1340 sha256=b5339ff1ee3af7402825261f93cafdb1a08f6c738140cabba9fe002c2e89c104
  Stored in directory: C:\Users\<UserName>\AppData\Local\Temp\pip-ephem-wheel-cache-xg2ys308\wheels\97\f9\7d\8ba8038327542c6b67e8de16bce440409717a90f44d305b20b
Successfully built small-example
Installing collected packages: polars-lts-cpu, small-example
Successfully installed polars-lts-cpu-1.33.0 small-example-0.1.0

[notice] A new release of pip is available: 25.1.1 -> 25.2
[notice] To update, run: python.exe -m pip install --upgrade pip
```

However, with uv, the build process fails:
```
> uv sync
Resolved 3 packages in 111ms
  x Failed to build `polars-lts-cpu==1.33.0`
  |-> The build backend returned an error
  `-> Call to `maturin.build_wheel` failed (exit code: 1)

      [stdout]
      Running `maturin pep517 build-wheel -i
      C:\Users\<UserName>\AppData\Local\uv\cache\builds-v0\.tmptbQtP3\Scripts\python.exe --compatibility off`

      [stderr]
      🍹 Building a mixed python/rust project
      🔗 Found pyo3 bindings with abi3 support
         Compiling either v1.15.0
         Compiling bytes v1.10.1
         Compiling pyo3-build-config v0.25.1
         Compiling yoke v0.8.0
         Compiling ring v0.17.14
         Compiling regex v1.11.1
         Compiling typenum v1.18.0
         Compiling tracing-attributes v0.1.30
      warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
      warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
      error: failed to run custom build command for `ring v0.17.14`

      Caused by:
        process didn't exit successfully:
      `C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-ed18bd76a4a7bb4e\build-script-build`
      (exit code: 1)
        --- stdout
        cargo:rerun-if-env-changed=CARGO_MANIFEST_DIR
        cargo:rerun-if-env-changed=CARGO_PKG_NAME
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MAJOR
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MINOR
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PATCH
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PRE
        cargo:rerun-if-env-changed=CARGO_MANIFEST_LINKS
        cargo:rerun-if-env-changed=RING_PREGENERATE_ASM
        cargo:rerun-if-env-changed=OUT_DIR
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ARCH
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_OS
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENV
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENDIAN
        OPT_LEVEL = Some(3)
        OUT_DIR =
      Some(C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-c536cf4a7f3e1597\out)
        TARGET = Some(aarch64-pc-windows-msvc)
        cargo:rerun-if-env-changed=VCINSTALLDIR
        VCINSTALLDIR = None
        cargo:rerun-if-env-changed=VSTEL_MSBuildProjectFullPath
        VSTEL_MSBuildProjectFullPath = None
        cargo:rerun-if-env-changed=VSCMD_ARG_VCVARS_SPECTRE
        VSCMD_ARG_VCVARS_SPECTRE = None
        cargo:rerun-if-env-changed=WindowsSdkDir
        WindowsSdkDir = None
        cargo:rerun-if-env-changed=WindowsSDKVersion
        WindowsSDKVersion = None
        cargo:rerun-if-env-changed=LIB
        LIB = None
        PATH =
      Some(C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\deps;C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release;C:\Users\<UserName>\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\lib\rustlib\aarch64-pc-windows-msvc\lib;C:\Users\<UserName>\AppData\Local\uv\cache\builds-v0\.tmptbQtP3\Scripts;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program
      Files\Git\cmd;C:\Program Files (x86)\Windows Kits\10\Windows Performance Toolkit\;C:\Program
      Files\dotnet\;C:\Users\<UserName>\.cargo\bin;C:\Users\<UserName>\.local\bin;C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Scripts\;C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\;C:\Users\<UserName>\AppData\Local\Programs\Python\Launcher\;C:\Users\<UserName>\AppData\Local\Microsoft\WindowsApps;C:\Users\<UserName>\AppData\Roaming\Python\Scripts;C:\Users\<UserName>\.dotnet\tools;C:\Program
      Files\Microsoft Visual
      Studio\2022\Community\VC\Tools\MSVC\14.44.35207\lib\x64;;C:\Users\<UserName>\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\bin)
        cargo:rerun-if-env-changed=INCLUDE
        INCLUDE = None
        HOST = Some(aarch64-pc-windows-msvc)
        cargo:rerun-if-env-changed=CC_aarch64-pc-windows-msvc
        CC_aarch64-pc-windows-msvc = None
        cargo:rerun-if-env-changed=CC_aarch64_pc_windows_msvc
        CC_aarch64_pc_windows_msvc = None
        cargo:rerun-if-env-changed=HOST_CC
        HOST_CC = None
        cargo:rerun-if-env-changed=CC
        CC = None
        cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
        CRATE_CC_NO_DEFAULTS = None
        CARGO_CFG_TARGET_FEATURE = Some(neon)
        DEBUG = Some(true)
        cargo:rerun-if-env-changed=CFLAGS
        CFLAGS = None
        cargo:rerun-if-env-changed=HOST_CFLAGS
        HOST_CFLAGS = None
        cargo:rerun-if-env-changed=CFLAGS_aarch64_pc_windows_msvc
        CFLAGS_aarch64_pc_windows_msvc = None
        cargo:rerun-if-env-changed=CFLAGS_aarch64-pc-windows-msvc
        CFLAGS_aarch64-pc-windows-msvc = None
        CARGO_ENCODED_RUSTFLAGS = Some()
        cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)

        --- stderr


        error occurred in cc-rs: failed to find tool "clang": program not found (see
      https://docs.rs/cc/latest/cc/#compile-time-requirements for help)


      warning: build failed, waiting for other jobs to finish...
      💥 maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit code: 101": `"cargo"
      "rustc" "--message-format" "json-render-diagnostics" "--manifest-path"
      "C:\\Users\\<UserName>\\AppData\\Local\\uv\\cache\\sdists-v9\\pypi\\polars-lts-cpu\\1.33.0\\hAXhuRtb3NI4cwW3tIBny\\src\\py-polars\\Cargo.toml"
      "--release" "--lib"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i',
      'C:\\Users\\<UserName>\\AppData\\Local\\uv\\cache\\builds-v0\\.tmptbQtP3\\Scripts\\python.exe',
      '--compatibility', 'off'] returned non-zero exit status 1

      hint: This usually indicates a problem with the package or the build environment.
  help: `polars-lts-cpu` (v1.33.0) was included because `small-example` (v0.1.0) depends on `polars-lts-cpu`
```

I have replaced my actual username with `<UserName>` and `<username>`, reflecting the upper/lower case in the output.

Before I got the idea to try with pip, I made many attempts to get the build to work. I installed Rust, Visual Studio with the  ".NET desktop development" and "Desktop development with C++" work loads ticked. Additionally, I also selected the individual components "C++ Clang Compiler for Windows (19.1.5)" and "MSBuild support for LLVM (clang-cl) toolset". I don't recall if I included other individual components manually.

Please let me know if there is any additional information that would be helpful.

Also, a big thank you for all the work on uv. Although I only started using uv recently, I already like it a lot.

### Platform

Windows 11 Pro, Snapdragon X Plus (ARM-based)

### Version

uv 0.8.12 (36151df0e 2025-08-18)

### Python version

Python 3.13.5

---

_Label `bug` added by @laurafroelich on 2025-09-05 06:58_

---

_Comment by @konstin on 2025-09-05 09:13_

In the pip case, pip is installing the `win_amd64` wheel for x86_64. Are you using the same Python interpreter for pip and for `uv sync`? If pip is using a transparently emulated x86_64 Python, and uv is using the a native arm64 Python, this would explain why pip can install the wheel, while uv tries to build from source.

`uv sync -v` can give you more insight in what uv is doing, and whether it's using emulated x86_64 or native arm64.

For both the compiler tools and for uv, are you using the x86_64 versions or the arm64 versions?

---

_Comment by @laurafroelich on 2025-09-05 12:00_

Thank you for the quick reply.

I ran `uv sync -v` and `pip --version` and it looks like they are using the same Python interpreter. In the output from `uv sync -v`, I see `Using CPython 3.13.5 interpreter at: C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\python.exe` and with pip I see:

```
PS C:\Users\<UserName>\Documents\projects\small-example> pip --version
pip 25.1.1 from C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\site-packages\pip (python 3.13)
```

```
PS C:\Users\<UserName>\Documents\projects\small-example> uv sync -v
DEBUG uv 0.8.15 (8473ecba1 2025-09-03)
DEBUG Found project root: `C:\Users\<UserName>\Documents\projects\small-example`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\Users\<UserName>\Documents\projects\small-example`
DEBUG Reading Python requests from version file at `C:\Users\<UserName>\Documents\projects\small-example\.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG Searching for Python 3.13 in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\<UserName>\AppData\Roaming\uv\python`
DEBUG Found `cpython-3.13.5-windows-x86_64-none` at `C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\python.exe` (first executable in the search path)
Using CPython 3.13.5 interpreter at: C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\python.exe
Creating virtual environment at: .venv
DEBUG Using base executable for virtual environment: C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\python.exe
DEBUG Released lock at `C:\Users\LAURAF~1\AppData\Local\Temp\uv-4d904aeae39cf5e9.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: small-example @ file:///C:/Users/LauraFr%C3%B8lich/Documents/projects/small-example
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 3 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: polars-lts-cpu==1.33.0
DEBUG Acquired lock for `C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/28/b8/3763cc3ea3fdfa39057ce105a477789f13c5278e47f2ce5fe507e8a4d2e4/polars_lts_cpu-1.33.0.tar.gz
   Building polars-lts-cpu==1.33.0
DEBUG Building: polars-lts-cpu==1.33.0
DEBUG Not using uv build backend direct build of polars-lts-cpu==1.33.0, no pyproject.toml: TOML parse error at line 5, column 1
  |
5 | [project]
  | ^^^^^^^^^
missing field `version`

DEBUG Reusing existing build environment for: polars-lts-cpu==1.33.0
DEBUG Using base executable for virtual environment: C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\python.exe
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13.5
DEBUG Adding direct dependency: maturin>=1.3.2
DEBUG Acquired lock for `C:\Users\<UserName>\AppData\Local\uv\cache\simple-v17\pypi\maturin.lock`
DEBUG Found fresh response for: https://pypi.org/simple/maturin/
DEBUG Released lock at `C:\Users\<UserName>\AppData\Local\uv\cache\simple-v17\pypi\maturin.lock`
DEBUG Searching for a compatible version of maturin (>=1.3.2)
DEBUG Selecting: maturin==1.9.4 [compatible] (maturin-1.9.4-py3-none-win_amd64.whl)
DEBUG Acquired lock for `C:\Users\<UserName>\AppData\Local\uv\cache\wheels-v5\pypi\maturin\maturin-1.9.4-py3-none-win_amd64.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/14/14/f86d0124bf1816b99005c058a1dbdca7cb5850d9cf4b09dcae07a1bc6201/maturin-1.9.4-py3-none-win_amd64.whl.metadata
DEBUG Released lock at `C:\Users\<UserName>\AppData\Local\uv\cache\wheels-v5\pypi\maturin\maturin-1.9.4-py3-none-win_amd64.lock`
DEBUG Tried 1 versions: maturin 1
DEBUG marker environment resolution took 0.004s
DEBUG Installing in maturin==1.9.4 in C:\Users\<UserName>\AppData\Local\uv\cache\builds-v0\.tmpw0rdR8
DEBUG Registry requirement already cached: maturin==1.9.4
DEBUG Installing build requirement: maturin==1.9.4
DEBUG Creating PEP 517 build environment
DEBUG Calling `maturin.get_requires_for_build_wheel()`
DEBUG Calling `maturin.build_wheel("C:\\Users\\<UserName>\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpCml6KL", {}, None)`
DEBUG Running `maturin pep517 build-wheel -i C:\Users\<UserName>\AppData\Local\uv\cache\builds-v0\.tmpw0rdR8\Scripts\python.exe --compatibility off`
DEBUG 🍹 Building a mixed python/rust project
DEBUG 🔗 Found pyo3 bindings with abi3 support
DEBUG    Compiling pyo3-build-config v0.25.1
DEBUG    Compiling ring v0.17.14
DEBUG    Compiling icu_provider v2.0.0
DEBUG    Compiling typenum v1.18.0
DEBUG    Compiling tracing v0.1.41
DEBUG    Compiling httparse v1.10.1
DEBUG    Compiling icu_normalizer_data v2.0.0
DEBUG    Compiling icu_properties_data v2.0.1
DEBUG warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
DEBUG warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
DEBUG error: failed to run custom build command for `ring v0.17.14`
DEBUG
DEBUG Caused by:
DEBUG   process didn't exit successfully: `C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-ed18bd76a4a7bb4e\build-script-build` (exit code: 1)
DEBUG   --- stdout
DEBUG   cargo:rerun-if-env-changed=CARGO_MANIFEST_DIR
DEBUG   cargo:rerun-if-env-changed=CARGO_PKG_NAME
DEBUG   cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MAJOR
DEBUG   cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MINOR
DEBUG   cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PATCH
DEBUG   cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PRE
DEBUG   cargo:rerun-if-env-changed=CARGO_MANIFEST_LINKS
DEBUG   cargo:rerun-if-env-changed=RING_PREGENERATE_ASM
DEBUG   cargo:rerun-if-env-changed=OUT_DIR
DEBUG   cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ARCH
DEBUG   cargo:rerun-if-env-changed=CARGO_CFG_TARGET_OS
DEBUG   cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENV
DEBUG   cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENDIAN
DEBUG   OPT_LEVEL = Some(3)
DEBUG   OUT_DIR = Some(C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-c536cf4a7f3e1597\out)
DEBUG   TARGET = Some(aarch64-pc-windows-msvc)
DEBUG   cargo:rerun-if-env-changed=VCINSTALLDIR
DEBUG   VCINSTALLDIR = None
DEBUG   cargo:rerun-if-env-changed=VSTEL_MSBuildProjectFullPath
DEBUG   VSTEL_MSBuildProjectFullPath = None
DEBUG   cargo:rerun-if-env-changed=VSCMD_ARG_VCVARS_SPECTRE
DEBUG   VSCMD_ARG_VCVARS_SPECTRE = None
DEBUG   cargo:rerun-if-env-changed=WindowsSdkDir
DEBUG   WindowsSdkDir = None
DEBUG   cargo:rerun-if-env-changed=WindowsSDKVersion
DEBUG   WindowsSDKVersion = None
DEBUG   cargo:rerun-if-env-changed=LIB
DEBUG   LIB = None
DEBUG   PATH = Some(C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\deps;C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release;C:\Users\<UserName>\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\lib\rustlib\aarch64-pc-windows-msvc\lib;C:\Users\<UserName>\AppData\Local\uv\cache\builds-v0\.tmpw0rdR8\Scripts;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program Files\Git\cmd;C:\Program Files (x86)\Windows Kits\10\Windows Performance Toolkit\;C:\Program Files\dotnet\;C:\Users\<UserName>\.cargo\bin;C:\Users\<UserName>\.local\bin;C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Scripts\;C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\;C:\Users\<UserName>\AppData\Local\Programs\Python\Launcher\;C:\Users\<UserName>\AppData\Local\Microsoft\WindowsApps;C:\Users\<UserName>\AppData\Roaming\Python\Scripts;C:\Users\<UserName>\.dotnet\tools;C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.44.35207\lib\x64;;C:\Users\<UserName>\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\bin)
DEBUG   cargo:rerun-if-env-changed=INCLUDE
DEBUG   INCLUDE = None
DEBUG   HOST = Some(aarch64-pc-windows-msvc)
DEBUG   cargo:rerun-if-env-changed=CC_aarch64-pc-windows-msvc
DEBUG   CC_aarch64-pc-windows-msvc = None
DEBUG   cargo:rerun-if-env-changed=CC_aarch64_pc_windows_msvc
DEBUG   CC_aarch64_pc_windows_msvc = None
DEBUG   cargo:rerun-if-env-changed=HOST_CC
DEBUG   HOST_CC = None
DEBUG   cargo:rerun-if-env-changed=CC
DEBUG   CC = None
DEBUG   cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
DEBUG   CRATE_CC_NO_DEFAULTS = None
DEBUG   CARGO_CFG_TARGET_FEATURE = Some(neon)
DEBUG   DEBUG = Some(true)
DEBUG   cargo:rerun-if-env-changed=CFLAGS
DEBUG   CFLAGS = None
DEBUG   cargo:rerun-if-env-changed=HOST_CFLAGS
DEBUG   HOST_CFLAGS = None
DEBUG   cargo:rerun-if-env-changed=CFLAGS_aarch64_pc_windows_msvc
DEBUG   CFLAGS_aarch64_pc_windows_msvc = None
DEBUG   cargo:rerun-if-env-changed=CFLAGS_aarch64-pc-windows-msvc
DEBUG   CFLAGS_aarch64-pc-windows-msvc = None
DEBUG   CARGO_ENCODED_RUSTFLAGS = Some()
DEBUG   cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
DEBUG   cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
DEBUG   cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
DEBUG
DEBUG   --- stderr
DEBUG
DEBUG
DEBUG   error occurred in cc-rs: failed to find tool "clang": program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
DEBUG
DEBUG
DEBUG warning: build failed, waiting for other jobs to finish...
DEBUG 💥 maturin failed
DEBUG   Caused by: Failed to build a native library through cargo
DEBUG   Caused by: Cargo build finished with "exit code: 101": `"cargo" "rustc" "--message-format" "json-render-diagnostics" "--manifest-path" "C:\\Users\\<UserName>\\AppData\\Local\\uv\\cache\\sdists-v9\\pypi\\polars-lts-cpu\\1.33.0\\hAXhuRtb3NI4cwW3tIBny\\src\\py-polars\\Cargo.toml" "--release" "--lib"`
DEBUG Error: command ['maturin', 'pep517', 'build-wheel', '-i', 'C:\\Users\\<UserName>\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpw0rdR8\\Scripts\\python.exe', '--compatibility', 'off'] returned non-zero exit status 1
DEBUG Released lock at `C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\.lock`
  x Failed to build `polars-lts-cpu==1.33.0`
  |-> The build backend returned an error
  `-> Call to `maturin.build_wheel` failed (exit code: 1)

      [stdout]
      Running `maturin pep517 build-wheel -i
      C:\Users\<UserName>\AppData\Local\uv\cache\builds-v0\.tmpw0rdR8\Scripts\python.exe --compatibility off`

      [stderr]
      🍹 Building a mixed python/rust project
      🔗 Found pyo3 bindings with abi3 support
         Compiling pyo3-build-config v0.25.1
         Compiling ring v0.17.14
         Compiling icu_provider v2.0.0
         Compiling typenum v1.18.0
         Compiling tracing v0.1.41
         Compiling httparse v1.10.1
         Compiling icu_normalizer_data v2.0.0
         Compiling icu_properties_data v2.0.1
      warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
      warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
      error: failed to run custom build command for `ring v0.17.14`

      Caused by:
        process didn't exit successfully:
      `C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-ed18bd76a4a7bb4e\build-script-build`
      (exit code: 1)
        --- stdout
        cargo:rerun-if-env-changed=CARGO_MANIFEST_DIR
        cargo:rerun-if-env-changed=CARGO_PKG_NAME
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MAJOR
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MINOR
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PATCH
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PRE
        cargo:rerun-if-env-changed=CARGO_MANIFEST_LINKS
        cargo:rerun-if-env-changed=RING_PREGENERATE_ASM
        cargo:rerun-if-env-changed=OUT_DIR
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ARCH
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_OS
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENV
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENDIAN
        OPT_LEVEL = Some(3)
        OUT_DIR =
      Some(C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-c536cf4a7f3e1597\out)
        TARGET = Some(aarch64-pc-windows-msvc)
        cargo:rerun-if-env-changed=VCINSTALLDIR
        VCINSTALLDIR = None
        cargo:rerun-if-env-changed=VSTEL_MSBuildProjectFullPath
        VSTEL_MSBuildProjectFullPath = None
        cargo:rerun-if-env-changed=VSCMD_ARG_VCVARS_SPECTRE
        VSCMD_ARG_VCVARS_SPECTRE = None
        cargo:rerun-if-env-changed=WindowsSdkDir
        WindowsSdkDir = None
        cargo:rerun-if-env-changed=WindowsSDKVersion
        WindowsSDKVersion = None
        cargo:rerun-if-env-changed=LIB
        LIB = None
        PATH =
      Some(C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\deps;C:\Users\<UserName>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release;C:\Users\<UserName>\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\lib\rustlib\aarch64-pc-windows-msvc\lib;C:\Users\<UserName>\AppData\Local\uv\cache\builds-v0\.tmpw0rdR8\Scripts;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program
      Files\Git\cmd;C:\Program Files (x86)\Windows Kits\10\Windows Performance Toolkit\;C:\Program
      Files\dotnet\;C:\Users\<UserName>\.cargo\bin;C:\Users\<UserName>\.local\bin;C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Scripts\;C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\;C:\Users\<UserName>\AppData\Local\Programs\Python\Launcher\;C:\Users\<UserName>\AppData\Local\Microsoft\WindowsApps;C:\Users\<UserName>\AppData\Roaming\Python\Scripts;C:\Users\<UserName>\.dotnet\tools;C:\Program
      Files\Microsoft Visual
      Studio\2022\Community\VC\Tools\MSVC\14.44.35207\lib\x64;;C:\Users\<UserName>\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\bin)
        cargo:rerun-if-env-changed=INCLUDE
        INCLUDE = None
        HOST = Some(aarch64-pc-windows-msvc)
        cargo:rerun-if-env-changed=CC_aarch64-pc-windows-msvc
        CC_aarch64-pc-windows-msvc = None
        cargo:rerun-if-env-changed=CC_aarch64_pc_windows_msvc
        CC_aarch64_pc_windows_msvc = None
        cargo:rerun-if-env-changed=HOST_CC
        HOST_CC = None
        cargo:rerun-if-env-changed=CC
        CC = None
        cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
        CRATE_CC_NO_DEFAULTS = None
        CARGO_CFG_TARGET_FEATURE = Some(neon)
        DEBUG = Some(true)
        cargo:rerun-if-env-changed=CFLAGS
        CFLAGS = None
        cargo:rerun-if-env-changed=HOST_CFLAGS
        HOST_CFLAGS = None
        cargo:rerun-if-env-changed=CFLAGS_aarch64_pc_windows_msvc
        CFLAGS_aarch64_pc_windows_msvc = None
        cargo:rerun-if-env-changed=CFLAGS_aarch64-pc-windows-msvc
        CFLAGS_aarch64-pc-windows-msvc = None
        CARGO_ENCODED_RUSTFLAGS = Some()
        cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang":
      program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for help)

        --- stderr


        error occurred in cc-rs: failed to find tool "clang": program not found (see
      https://docs.rs/cc/latest/cc/#compile-time-requirements for help)


      warning: build failed, waiting for other jobs to finish...
      💥 maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit code: 101": `"cargo"
      "rustc" "--message-format" "json-render-diagnostics" "--manifest-path"
      "C:\\Users\\<UserName>\\AppData\\Local\\uv\\cache\\sdists-v9\\pypi\\polars-lts-cpu\\1.33.0\\hAXhuRtb3NI4cwW3tIBny\\src\\py-polars\\Cargo.toml"
      "--release" "--lib"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i',
      'C:\\Users\\<UserName>\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpw0rdR8\\Scripts\\python.exe',
      '--compatibility', 'off'] returned non-zero exit status 1

      hint: This usually indicates a problem with the package or the build environment.
  help: `polars-lts-cpu` (v1.33.0) was included because `small-example` (v0.1.0) depends on `polars-lts-cpu`
DEBUG Released lock at `C:\Users\<UserName>\Documents\projects\small-example\.venv\.lock`
```



I assume that compiler tools refer to the Visual Studio work loads and individual components. I have "C++ Modules for v143 build tools (ARM64 - experimental)" and "C++ Universal Windows Platform support for v143 build tools (ARM64/ARM64EC)". I have tried to look in the uv documentation to figure out how to specify or infer which of x86_64 or arm64 is used for uv, but without luck. Can you help point me in the right direction to find out?

---

_Comment by @laurafroelich on 2025-09-05 12:22_

Hi again,

I realized that I had both x86_64 and ARM compiler tools. I have now tried `uv sync` and `pip install .` with only ARM or x86_64 compiler tools installed. I have also upgraded uv to version `uv 0.8.15 (8473ecba1 2025-09-03)`. I still see the same behavior of pip using `polars_lts_cpu-1.33.0-cp39-abi3-win_amd64.whl` and uv trying to build.

---

_Comment by @zsol on 2025-09-05 12:37_

Have you tried running `uv sync`from a developer (power)shell? 

---

_Comment by @laurafroelich on 2025-09-05 13:13_

I hadn't, but now I did, thanks for the suggestion. I have a special character in my name (the Danish letter ø), which is part of my user name. In the logs from the developer powershell, this ø is replaced with o in some places (<UserName> indicates my actual user name, and <UserName2> indicates the username where ø was replaced with o). I did not see this in the regular powershell. I don't know if that could be why I am now getting a FileNotFoundError, or if that is unrelated. 

```
PS C:\Users\<UserName>\Documents\projects\small-example>      Python reports SOABI: cp313-win_amd64
>>       Computed rustc target triple: x86_64-pc-windows-msvc     Python reports SOABI: cp313-win_amd64
>>       Computed rustc target triple: x86_64-pc-windows-msvc^C
PS C:\Users\<UserName>\Documents\projects\small-example> ^C
PS C:\Users\<UserName>\Documents\projects\small-example>  C:\Users\<UserName>\.local\bin\uv sync -v
DEBUG uv 0.8.15 (8473ecba1 2025-09-03)
DEBUG Found project root: `C:\Users\<UserName>\Documents\projects\small-example`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\Users\<UserName>\Documents\projects\small-example`
DEBUG Reading Python requests from version file at `C:\Users\<UserName>\Documents\projects\small-example\.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
DEBUG Released lock at `C:\Users\LAURAF~1\AppData\Local\Temp\uv-4d904aeae39cf5e9.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: small-example @ file:///C:/Users/LauraFr%C3%B8lich/Documents/projects/small-example
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 3 packages in 2ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: polars-lts-cpu==1.33.0
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/28/b8/3763cc3ea3fdfa39057ce105a477789f13c5278e47f2ce5fe507e8a4d2e4/polars_lts_cpu-1.33.0.tar.gz
   Building polars-lts-cpu==1.33.0
DEBUG Building: polars-lts-cpu==1.33.0
DEBUG Not using uv build backend direct build of polars-lts-cpu==1.33.0, no pyproject.toml: TOML parse error at line 5, column 1
  |
5 | [project]
  | ^^^^^^^^^
missing field `version`

DEBUG Reusing existing build environment for: polars-lts-cpu==1.33.0
DEBUG Using base executable for virtual environment: C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\python.exe
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13.5
DEBUG Adding direct dependency: maturin>=1.3.2
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\maturin.lock`
DEBUG Found stale response for: https://pypi.org/simple/maturin/
DEBUG Sending revalidation request for: https://pypi.org/simple/maturin/
DEBUG Found not-modified response for: https://pypi.org/simple/maturin/
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\maturin.lock`
DEBUG Searching for a compatible version of maturin (>=1.3.2)
DEBUG Selecting: maturin==1.9.4 [compatible] (maturin-1.9.4-py3-none-win_amd64.whl)
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\maturin\maturin-1.9.4-py3-none-win_amd64.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/14/14/f86d0124bf1816b99005c058a1dbdca7cb5850d9cf4b09dcae07a1bc6201/maturin-1.9.4-py3-none-win_amd64.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\maturin\maturin-1.9.4-py3-none-win_amd64.lock`
DEBUG Tried 1 versions: maturin 1
DEBUG marker environment resolution took 0.177s
DEBUG Installing in maturin==1.9.4 in C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap
DEBUG Registry requirement already cached: maturin==1.9.4
DEBUG Installing build requirement: maturin==1.9.4
DEBUG Creating PEP 517 build environment
DEBUG Calling `maturin.get_requires_for_build_wheel()`
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13.5
DEBUG Adding direct dependency: maturin>=1.3.2
DEBUG Adding direct dependency: puccinialin*
DEBUG Searching for a compatible version of maturin (>=1.3.2)
DEBUG Selecting: maturin==1.9.4 [compatible] (maturin-1.9.4-py3-none-win_amd64.whl)
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\puccinialin.lock`
DEBUG Found stale response for: https://pypi.org/simple/puccinialin/
DEBUG Sending revalidation request for: https://pypi.org/simple/puccinialin/
DEBUG Found not-modified response for: https://pypi.org/simple/puccinialin/
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\puccinialin.lock`
DEBUG Searching for a compatible version of puccinialin (*)
DEBUG Selecting: puccinialin==0.1.5 [compatible] (puccinialin-0.1.5-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\puccinialin\puccinialin-0.1.5-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b0/25/91d671da2a6d98e95094f7456754c55d972febcc35de3b6d4e9a9017c445/puccinialin-0.1.5-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\puccinialin\puccinialin-0.1.5-py3-none-any.lock`
DEBUG Adding transitive dependency for puccinialin==0.1.5: httpx>=0.28.1, <0.29
DEBUG Adding transitive dependency for puccinialin==0.1.5: platformdirs>=4.3.6, <5
DEBUG Adding transitive dependency for puccinialin==0.1.5: tqdm>=4.67.1, <5
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\httpx.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\platformdirs.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\tqdm.lock`
DEBUG Found stale response for: https://pypi.org/simple/httpx/
DEBUG Sending revalidation request for: https://pypi.org/simple/httpx/
DEBUG Found stale response for: https://pypi.org/simple/tqdm/
DEBUG Sending revalidation request for: https://pypi.org/simple/tqdm/
DEBUG Found stale response for: https://pypi.org/simple/platformdirs/
DEBUG Sending revalidation request for: https://pypi.org/simple/platformdirs/
DEBUG Found not-modified response for: https://pypi.org/simple/tqdm/
DEBUG Found not-modified response for: https://pypi.org/simple/httpx/
DEBUG Found not-modified response for: https://pypi.org/simple/platformdirs/
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\tqdm.lock`
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\httpx.lock`
DEBUG Searching for a compatible version of httpx (>=0.28.1, <0.29)
DEBUG Selecting: httpx==0.28.1 [compatible] (httpx-0.28.1-py3-none-any.whl)
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\platformdirs.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\tqdm\tqdm-4.67.1-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.4.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\httpx\httpx-0.28.1-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/40/4b/2028861e724d3bd36227adfa20d3fd24c3fc6d52032f4a93c133be5d17ce/platformdirs-4.4.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.4.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/30/dc54f88dd4a2b5dc8a0279bdd7270e735851848b762aeb1c1184ed1f6b14/tqdm-4.67.1-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\tqdm\tqdm-4.67.1-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2a/39/e50c7c3a983047577ee07d2a9e53faf5a69493943ec3f6a384bdc792deb2/httpx-0.28.1-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\httpx\httpx-0.28.1-py3-none-any.lock`
DEBUG Adding transitive dependency for httpx==0.28.1: anyio*
DEBUG Adding transitive dependency for httpx==0.28.1: certifi*
DEBUG Adding transitive dependency for httpx==0.28.1: httpcore>=1.dev0, <2.dev0
DEBUG Adding transitive dependency for httpx==0.28.1: idna*
DEBUG Searching for a compatible version of platformdirs (>=4.3.6, <5)
DEBUG Selecting: platformdirs==4.4.0 [compatible] (platformdirs-4.4.0-py3-none-any.whl)
DEBUG Searching for a compatible version of tqdm (>=4.67.1, <5)
DEBUG Selecting: tqdm==4.67.1 [compatible] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\httpcore.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\anyio.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\certifi.lock`
DEBUG Found stale response for: https://pypi.org/simple/httpcore/
DEBUG Sending revalidation request for: https://pypi.org/simple/httpcore/
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\idna.lock`
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{sys_platform == 'win32'}*
DEBUG Found stale response for: https://pypi.org/simple/anyio/
DEBUG Sending revalidation request for: https://pypi.org/simple/anyio/
DEBUG Found stale response for: https://pypi.org/simple/certifi/
DEBUG Sending revalidation request for: https://pypi.org/simple/certifi/
DEBUG Found stale response for: https://pypi.org/simple/idna/
DEBUG Sending revalidation request for: https://pypi.org/simple/idna/
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\colorama.lock`
DEBUG Found stale response for: https://pypi.org/simple/colorama/
DEBUG Sending revalidation request for: https://pypi.org/simple/colorama/
DEBUG Found not-modified response for: https://pypi.org/simple/certifi/
DEBUG Found not-modified response for: https://pypi.org/simple/idna/
DEBUG Found not-modified response for: https://pypi.org/simple/anyio/
DEBUG Found not-modified response for: https://pypi.org/simple/httpcore/
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\certifi.lock`
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\idna.lock`
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\anyio.lock`
DEBUG Searching for a compatible version of anyio (*)
DEBUG Selecting: anyio==4.10.0 [compatible] (anyio-4.10.0-py3-none-any.whl)
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\httpcore.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\certifi\certifi-2025.8.3-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\idna\idna-3.10-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\httpcore\httpcore-1.0.9-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\anyio\anyio-4.10.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7e/f5/f66802a942d491edb555dd61e3a9961140fd64c90bce1eafd741609d334d/httpcore-1.0.9-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\httpcore\httpcore-1.0.9-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e5/48/1549795ba7742c948d2ad169c1c8cdbae65bc450d6cd753d124b17c8cd32/certifi-2025.8.3-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\certifi\certifi-2025.8.3-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\idna\idna-3.10-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6f/12/e5e0282d673bb9746bacfb6e2dba8719989d3660cdb2ea79aee9a9651afb/anyio-4.10.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\anyio\anyio-4.10.0-py3-none-any.lock`
DEBUG Adding transitive dependency for anyio==4.10.0: idna>=2.8
DEBUG Adding transitive dependency for anyio==4.10.0: sniffio>=1.1
DEBUG Searching for a compatible version of certifi (*)
DEBUG Selecting: certifi==2025.8.3 [compatible] (certifi-2025.8.3-py3-none-any.whl)
DEBUG Searching for a compatible version of httpcore (>=1.dev0, <2.dev0)
DEBUG Selecting: httpcore==1.0.9 [compatible] (httpcore-1.0.9-py3-none-any.whl)
DEBUG Adding transitive dependency for httpcore==1.0.9: certifi*
DEBUG Found not-modified response for: https://pypi.org/simple/colorama/
DEBUG Adding transitive dependency for httpcore==1.0.9: h11>=0.16
DEBUG Searching for a compatible version of idna (>=2.8)
DEBUG Selecting: idna==3.10 [compatible] (idna-3.10-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\sniffio.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\h11.lock`
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\colorama.lock`
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/sniffio/
DEBUG Sending revalidation request for: https://pypi.org/simple/sniffio/
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Found stale response for: https://pypi.org/simple/h11/
DEBUG Sending revalidation request for: https://pypi.org/simple/h11/
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock`
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Found not-modified response for: https://pypi.org/simple/h11/
DEBUG Found not-modified response for: https://pypi.org/simple/sniffio/
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\h11.lock`
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\simple-v17\pypi\sniffio.lock`
DEBUG Searching for a compatible version of sniffio (>=1.1)
DEBUG Selecting: sniffio==1.3.1 [compatible] (sniffio-1.3.1-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\h11\h11-0.16.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\sniffio\sniffio-1.3.1-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\h11\h11-0.16.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\wheels-v5\pypi\sniffio\sniffio-1.3.1-py3-none-any.lock`
DEBUG Searching for a compatible version of h11 (>=0.16)
DEBUG Selecting: h11==0.16.0 [compatible] (h11-0.16.0-py3-none-any.whl)
DEBUG Tried 12 versions: anyio 1, certifi 1, colorama 1, h11 1, httpcore 1, httpx 1, idna 1, maturin 1, platformdirs 1, puccinialin 1, sniffio 1, tqdm 1
DEBUG marker environment resolution took 0.168s
DEBUG Installing in idna==3.10, tqdm==4.67.1, httpx==0.28.1, h11==0.16.0, maturin==1.9.4, anyio==4.10.0, colorama==0.4.6, httpcore==1.0.9, platformdirs==4.4.0, sniffio==1.3.1, certifi==2025.8.3, puccinialin==0.1.5 in C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: tqdm==4.67.1
DEBUG Registry requirement already cached: httpx==0.28.1
DEBUG Registry requirement already cached: h11==0.16.0
DEBUG Requirement already installed: maturin==1.9.4
DEBUG Registry requirement already cached: anyio==4.10.0
DEBUG Registry requirement already cached: colorama==0.4.6
DEBUG Registry requirement already cached: httpcore==1.0.9
DEBUG Registry requirement already cached: platformdirs==4.4.0
DEBUG Registry requirement already cached: sniffio==1.3.1
DEBUG Registry requirement already cached: certifi==2025.8.3
DEBUG Registry requirement already cached: puccinialin==0.1.5
DEBUG Installing build requirements: idna==3.10, tqdm==4.67.1, httpx==0.28.1, h11==0.16.0, anyio==4.10.0, colorama==0.4.6, httpcore==1.0.9, platformdirs==4.4.0, sniffio==1.3.1, certifi==2025.8.3, puccinialin==0.1.5
DEBUG Calling `maturin.build_wheel("C:\\Users\\<UserName2>\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpQFqrZp", {}, None)`
DEBUG Running `maturin pep517 build-wheel -i C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Scripts\python.exe --compatibility off`
DEBUG Python reports SOABI: cp313-win_amd64
DEBUG Computed rustc target triple: x86_64-pc-windows-msvc
DEBUG Installation directory: C:\Users\<UserName2>\AppData\Local\puccinialin\puccinialin\Cache
DEBUG Rustup already downloaded
DEBUG Installing rust to C:\Users\<UserName2>\AppData\Local\puccinialin\puccinialin\Cache\rustup
DEBUG warn: It looks like you have an existing rustup settings file at:
DEBUG warn: C:\Users\<UserName2>\.rustup\settings.toml
DEBUG warn: Rustup will install the default toolchain as specified in the settings file,
DEBUG warn: instead of the one inferred from the default host triple.
DEBUG info: profile set to 'minimal'
DEBUG info: default host triple is aarch64-pc-windows-msvc
DEBUG warn: Updating existing toolchain, profile choice will be ignored
DEBUG info: syncing channel updates for 'stable-aarch64-pc-windows-msvc'
DEBUG info: default toolchain set to 'stable-aarch64-pc-windows-msvc'
DEBUG Checking if cargo is installed
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 11, in <module>
DEBUG     wheel_filename = backend.build_wheel("C:\\Users\\<UserName2>\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpQFqrZp", {}, None)
DEBUG   File "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\maturin\__init__.py", line 133, in build_wheel
DEBUG     return _build_wheel(wheel_directory, config_settings, metadata_directory)
DEBUG   File "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\maturin\__init__.py", line 114, in _build_wheel
DEBUG     result = subprocess.run(command, stdout=subprocess.PIPE, env=_get_env())
DEBUG                                                                  ~~~~~~~~^^
DEBUG   File "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\maturin\__init__.py", line 76, in _get_env
DEBUG     extra_env = setup_rust()
DEBUG   File "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\puccinialin\_setup_rust.py", line 134, in setup_rust
DEBUG     check_call(["cargo", "--version"], env={**os.environ, **extra_env})
DEBUG     ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 414, in check_call
DEBUG     retcode = call(*popenargs, **kwargs)
DEBUG   File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 395, in call
DEBUG     with Popen(*popenargs, **kwargs) as p:
DEBUG          ~~~~~^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 1039, in __init__
DEBUG     self._execute_child(args, executable, preexec_fn, close_fds,
DEBUG     ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG                         pass_fds, cwd, env,
DEBUG                         ^^^^^^^^^^^^^^^^^^^
DEBUG     ...<5 lines>...
DEBUG                         gid, gids, uid, umask,
DEBUG                         ^^^^^^^^^^^^^^^^^^^^^^
DEBUG                         start_new_session, process_group)
DEBUG                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 1554, in _execute_child
DEBUG     hp, ht, pid, tid = _winapi.CreateProcess(executable, args,
DEBUG                        ~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^
DEBUG                              # no special security
DEBUG                              ^^^^^^^^^^^^^^^^^^^^^
DEBUG     ...<4 lines>...
DEBUG                              cwd,
DEBUG                              ^^^^
DEBUG                              startupinfo)
DEBUG                              ^^^^^^^^^^^^
DEBUG FileNotFoundError: [WinError 2] The system cannot find the file specified
DEBUG Rust not found, installing into a temporary directory
DEBUG Released lock at `C:\Users\<UserName2>\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\.lock`
  x Failed to build `polars-lts-cpu==1.33.0`
  |-> The build backend returned an error
  `-> Call to `maturin.build_wheel` failed (exit code: 1)

      [stdout]
      Running `maturin pep517 build-wheel -i
      C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Scripts\python.exe --compatibility off`
      Rust not found, installing into a temporary directory

      [stderr]
      Python reports SOABI: cp313-win_amd64
      Computed rustc target triple: x86_64-pc-windows-msvc
      Installation directory: C:\Users\<UserName2>\AppData\Local\puccinialin\puccinialin\Cache
      Rustup already downloaded
      Installing rust to C:\Users\<UserName2>\AppData\Local\puccinialin\puccinialin\Cache\rustup
      warn: It looks like you have an existing rustup settings file at:
      warn: C:\Users\<UserName2>\.rustup\settings.toml
      warn: Rustup will install the default toolchain as specified in the settings file,
      warn: instead of the one inferred from the default host triple.
      info: profile set to 'minimal'
      info: default host triple is aarch64-pc-windows-msvc
      warn: Updating existing toolchain, profile choice will be ignored
      info: syncing channel updates for 'stable-aarch64-pc-windows-msvc'
      info: default toolchain set to 'stable-aarch64-pc-windows-msvc'
      Checking if cargo is installed
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
          wheel_filename =
      backend.build_wheel("C:\\Users\\<UserName2>\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpQFqrZp", {}, None)
        File "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\maturin\__init__.py",
      line 133, in build_wheel
          return _build_wheel(wheel_directory, config_settings, metadata_directory)
        File "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\maturin\__init__.py",
      line 114, in _build_wheel
          result = subprocess.run(command, stdout=subprocess.PIPE, env=_get_env())
                                                                       ~~~~~~~~^^
        File "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\maturin\__init__.py",
      line 76, in _get_env
          extra_env = setup_rust()
        File
      "C:\Users\<UserName2>\AppData\Local\uv\cache\builds-v0\.tmpVtp5ap\Lib\site-packages\puccinialin\_setup_rust.py",
      line 134, in setup_rust
          check_call(["cargo", "--version"], env={**os.environ, **extra_env})
          ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 414, in
      check_call
          retcode = call(*popenargs, **kwargs)
        File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 395, in call
          with Popen(*popenargs, **kwargs) as p:
               ~~~~~^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 1039, in __init__
          self._execute_child(args, executable, preexec_fn, close_fds,
          ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                              pass_fds, cwd, env,
                              ^^^^^^^^^^^^^^^^^^^
          ...<5 lines>...
                              gid, gids, uid, umask,
                              ^^^^^^^^^^^^^^^^^^^^^^
                              start_new_session, process_group)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\subprocess.py", line 1554, in
      _execute_child
          hp, ht, pid, tid = _winapi.CreateProcess(executable, args,
                             ~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^
                                   # no special security
                                   ^^^^^^^^^^^^^^^^^^^^^
          ...<4 lines>...
                                   cwd,
                                   ^^^^
                                   startupinfo)
                                   ^^^^^^^^^^^^
      FileNotFoundError: [WinError 2] The system cannot find the file specified

      hint: This usually indicates a problem with the package or the build environment.
  help: `polars-lts-cpu` (v1.33.0) was included because `small-example` (v0.1.0) depends on `polars-lts-cpu`
DEBUG Released lock at `C:\Users\<UserName>\Documents\projects\small-example\.venv\.lock`
```

---

_Comment by @konstin on 2025-09-05 13:19_

The surprising thing I'm not understand is why we're trying to build at all, there are wheels that we can use on the index. The build failure is a different question, though it's odd that Rust is not available, when it was available in the original report (maybe just not in `PATH` here?).

Can you share the output of the first one in the shell and the second one in a `uv run --no-sync` Python interpreter.

```
pip debug --verbose
```

```python
import platform
import sys
import os

print("os_name:", os.name)
print("sys_platform:", sys.platform)
print("platform_machine:", platform.machine())
print("platform_python_implementation:", platform.python_implementation())
print("platform_release:", platform.release())
print("platform_system:", platform.system())
print("platform_version:", platform.version())
print("python_version:", '.'.join(map(str, sys.version_info[:2])))
print("python_full_version:", '.'.join(map(str, sys.version_info[:3])))
print("implementation_name:", sys.implementation.name)
print("implementation_version:", '.'.join(map(str, sys.implementation.version[:2])))
```

---

_Comment by @laurafroelich on 2025-09-05 17:09_

True, good point about why it's not just using the wheel.

I have pasted the output from the commands below.

This is from PowerShell (standard, non-developer), happy to run it from the developer version instead if relevant.

```
PS C:\Users\<UserName>\Documents\projects\small-example> pip debug --verbose
WARNING: This command is only meant for debugging. Do not use this with automation for parsing and getting these details, since the output and options of this command may change without notice.
pip version: pip 25.1.1 from C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\site-packages\pip (python 3.13)
sys.version: 3.13.5 (tags/v3.13.5:6cb20a2, Jun 11 2025, 16:15:46) [MSC v.1943 64 bit (AMD64)]
sys.executable: C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\python.exe
sys.getdefaultencoding: utf-8
sys.getfilesystemencoding: utf-8
locale.getpreferredencoding: cp1252
sys.platform: win32
sys.implementation:
  name: cpython
'cert' config value: Not specified
REQUESTS_CA_BUNDLE: None
CURL_CA_BUNDLE: None
pip._vendor.certifi.where(): C:\Users\<UserName>\AppData\Local\Programs\Python\Python313\Lib\site-packages\pip\_vendor\certifi\cacert.pem
pip._vendor.DEBUNDLED: False
vendored library versions:
  CacheControl==0.14.2
  distlib==0.3.9
  distro==1.9.0
  msgpack==1.1.0
  packaging==25.0
  platformdirs==4.3.7
  pyproject-hooks==1.2.0
  requests==2.32.3
  certifi==2025.01.31
  idna==3.10
  urllib3==1.26.20
  rich==14.0.0 (Unable to locate actual module version, using vendor.txt specified version)
  pygments==2.19.1
  typing_extensions==4.13.2 (Unable to locate actual module version, using vendor.txt specified version)
  resolvelib==1.1.0
  setuptools==70.3.0 (Unable to locate actual module version, using vendor.txt specified version)
  tomli==2.2.1
  tomli-w==1.2.0
  truststore==0.10.1
  dependency-groups==1.3.1 (Unable to locate actual module version, using vendor.txt specified version)
Compatible tags: 45
  cp313-cp313-win_amd64
  cp313-abi3-win_amd64
  cp313-none-win_amd64
  cp312-abi3-win_amd64
  cp311-abi3-win_amd64
  cp310-abi3-win_amd64
  cp39-abi3-win_amd64
  cp38-abi3-win_amd64
  cp37-abi3-win_amd64
  cp36-abi3-win_amd64
  cp35-abi3-win_amd64
  cp34-abi3-win_amd64
  cp33-abi3-win_amd64
  cp32-abi3-win_amd64
  py313-none-win_amd64
  py3-none-win_amd64
  py312-none-win_amd64
  py311-none-win_amd64
  py310-none-win_amd64
  py39-none-win_amd64
  py38-none-win_amd64
  py37-none-win_amd64
  py36-none-win_amd64
  py35-none-win_amd64
  py34-none-win_amd64
  py33-none-win_amd64
  py32-none-win_amd64
  py31-none-win_amd64
  py30-none-win_amd64
  cp313-none-any
  py313-none-any
  py3-none-any
  py312-none-any
  py311-none-any
  py310-none-any
  py39-none-any
  py38-none-any
  py37-none-any
  py36-none-any
  py35-none-any
  py34-none-any
  py33-none-any
  py32-none-any
  py31-none-any
```



```
PS C:\Users\<UserName>\Documents\projects\small-example> uv run --no-sync .\cmds.py
os_name: nt
sys_platform: win32
platform_machine: ARM64
platform_python_implementation: CPython
platform_release: 11
platform_system: Windows
platform_version: 10.0.26100
python_version: 3.13
python_full_version: 3.13.5
implementation_name: cpython
implementation_version: 3.13
```


---

_Comment by @konstin on 2025-09-09 17:17_

uv and pip are using different Pythons, can you check `sys.executable` and `sys.base_executable` in the uv Python to figure out which Python it is? Can you also share `uv python list`?

---

_Comment by @zsol on 2025-09-09 18:01_

I was tinkering with this on my own arm64 windows machine, and I've noticed a few things:
- in a normal shell, I get the same error as @laurafroelich mentions (missing `clang.exe`)
- in a developer shell however, the build proceeds until the linking step, at which point it aborts with tons of undefined references, and it seems like it's trying to link an arm64 object file against an x86_64 (**amd**64) CPython

Indeed, when running with `uv sync -vvv`, I notice that `maturin` is being invoked with:
```
DEBUG Running `maturin pep517 build-wheel -i C:\Users\zsolz\AppData\Local\uv\cache\builds-v0\.tmp5ysbeE\Scripts\python.exe --compatibility off`
```
And that python is an x86_64 build: 
```
> C:\Users\zsolz\AppData\Local\uv\cache\builds-v0\.tmp5ysbeE\Scripts\python.exe
Python 3.13.7 (main, Sep  2 2025, 14:16:00) [MSC v.1944 64 bit (AMD64)] on win32
```

I think this works with @laurafroelich's pip invocation, because I suspect that `pip` is actually using an x86_64 CPython interpreter, and ends up building the extension also for x86_64. uv however is trying to build the extension for arm64. I suspect this is down to the build backend (of polars-lts-cpu), so we'd need to dig into that a bit more.

---

_Comment by @konstin on 2025-09-09 18:13_

But there's both 
`polars_lts_cpu-1.33.0-cp39-abi3-win_amd64.whl` and `polars_lts_cpu-1.33.0-cp39-abi3-win_arm64.whl`, we should not be building at all. There's a story about compiling on these platforms and setting all these dev tools up; This can be an interesting discussion for maturin. But there's a bug that's either in uv or affects uv that it doesn't select the right package, while pip does.

---

_Comment by @zsol on 2025-09-10 08:44_

Right, I think we don't consider the `win_amd64.whl` because
```toml
dependencies = [    
    "polars; platform_machine=='x86_64' or platform_machine == 'AMD64'",
    "polars-lts-cpu; platform_machine=='ARM64'"]
```
But we _do_ try and resolve for an amd64 platform, because that's the interpreter's architecture. That's why we end up using the sdist to attempt to build a wheel, but during the build something decides to build for the host's platform (arm64), which is how I've seen the linker errors.

@laurafroelich you can force the situation by passing `--python-platform aarch64-pc-windows-msvc`, which would make uv fetch the wheels, but those wheels would still be for arm64, and your venv will be using a python interpreter for x86_64. I'm not sure if that would work at runtime, you'd need to test. 
If I were you, I'd use an arm64 CPython instead, which would hopefully make all of this go away. Unfortunately today uv by default uses an x86_64 interpreter on Windows/arm64 (see https://github.com/astral-sh/uv/issues/12906).

---

_Comment by @laurafroelich on 2025-09-10 15:28_

> uv and pip are using different Pythons, can you check `sys.executable` and `sys.base_executable` in the uv Python to figure out which Python it is? Can you also share `uv python list`?

Here is the output from `sys.executable`, it looks like `sys.base_executable` is not found. Please let me know if I should do something differently to get it working.

```
PS C:\Users\LauraFrølich\Documents\projects> uv run python
Python 3.13.5 (tags/v3.13.5:6cb20a2, Jun 11 2025, 16:15:46) [MSC v.1943 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.executable
'C:\\Users\\LauraFrølich\\AppData\\Local\\Programs\\Python\\Python313\\python.exe'
>>> sys.base_executable
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    sys.base_executable
AttributeError: module 'sys' has no attribute 'base_executable'
```

And this is from `uv python list`:

```
PS C:\Users\LauraFrølich\Documents\projects> uv python list
cpython-3.14.0rc2-windows-x86_64-none                 <download available>
cpython-3.14.0rc2+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.7-windows-x86_64-none                    <download available>
cpython-3.13.7+freethreaded-windows-x86_64-none       <download available>
cpython-3.13.5-windows-x86_64-none                    C:\Users\LauraFrølich\AppData\Local\Programs\Python\Python313\python.exe
cpython-3.12.11-windows-x86_64-none                   <download available>
cpython-3.11.13-windows-x86_64-none                   <download available>
cpython-3.10.18-windows-x86_64-none                   <download available>
cpython-3.9.23-windows-x86_64-none                    <download available>
cpython-3.8.20-windows-x86_64-none                    <download available>
pypy-3.11.13-windows-x86_64-none                      <download available>
pypy-3.10.16-windows-x86_64-none                      <download available>
pypy-3.9.19-windows-x86_64-none                       <download available>
pypy-3.8.16-windows-x86_64-none                       <download available>
graalpy-3.11.0-windows-x86_64-none                    <download available>
graalpy-3.10.0-windows-x86_64-none                    <download available>
PS C:\Users\LauraFrølich\Documents\projects>
```

---

_Comment by @laurafroelich on 2025-09-10 15:42_

> Right, I think we don't consider the `win_amd64.whl` because
> 
> dependencies = [    
>     "polars; platform_machine=='x86_64' or platform_machine == 'AMD64'",
>     "polars-lts-cpu; platform_machine=='ARM64'"]
> But we _do_ try and resolve for an amd64 platform, because that's the interpreter's architecture. That's why we end up using the sdist to attempt to build a wheel, but during the build something decides to build for the host's platform (arm64), which is how I've seen the linker errors.
> 
> [@laurafroelich](https://github.com/laurafroelich) you can force the situation by passing `--python-platform aarch64-pc-windows-msvc`, which would make uv fetch the wheels, but those wheels would still be for arm64, and your venv will be using a python interpreter for x86_64. I'm not sure if that would work at runtime, you'd need to test. If I were you, I'd use an arm64 CPython instead, which would hopefully make all of this go away. Unfortunately today uv by default uses an x86_64 interpreter on Windows/arm64 (see [#12906](https://github.com/astral-sh/uv/issues/12906)).

You're right, passing the python-platform argument allows me to install `polars-lts-cpu` from the wheel when I use `sync:`:

```
PS C:\Users\LauraFrølich\Documents\projects\small-example> uv sync --python-platform aarch64-pc-windows-msvc
Using CPython 3.13.5 interpreter at: C:\Users\LauraFrølich\AppData\Local\Programs\Python\Python313\python.exe
Creating virtual environment at: .venv
Resolved 3 packages in 1ms
Prepared 1 package in 14.68s
Installed 1 package in 185ms
 + polars-lts-cpu==1.33.0
```

However, with `uv run python --python-platform aarch64-pc-windows-msvc` I get the same error as before (not sure if this is unsurprising):
```
PS C:\Users\LauraFrølich\Documents\projects\small-example> uv run python --python-platform aarch64-pc-windows-msvc
  x Failed to build `polars-lts-cpu==1.33.0`
  |-> The build backend returned an error
  `-> Call to `maturin.build_wheel` failed (exit code: 1)

      [stdout]
      Running `maturin pep517 build-wheel -i C:\Users\LauraFrølich\AppData\Local\uv\cache\builds-v0\.tmpYkQmJb\Scripts\python.exe --compatibility
      off`

      [stderr]
      🍹 Building a mixed python/rust project
      🔗 Found pyo3 bindings with abi3 support
         Compiling pyo3-build-config v0.25.1
         Compiling ring v0.17.14
         Compiling utf8_iter v1.0.4
         Compiling rustversion v1.0.21
         Compiling thiserror v2.0.12
         Compiling base64 v0.22.1
         Compiling tower-layer v0.3.3
         Compiling ipnet v2.11.0
      warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see
      https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
      warning: ring@0.17.14: Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see
      https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
      error: failed to run custom build command for `ring v0.17.14`

      Caused by:
        process didn't exit successfully:
      `C:\Users\LauraFrølich\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-ed18bd76a4a7bb4e\build-script-build`
      (exit code: 1)
        --- stdout
        cargo:rerun-if-env-changed=CARGO_MANIFEST_DIR
        cargo:rerun-if-env-changed=CARGO_PKG_NAME
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MAJOR
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_MINOR
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PATCH
        cargo:rerun-if-env-changed=CARGO_PKG_VERSION_PRE
        cargo:rerun-if-env-changed=CARGO_MANIFEST_LINKS
        cargo:rerun-if-env-changed=RING_PREGENERATE_ASM
        cargo:rerun-if-env-changed=OUT_DIR
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ARCH
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_OS
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENV
        cargo:rerun-if-env-changed=CARGO_CFG_TARGET_ENDIAN
        OPT_LEVEL = Some(3)
        OUT_DIR =
      Some(C:\Users\LauraFrølich\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\build\ring-c536cf4a7f3e1597\out)
        TARGET = Some(aarch64-pc-windows-msvc)
        cargo:rerun-if-env-changed=VCINSTALLDIR
        VCINSTALLDIR = None
        cargo:rerun-if-env-changed=VSTEL_MSBuildProjectFullPath
        VSTEL_MSBuildProjectFullPath = None
        cargo:rerun-if-env-changed=VSCMD_ARG_VCVARS_SPECTRE
        VSCMD_ARG_VCVARS_SPECTRE = None
        cargo:rerun-if-env-changed=WindowsSdkDir
        WindowsSdkDir = None
        cargo:rerun-if-env-changed=WindowsSDKVersion
        WindowsSDKVersion = None
        cargo:rerun-if-env-changed=LIB
        LIB = None
        PATH =
      Some(C:\Users\LauraFrølich\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release\deps;C:\Users\LauraFrølich\AppData\Local\uv\cache\sdists-v9\pypi\polars-lts-cpu\1.33.0\hAXhuRtb3NI4cwW3tIBny\src\target\release;C:\Users\LauraFrølich\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\lib\rustlib\aarch64-pc-windows-msvc\lib;C:\Users\LauraFrølich\AppData\Local\uv\cache\builds-v0\.tmpYkQmJb\Scripts;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program
      Files\Git\cmd;C:\Program Files\dotnet\;C:\Program Files (x86)\Windows Kits\10\Windows Performance
      Toolkit\;C:\Users\LauraFrølich\.cargo\bin;C:\Users\LauraFrølich\.local\bin;C:\Users\LauraFrølich\AppData\Local\Programs\Python\Python313\Scripts\;C:\Users\LauraFrølich\AppData\Local\Programs\Python\Python313\;C:\Users\LauraFrølich\AppData\Local\Programs\Python\Launcher\;C:\Users\LauraFrølich\AppData\Local\Microsoft\WindowsApps;C:\Users\LauraFrølich\AppData\Roaming\Python\Scripts;C:\Users\LauraFrølich\.dotnet\tools;C:\Program
      Files\Microsoft Visual
      Studio\2022\Community\VC\Tools\MSVC\14.44.35207\lib\x64;;C:\Users\LauraFrølich\.rustup\toolchains\nightly-2025-08-29-aarch64-pc-windows-msvc\bin)
        cargo:rerun-if-env-changed=INCLUDE
        INCLUDE = None
        HOST = Some(aarch64-pc-windows-msvc)
        cargo:rerun-if-env-changed=CC_aarch64-pc-windows-msvc
        CC_aarch64-pc-windows-msvc = None
        cargo:rerun-if-env-changed=CC_aarch64_pc_windows_msvc
        CC_aarch64_pc_windows_msvc = None
        cargo:rerun-if-env-changed=HOST_CC
        HOST_CC = None
        cargo:rerun-if-env-changed=CC
        CC = None
        cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
        CRATE_CC_NO_DEFAULTS = None
        CARGO_CFG_TARGET_FEATURE = Some(neon)
        DEBUG = Some(true)
        cargo:rerun-if-env-changed=CFLAGS
        CFLAGS = None
        cargo:rerun-if-env-changed=HOST_CFLAGS
        HOST_CFLAGS = None
        cargo:rerun-if-env-changed=CFLAGS_aarch64_pc_windows_msvc
        CFLAGS_aarch64_pc_windows_msvc = None
        cargo:rerun-if-env-changed=CFLAGS_aarch64-pc-windows-msvc
        CFLAGS_aarch64-pc-windows-msvc = None
        CARGO_ENCODED_RUSTFLAGS = Some()
        cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see
      https://docs.rs/cc/latest/cc/#compile-time-requirements for help)
        cargo:warning=Compiler family detection failed due to error: ToolNotFound: failed to find tool "clang": program not found (see
      https://docs.rs/cc/latest/cc/#compile-time-requirements for help)

        --- stderr


        error occurred in cc-rs: failed to find tool "clang": program not found (see https://docs.rs/cc/latest/cc/#compile-time-requirements for
      help)


      warning: build failed, waiting for other jobs to finish...
      💥 maturin failed
        Caused by: Failed to build a native library through cargo
        Caused by: Cargo build finished with "exit code: 101": `"cargo" "rustc" "--message-format" "json-render-diagnostics" "--manifest-path"
      "C:\\Users\\LauraFrølich\\AppData\\Local\\uv\\cache\\sdists-v9\\pypi\\polars-lts-cpu\\1.33.0\\hAXhuRtb3NI4cwW3tIBny\\src\\py-polars\\Cargo.toml"
      "--release" "--lib"`
      Error: command ['maturin', 'pep517', 'build-wheel', '-i',
      'C:\\Users\\LauraFrølich\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpYkQmJb\\Scripts\\python.exe', '--compatibility', 'off'] returned non-zero
      exit status 1

      hint: This usually indicates a problem with the package or the build environment.
  help: `polars-lts-cpu` (v1.33.0) was included because `small-example` (v0.1.0) depends on `polars-lts-cpu`
```

---

_Comment by @laurafroelich on 2025-09-10 15:56_

You were also right that using the appropriate python solved the issue:

```
PS C:\Users\LauraFrølich\Documents\projects\small-example> uv python list
cpython-3.14.0rc2-windows-x86_64-none                 <download available>
cpython-3.14.0rc2+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.7-windows-x86_64-none                    <download available>
cpython-3.13.7+freethreaded-windows-x86_64-none       <download available>
cpython-3.13.7-windows-aarch64-none                   C:\Users\LauraFrølich\AppData\Local\Programs\Python\Python313-arm64\python.exe
cpython-3.12.11-windows-x86_64-none                   <download available>
cpython-3.11.13-windows-x86_64-none                   <download available>
cpython-3.10.18-windows-x86_64-none                   <download available>
cpython-3.9.23-windows-x86_64-none                    <download available>
cpython-3.8.20-windows-x86_64-none                    <download available>
pypy-3.11.13-windows-x86_64-none                      <download available>
pypy-3.10.16-windows-x86_64-none                      <download available>
pypy-3.9.19-windows-x86_64-none                       <download available>
pypy-3.8.16-windows-x86_64-none                       <download available>
graalpy-3.11.0-windows-x86_64-none                    <download available>
graalpy-3.10.0-windows-x86_64-none                    <download available>
PS C:\Users\LauraFrølich\Documents\projects\small-example> uv sync
Using CPython 3.13.7 interpreter at: C:\Users\LauraFrølich\AppData\Local\Programs\Python\Python313-arm64\python.exe
Creating virtual environment at: .venv
Resolved 3 packages in 2ms
Installed 1 package in 70ms
 + polars-lts-cpu==1.33.0
PS C:\Users\LauraFrølich\Documents\projects\small-example> uv run python
Python 3.13.7 (tags/v3.13.7:bcee1c3, Aug 14 2025, 14:57:06) [MSC v.1944 64 bit (ARM64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import polars as pl
>>> pl.DataFrame()
shape: (0, 0)
┌┐
╞╡
└┘
```

Thanks for the help and advice!

---

_Comment by @zsol on 2025-09-10 16:18_

> However, with uv run python --python-platform aarch64-pc-windows-msvc I get the same error as before (not sure if this is unsurprising):

Oh that'll probably be because the `--python-platform` flag in this case would be passed to `python`, and not `uv` (but of course we don't actually get to running the `python` binary because of the build issue).
Try
```
uv run --python-platform aarch64-pc-windows-msvc python
```

---

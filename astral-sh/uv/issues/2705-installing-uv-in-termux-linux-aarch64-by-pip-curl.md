```yaml
number: 2705
title: "Installing uv in Termux: Linux-aarch64 (by pip & curl)"
type: issue
state: closed
author: b9Joker108
labels:
  - duplicate
assignees: []
created_at: 2024-03-28T03:15:47Z
updated_at: 2025-05-23T08:42:54Z
url: https://github.com/astral-sh/uv/issues/2705
synced_at: 2026-01-12T15:58:40Z
```

# Installing uv in Termux: Linux-aarch64 (by pip & curl)

---

_@b9Joker108_

Hi

In Termux, I had an issue installing: `pip install uv`, due to not being able to build a wheel. My python-pip is up to date with Termux repository, but Termux may be behind in packaging a new python-pip release.  All my Termux packages are up to date.

```zsh
❯ pip install uv
Collecting uv
  Using cached uv-0.1.24.tar.gz (598 kB)
  Installing build dependencies ... error
  error: subprocess-exited-with-error

  × pip subprocess to install build dependencies did not run successfully.
  │ exit code: 1
  ╰─> [57 lines of output]
      Collecting maturin<2.0,>=1.0
        Using cached maturin-1.5.1.tar.gz (181 kB)
        Installing build dependencies: started
        Installing build dependencies: finished with status 'done'
        Getting requirements to build wheel: started
        Getting requirements to build wheel: finished with status 'done'
        Preparing metadata (pyproject.toml): started
        Preparing metadata (pyproject.toml): finished with status 'done'
      Building wheels for collected packages: maturin
        Building wheel for maturin (pyproject.toml): started
        Building wheel for maturin (pyproject.toml): finished with status 'error'
        error: subprocess-exited-with-error

        × Building wheel for maturin (pyproject.toml) did not run successfully.
        │ exit code: 1
        ╰─> [35 lines of output]
            /data/data/com.termux/files/usr/tmp/pip-build-env-yo2nrytm/overlay/lib/python3.11/site-packages/setuptools/config/_apply_pyprojecttoml.py:83: SetuptoolsWarning: `install_requires` overwritten in `pyproject.toml` (dependencies)
              corresp(dist, value, root_dir)
            running bdist_wheel
            running build
            running build_py
            creating build
            creating build/lib.linux-aarch64-cpython-311
            creating build/lib.linux-aarch64-cpython-311/maturin
            copying maturin/__init__.py -> build/lib.linux-aarch64-cpython-311/maturin
            copying maturin/__main__.py -> build/lib.linux-aarch64-cpython-311/maturin
            copying maturin/import_hook.py -> build/lib.linux-aarch64-cpython-311/maturin
            running egg_info
            creating maturin.egg-info
            writing maturin.egg-info/PKG-INFO
            writing dependency_links to maturin.egg-info/dependency_links.txt
            writing requirements to maturin.egg-info/requires.txt
            writing top-level names to maturin.egg-info/top_level.txt
            writing manifest file 'maturin.egg-info/SOURCES.txt'
            reading manifest file 'maturin.egg-info/SOURCES.txt'
            reading manifest template 'MANIFEST.in'
            warning: no files found matching '*.json' under directory 'src/python_interpreter'
            writing manifest file 'maturin.egg-info/SOURCES.txt'
            running build_ext
            running build_rust
            error: can't find Rust compiler

            If you are using an outdated pip version, it is possible a prebuilt wheel is available for this package but pip is not able to install from it. Installing from the wheel would avoid the need for a Rust compiler.

            To update pip, run:

                pip install --upgrade pip

            and then retry package installation.

            If you did intend to build this package from source, try installing a Rust compiler from your system package manager and ensure it is on the PATH during installation. Alternatively, rustup (available at https://rustup.rs) is the recommended way to download and update the Rust compiler toolchain.
            [end of output]

        note: This error originates from a subprocess, and is likely not a problem with pip.
        ERROR: Failed building wheel for maturin
      Failed to build maturin
      ERROR: Could not build wheels for maturin, which is required to install pyproject.toml-based projects
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× pip subprocess to install build dependencies did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

So, I navagated to the uv GitHub repository, and tried to install it with curl, and the shell installation script:

```zsh
❯ mkdir uv
mkdir: created directory 'uv'
❯ zz uv
/data/data/com.termux/files/home/uv
❯ l
❯ curl -LsSf https://astral.sh/uv/install.sh | sh
sh: 604: ldd: not found
System glibc version (`') is too old; using musl
ERROR: there isn't a package for aarch64-linux-android
```

I note that, there was no warning as to required dependencies in README.md, such as: lld.

So, I then checked for glibc ( aka ldd):

```zsh
❯ ldd --version
zsh: correct 'ldd' to 'ld' [nyae]? n
The program ldd is not installed. Install it by executing:
 pkg install ldd
❯ pkg install ldd
```

The lld package installed correctly. So, I re-tried curl:

```zsh
❯ curl -LsSf https://astral.sh/uv/install.sh | sh
readelf: Error: 'readlink (GNU coreutils) 9.4
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Dmitry V. Levin.': No such file
readelf: Error: 'readlink (GNU coreutils) 9.4
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Dmitry V. Levin.': No such file
System glibc version (`') is too old; using musl
ERROR: there isn't a package for aarch64-linux-android
```

I just want to successfully install uv in my host environment in Termux on Android. How should I proceed?

Any assistance is greatly apprecisted.

Thanking you in anticipation
Beauford


---

_Comment by @zanieb on 2024-03-28 04:12_

Thanks for the thorough report! We'll look into this, I'm not sure we support Android right now.

---

_Comment by @konstin on 2024-03-28 09:59_

This is a duplicate of #2408. The relevant in the error log is "error: can't find Rust compiler", after installing rust with rustup you should get the same error as in #2408

---

_Label `duplicate` added by @konstin on 2024-03-28 09:59_

---

_Closed by @zanieb on 2024-03-28 14:16_

---

_Comment by @the-one-with-raspberry on 2025-05-13 12:59_

use `pkg install uv`

---

_Comment by @shankar-narayan-k on 2025-05-23 08:42_

> use `pkg install uv`

This installs uv as a binary not as a python module and works only in CLI, **NOT** in a python script.

Any other modules that has uv as requirement still breaks, because wheel can't be built for uv.

A good example will be `prefect` module.

I have included the output of the command `pip install --verbose uv` [here](https://pastebin.com/W3suwrQQ).

##### Python Version: *3.12.10*


---

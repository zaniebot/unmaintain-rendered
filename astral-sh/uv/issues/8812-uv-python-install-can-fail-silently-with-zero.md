```yaml
number: 8812
title: uv python install can fail silently with zero exit code on extraction
type: issue
state: closed
author: stephen-hansen
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-11-04T18:39:59Z
updated_at: 2024-12-23T23:46:49Z
url: https://github.com/astral-sh/uv/issues/8812
synced_at: 2026-01-12T15:59:35Z
```

# uv python install can fail silently with zero exit code on extraction

---

_@stephen-hansen_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi,

Apologies if this is a duplicate bug report but I can't find any issue similar to this in the past week. I'm on the latest uv tag (`uv --version` == `uv 0.4.29`) on Linux and it seems that sometimes `uv python install x.y` command fails silently with no error and returns a zero exit code.

For my setup I have XDG_CACHE_HOME and XDG_DATA_HOME exported to point outside my home directory due to storage limits. Supposing I have XDG_DATA_HOME set to `/path/to/my/.local/share` then according to `uv`, `uv python dir` yields `/path/to/my/.local/share/uv/python` and `uv python dir --bin` yields `/path/to/my/.local/share/../bin`. As expected from the docs. Except I only have the former path as a valid directory on my system. The `--bin` preview path does not exist. Originally when writing this issue I suspected the missing bin path to be the issue, but not 100% sure on that. I will say I do see this issue more often than not when the bin directory does not exist (even without `--preview`), but it's not consistent.

When I run e.g. the following command I get the following verbose output:

```
uv python install 3.13 -vvv

    0.000903s DEBUG uv uv 0.4.29
    0.009835s DEBUG uv_fs Acquired lock for `/path/to/my/.local/share/uv/python`
    0.010780s DEBUG uv::commands::python::install No installation found for request `Python 3.13`
    0.010803s DEBUG uv::commands::python::install Found download `cpython-3.13.0-linux-x86_64-gnu` for request `Python 3.13`
    0.010832s DEBUG uv_client::base_client Using request timeout of 30s
 uv_python::downloads::fetch self=ManagedPythonDownload { key: PythonInstallationKey { implementation: Known(CPython), major: 3, minor: 13, patch: 0, prerelease: None, os: Os(Linux), arch: Arch(X86_64), libc: Some(Gnu), variant: Default }, url: "https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", sha256: Some("b5e74d1e16402b633c6f04519618231fc0dbae7d2f9e4b1ac17c294cc3d3d076") }, reinstall=false, download=cpython-3.13.0-linux-x86_64-gnu
    5.669179s   5s  DEBUG uv_python::downloads Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /path/to/my/.local/share/uv/python/.cache/.tmp1RQZId
    5.669197s   5s  DEBUG uv_python::downloads Extracting cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
   42.335215s DEBUG uv_fs Released lock at `/path/to/my/.local/share/uv/python/.lock`
```

But the install isn't listed under `uv python list`. Likewise no error reported in the logs above and zero exit code.

Running the command a second time sometimes works:

```
uv python install 3.13 -vvv

    0.001088s DEBUG uv uv 0.4.29
    0.006953s DEBUG uv_fs Acquired lock for `/path/to/my/.local/share/uv/python`
    0.007416s DEBUG uv::commands::python::install No installation found for request `Python 3.13`
    0.007436s DEBUG uv::commands::python::install Found download `cpython-3.13.0-linux-x86_64-gnu` for request `Python 3.13`
    0.007462s DEBUG uv_client::base_client Using request timeout of 30s
 uv_python::downloads::fetch self=ManagedPythonDownload { key: PythonInstallationKey { implementation: Known(CPython), major: 3, minor: 13, patch: 0, prerelease: None, os: Os(Linux), arch: Arch(X86_64), libc: Some(Gnu), variant: Default }, url: "https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", sha256: Some("b5e74d1e16402b633c6f04519618231fc0dbae7d2f9e4b1ac17c294cc3d3d076") }, reinstall=false, download=cpython-3.13.0-linux-x86_64-gnu
    5.561562s   5s  DEBUG uv_python::downloads Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /path/to/my/.local/share/uv/python/.cache/.tmpyfRGyd
    5.561580s   5s  DEBUG uv_python::downloads Extracting cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
   46.498652s  46s  DEBUG uv_python::downloads Moving /path/to/my/.local/share/uv/python/.cache/.tmpyfRGyd/python to /path/to/my/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu
Installed Python 3.13.0 in 46.50s
 + cpython-3.13.0-linux-x86_64-gnu
   46.503390s DEBUG uv_fs Released lock at `/path/to/my/.local/share/uv/python/.lock`
```

I think I'm hitting an error somewhere between [here](https://github.com/astral-sh/uv/blob/0.4.29/crates/uv-python/src/downloads.rs#L503) and [here](https://github.com/astral-sh/uv/blob/0.4.29/crates/uv-python/src/downloads.rs#L571). Maybe a transient extraction error due to something in my environment.

Reading the source (apologies for any mistakes here, it's been many years since I last wrote Rust), I think any errors on extraction are populated [here](https://github.com/astral-sh/uv/blob/0.4.29/crates/uv/src/commands/python/install.rs#L250). Since the one install failed, the changelog is empty. We then get to [this line](https://github.com/astral-sh/uv/blob/0.4.29/crates/uv/src/commands/python/install.rs#L326), where the changelog is empty. Since I'm not doing a default Python version install and since only one version was requested, nothing gets reported. And then we return Ok with ExitStatus::Success.

We can actually see this is happening by re-running a default Python install over and over until we get the same failure on extraction:

```
uv python install -vvv

    0.000947s DEBUG uv uv 0.4.29
    0.014577s DEBUG uv_fs Acquired lock for `/path/to/my/.local/share/uv/python`
    0.014987s DEBUG uv::commands::python::install No installation found for request `a default Python`
    0.015008s DEBUG uv::commands::python::install Found download `cpython-3.13.0-linux-x86_64-gnu` for request `a default Python`
    0.015042s DEBUG uv_client::base_client Using request timeout of 30s
 uv_python::downloads::fetch self=ManagedPythonDownload { key: PythonInstallationKey { implementation: Known(CPython), major: 3, minor: 13, patch: 0, prerelease: None, os: Os(Linux), arch: Arch(X86_64), libc: Some(Gnu), variant: Default }, url: "https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", sha256: Some("b5e74d1e16402b633c6f04519618231fc0dbae7d2f9e4b1ac17c294cc3d3d076") }, reinstall=false, download=cpython-3.13.0-linux-x86_64-gnu
    5.832126s   5s  DEBUG uv_python::downloads Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /path/to/my/.local/share/uv/python/.cache/.tmpDBessB
    5.832149s   5s  DEBUG uv_python::downloads Extracting cpython-3.13.0%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
Python is already installed. Use `uv python install <request>` to install another version.
   41.756926s DEBUG uv_fs Released lock at `/path/to/my/.local/share/uv/python/.lock`
```

Note "Python is already installed. Use `uv python install <request>` to install another version." is printed here, but we don't actually have it in `uv python list --only-installed`.

I suspect there are probably other kinds of errors that can happen in `fetch` that are more reproducible and would likewise not get reported.


---

_Label `bug` added by @zanieb on 2024-11-04 19:18_

---

_Label `great writeup` added by @zanieb on 2024-11-04 19:18_

---

_Comment by @zanieb on 2024-11-04 19:32_

Thanks for the report! I think I might have fixed this at https://github.com/astral-sh/uv/pull/8733/files#diff-8f66fe82438d145b8def0f37cef57e6c4f874bacf87367476f4e7d06d3a3480aR390 though I'll need to look closer to confirm.

---

_Comment by @kfchou on 2024-11-04 22:23_

Similar issue. If I run 
```
uv python install 3.11 3.12
```
I get the message
```
All requested versions already installed
```
But the above python versions are listed as "download available" when I run `uv python list`. So those python versions were actually not installed for me. Using the debug flag shows request failures. This is likely an issue on my end, but the "All requested versions already installed" message was confusing.

I'm running `uv` version 0.4.29

---

_Comment by @zanieb on 2024-12-23 16:19_

Ah an easy reproduction of this is

```
❯ ALL_PROXY=http://localhost:12345 uv python install 3.11 3.12
All requested versions already installed
```

We should fix this

---

_Comment by @zanieb on 2024-12-23 16:21_

Oops, those versions _were_ installed for me. Uninstalling first yields

```
❯ ALL_PROXY=http://localhost:35325 uv python install 3.11 3.12
error: Failed to install cpython-3.11.11-macos-aarch64-none
  Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.11.11%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.11.11%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: tcp connect error: Connection refused (os error 61)
  Caused by: Connection refused (os error 61)
error: Failed to install cpython-3.12.8-macos-aarch64-none
  Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.12.8%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.12.8%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: tcp connect error: Connection refused (os error 61)
  Caused by: Connection refused (os error 61)
```

I think this is fixed.

---

_Closed by @zanieb on 2024-12-23 16:21_

---

_Comment by @stephen-hansen on 2024-12-23 23:46_

Thanks all!

---

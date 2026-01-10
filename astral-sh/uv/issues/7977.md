```yaml
number: 7977
title: uv python install does not respect rc qualifiers
type: issue
state: closed
author: gaborbernat
labels:
  - bug
  - good first issue
  - uv python
assignees: []
created_at: 2024-10-07T16:22:12Z
updated_at: 2024-10-08T21:20:59Z
url: https://github.com/astral-sh/uv/issues/7977
synced_at: 2026-01-10T04:45:10Z
```

# uv python install does not respect rc qualifiers

---

_Issue opened by @gaborbernat on 2024-10-07 16:22_

It installs rc2 even though I request rc3... 😊 

```bash
❯ uv python install 3.13.0rc3 --native-tls -vv
    0.000272s DEBUG uv uv 0.4.17
    0.000755s DEBUG uv_fs Acquired lock for `/Users/bgabor8/Library/Application Support/uv/python`
Searching for Python versions matching: Python 3.13rc3
    0.001142s DEBUG uv_client::base_client Using request timeout of 30s
 uv_python::downloads::fetch self=ManagedPythonDownload { key: PythonInstallationKey { implementation: Known(CPython), major: 3, minor: 13, patch: 0, prerelease: "rc2", os: Os(Darwin), arch: Arch(Aarch64(Aarch64)), libc: None }, url: "https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.13.0rc2%2B20240909-aarch64-apple-darwin-install_only_stripped.tar.gz", sha256: Some("9e17f9fcc314a5dd489089a7502a525c4dd08af862f9cf33b52161a752f2a5b7") }, download=cpython-3.13.0rc2-macos-aarch64-none
   11.971556s  11s  DEBUG uv_python::downloads Downloading https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.13.0rc2%2B20240909-aarch64-apple-darwin-install_only_stripped.tar.gz to temporary location: /Users/bgabor8/Library/Application Support/uv/python/.cache/.tmphDfaX1
   11.971687s  11s  DEBUG uv_python::downloads Extracting cpython-3.13.0rc2%2B20240909-aarch64-apple-darwin-install_only_stripped.tar.gz
   12.968445s  12s  DEBUG uv_python::downloads Moving /Users/bgabor8/Library/Application Support/uv/python/.cache/.tmphDfaX1/python to /Users/bgabor8/Library/Application Support/uv/python/cpython-3.13.0rc2-macos-aarch64-none
Installed Python 3.13.0rc2 in 12.96s
 + cpython-3.13.0rc2-macos-aarch64-none
   12.969182s DEBUG uv_fs Released lock at `/Users/x/Library/Application Support/uv/python/.lock`
```

```
❯ uv --version
uv 0.4.17 (d2e7b40ce 2024-09-27)
```

---

_Comment by @zanieb on 2024-10-07 16:27_

We're missing a check for pre-release compatibility at https://github.com/astral-sh/uv/blob/891c91dd3a0141f05adea20674f3ea7f4477c510/crates/uv-python/src/downloads.rs#L264-L266

---

_Label `bug` added by @zanieb on 2024-10-07 16:27_

---

_Label `good first issue` added by @zanieb on 2024-10-07 16:27_

---

_Label `uv python` added by @zanieb on 2024-10-07 16:27_

---

_Comment by @trag1c on 2024-10-07 16:56_

I'd like to give this a go :) Can I get assigned to this issue?

---

_Assigned to @trag1c by @konstin on 2024-10-07 16:56_

---

_Comment by @konstin on 2024-10-07 16:57_

I assigned you

---

_Closed by @zanieb on 2024-10-08 21:20_

---

_Closed by @zanieb on 2024-10-08 21:20_

---

```yaml
number: 1068
title: Add script to download Python versions
type: pull_request
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
base: main
head: zb/bootstrap
created_at: 2024-01-23T20:38:21Z
updated_at: 2024-01-25T19:55:16Z
url: https://github.com/astral-sh/uv/pull/1068
synced_at: 2026-01-12T16:04:23Z
```

# Add script to download Python versions

---

_@zanieb_

Adapts the [`find-downloads` script from Rye](https://github.com/mitsuhiko/rye/blob/main/rye/find-downloads.py) to give us a utility for fetching Python versions. 

```
❯ python scripts/bootstrap --help
usage: bootstrap [-h] [-v] [-q] {find,install,list,clean} ...

Bootstrap Puffin development.

options:
  -h, --help            show this help message and exit
  -v, --verbose         Enable debug logging
  -q, --quiet           Disable logging

commands:
  {find,install,list,clean}
    find                Find available Python downloads and store metadata
    install             Fetch and install the given Python version
    list                List all available versions for the current system
    clean               Remove all artifacts from bootstrapping
```

e.g. after installing all available versions and the recommend versions with

```
python scripts/bootstrap list | xargs -L1 -P3 python scripts/bootstrap install
cat .python-versions | xargs -L1 python3.12 scripts/bootstrap install
```

```
bootstrap
├── bin
│   ├── python -> ../versions/cpython@3.12.1/install/bin/python3
│   ├── python3 -> ../versions/cpython@3.12.1/install/bin/python3
│   ├── python3.10 -> ../versions/cpython@3.10.13/install/bin/python3
│   ├── python3.10.0 -> ../versions/cpython@3.10.0/install/bin/python3
│   ├── python3.10.11 -> ../versions/cpython@3.10.11/install/bin/python3
│   ├── python3.10.12 -> ../versions/cpython@3.10.12/install/bin/python3
│   ├── python3.10.13 -> ../versions/cpython@3.10.13/install/bin/python3
│   ├── python3.10.2 -> ../versions/cpython@3.10.2/install/bin/python3
│   ├── python3.10.3 -> ../versions/cpython@3.10.3/install/bin/python3
│   ├── python3.10.4 -> ../versions/cpython@3.10.4/install/bin/python3
│   ├── python3.10.5 -> ../versions/cpython@3.10.5/install/bin/python3
│   ├── python3.10.6 -> ../versions/cpython@3.10.6/install/bin/python3
│   ├── python3.10.7 -> ../versions/cpython@3.10.7/install/bin/python3
│   ├── python3.10.8 -> ../versions/cpython@3.10.8/install/bin/python3
│   ├── python3.10.9 -> ../versions/cpython@3.10.9/install/bin/python3
│   ├── python3.11.1 -> ../versions/cpython@3.11.1/install/bin/python3
│   ├── python3.11.3 -> ../versions/cpython@3.11.3/install/bin/python3
│   ├── python3.11.4 -> ../versions/cpython@3.11.4/install/bin/python3
│   ├── python3.11.5 -> ../versions/cpython@3.11.5/install/bin/python3
│   ├── python3.11.6 -> ../versions/cpython@3.11.6/install/bin/python3
│   ├── python3.11.7 -> ../versions/cpython@3.11.7/install/bin/python3
│   ├── python3.12 -> ../versions/cpython@3.12.1/install/bin/python3
│   ├── python3.12.0 -> ../versions/cpython@3.12.0/install/bin/python3
│   ├── python3.12.1 -> ../versions/cpython@3.12.1/install/bin/python3
│   ├── python3.8 -> ../versions/cpython@3.8.18/install/bin/python3
│   ├── python3.8.12 -> ../versions/cpython@3.8.12/install/bin/python3
│   ├── python3.8.13 -> ../versions/cpython@3.8.13/install/bin/python3
│   ├── python3.8.14 -> ../versions/cpython@3.8.14/install/bin/python3
│   ├── python3.8.15 -> ../versions/cpython@3.8.15/install/bin/python3
│   ├── python3.8.16 -> ../versions/cpython@3.8.16/install/bin/python3
│   ├── python3.8.17 -> ../versions/cpython@3.8.17/install/bin/python3
│   ├── python3.8.18 -> ../versions/cpython@3.8.18/install/bin/python3
│   ├── python3.9 -> ../versions/cpython@3.9.18/install/bin/python3
│   ├── python3.9.10 -> ../versions/cpython@3.9.10/install/bin/python3
│   ├── python3.9.11 -> ../versions/cpython@3.9.11/install/bin/python3
│   ├── python3.9.12 -> ../versions/cpython@3.9.12/install/bin/python3
│   ├── python3.9.13 -> ../versions/cpython@3.9.13/install/bin/python3
│   ├── python3.9.14 -> ../versions/cpython@3.9.14/install/bin/python3
│   ├── python3.9.15 -> ../versions/cpython@3.9.15/install/bin/python3
│   ├── python3.9.16 -> ../versions/cpython@3.9.16/install/bin/python3
│   ├── python3.9.17 -> ../versions/cpython@3.9.17/install/bin/python3
│   ├── python3.9.18 -> ../versions/cpython@3.9.18/install/bin/python3
│   ├── python3.9.2 -> ../versions/cpython@3.9.2/install/bin/python3
│   ├── python3.9.3 -> ../versions/cpython@3.9.3/install/bin/python3
│   ├── python3.9.4 -> ../versions/cpython@3.9.4/install/bin/python3
│   ├── python3.9.5 -> ../versions/cpython@3.9.5/install/bin/python3
│   ├── python3.9.6 -> ../versions/cpython@3.9.6/install/bin/python3
│   └── python3.9.7 -> ../versions/cpython@3.9.7/install/bin/python3
├── self
│   └── versions.json
└── versions
    ├── cpython@3.10.0
    ├── cpython@3.10.11
    ├── cpython@3.10.12
    ├── cpython@3.10.13
    ├── cpython@3.10.2
    ├── cpython@3.10.3
    ├── cpython@3.10.4
    ├── cpython@3.10.5
    ├── cpython@3.10.6
    ├── cpython@3.10.7
    ├── cpython@3.10.8
    ├── cpython@3.10.9
    ├── cpython@3.11.1
    ├── cpython@3.11.3
    ├── cpython@3.11.4
    ├── cpython@3.11.5
    ├── cpython@3.11.6
    ├── cpython@3.11.7
    ├── cpython@3.12.0
    ├── cpython@3.12.1
    ├── cpython@3.8.12
    ├── cpython@3.8.13
    ├── cpython@3.8.14
    ├── cpython@3.8.15
    ├── cpython@3.8.16
    ├── cpython@3.8.17
    ├── cpython@3.8.18
    ├── cpython@3.9.10
    ├── cpython@3.9.11
    ├── cpython@3.9.12
    ├── cpython@3.9.13
    ├── cpython@3.9.14
    ├── cpython@3.9.15
    ├── cpython@3.9.16
    ├── cpython@3.9.17
    ├── cpython@3.9.18
    ├── cpython@3.9.2
    ├── cpython@3.9.3
    ├── cpython@3.9.4
    ├── cpython@3.9.5
    ├── cpython@3.9.6
    └── cpython@3.9.7
```

---

_Label `internal` added by @zanieb on 2024-01-23 20:48_

---

_Label `testing` added by @zanieb on 2024-01-23 20:48_

---

_Comment by @zanieb on 2024-01-23 21:19_

It'd be nice for this to have no requirement on Python to start but then I'd need to write it in Rust or as a shell script which seems like too much work for now.

---

_Closed by @zanieb on 2024-01-25 19:55_

---

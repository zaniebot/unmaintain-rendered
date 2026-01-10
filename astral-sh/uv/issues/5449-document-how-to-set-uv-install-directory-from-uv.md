```yaml
number: 5449
title: "Document how to set `uv` install directory from `uv-installer.sh`"
type: issue
state: closed
author: SamuelMarks
labels:
  - documentation
  - releases
assignees: []
created_at: 2024-07-25T15:32:00Z
updated_at: 2024-10-17T01:06:25Z
url: https://github.com/astral-sh/uv/issues/5449
synced_at: 2026-01-10T04:45:09Z
```

# Document how to set `uv` install directory from `uv-installer.sh`

---

_Issue opened by @SamuelMarks on 2024-07-25 15:32_

Was about to add a feature request then realised it already works:

``` 
$ curl --proto '=https' --tlsv1.2 -OLsSf https://github.com/astral-sh/uv/releases/download/0.2.29/uv-installer.sh
$ env -i CARGO_HOME=/tmp/foo/cargo HOME=/tmp/foo/home /usr/bin/bash uv-installer.sh --no-modify-path
downloading uv 0.2.29 x86_64-unknown-linux-gnu
installing to /tmp/foo/cargo/bin
  uv
  uvx
everything's installed!
$ tree -a /tmp/foo
/tmp/foo
├── cargo
│   └── bin
│       ├── uv
│       └── uvx
└── home
    └── .config
        └── uv
            └── uv-receipt.json

6 directories, 3 files
```

Might be helpful to document this for others.

---

_Comment by @zanieb on 2024-07-25 16:18_

Thanks! This would go in https://github.com/astral-sh/uv/blob/main/docs/installation.md#standalone-installer if someone is interested in contributing.

---

_Label `documentation` added by @zanieb on 2024-07-25 16:18_

---

_Label `release` added by @zanieb on 2024-08-23 18:16_

---

_Closed by @zanieb on 2024-10-17 01:06_

---

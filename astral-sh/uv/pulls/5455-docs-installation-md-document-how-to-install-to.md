```yaml
number: 5455
title: "[docs/installation.md] Document how to install to custom locations"
type: pull_request
state: closed
author: SamuelMarks
labels:
  - releases
assignees: []
base: main
head: document-install
created_at: 2024-07-25T17:56:25Z
updated_at: 2024-09-17T04:07:07Z
url: https://github.com/astral-sh/uv/pull/5455
synced_at: 2026-01-10T12:53:32Z
```

# [docs/installation.md] Document how to install to custom locations

---

_Pull request opened by @SamuelMarks on 2024-07-25 17:56_

## Summary

Closes #5449

## Test Plan

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

---

_Assigned to @zanieb by @zanieb on 2024-07-25 18:01_

---

_Comment by @zanieb on 2024-07-30 14:36_

Sorry I was on vacation — I'll get to this soon!

---

_Comment by @zanieb on 2024-08-06 17:08_

I'm a little worried this is too complicated. I wonder if we should add a custom environment variable to make this possible? Is there a reason the download is separate from the install invocation?

---

_Label `release` added by @zanieb on 2024-08-23 18:16_

---

_Comment by @zanieb on 2024-09-17 04:07_

This is documented in a simpler way at https://docs.astral.sh/uv/getting-started/installation/#configuring-installation

See https://github.com/astral-sh/uv/pull/7107

Thanks for contributing though this was really helpful while we waited for upstream to add simpler support

---

_Closed by @zanieb on 2024-09-17 04:07_

---

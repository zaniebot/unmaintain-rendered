```yaml
number: 17383
title: Allow override of UV_VERSION in uv-installer.sh
type: issue
state: closed
author: tsaavik
labels:
  - enhancement
assignees: []
created_at: 2026-01-09T16:54:05Z
updated_at: 2026-01-09T17:09:58Z
url: https://github.com/astral-sh/uv/issues/17383
synced_at: 2026-01-10T03:11:36Z
```

# Allow override of UV_VERSION in uv-installer.sh

---

_Issue opened by @tsaavik on 2026-01-09 16:54_

### Summary

This feature request is for the uv-installer.sh hosted at https://astral.sh/uv/install.sh

It would be nice to allow users to override the version to be installed via the environment.
For example
`export UV_VERSION=0.9.22`

The change to the script is simple, posix compatible and non-breaking

```
-APP_VERSION="0.9.22"
+APP_VERSION=${UV_VERSION:=0.9.22}
```

I'm happy to post a PR, but I can not find the uv-installer.sh anywhere...

### Example

`./uv-install.sh`
Latest version is installed, potentially causing user issues

After change

```
export UV_VERSION=0.9.20
./uv-install.sh
```
 Fixed version 0.9.20 is installed

---

_Label `enhancement` added by @tsaavik on 2026-01-09 16:54_

---

_Comment by @zanieb on 2026-01-09 17:05_

The installer is tied to a specific uv version because sometimes there are updates to the installer. Why not use one of our versioned URLs instead?

---

_Comment by @tsaavik on 2026-01-09 17:09_

Ah, I missed that in the documentation. Awesome, thank you!

For anyone else looking for the format:
`https://astral.sh/uv/0.9.22/install.sh`

---

_Closed by @tsaavik on 2026-01-09 17:09_

---

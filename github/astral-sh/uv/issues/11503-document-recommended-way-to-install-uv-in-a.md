---
number: 11503
title: Document recommended way to install uv in a Dockerfile
type: issue
state: closed
author: snickell
labels: []
assignees: []
created_at: 2025-02-14T07:39:13Z
updated_at: 2025-05-19T12:22:51Z
url: https://github.com/astral-sh/uv/issues/11503
synced_at: 2026-01-07T13:12:18-06:00
---

# Document recommended way to install uv in a Dockerfile

---

_Issue opened by @snickell on 2025-02-14 07:39_

### Summary

Its great when projects document the recommended, and thus stable and not-changing way to do "install project in Dockerfile", maybe UV could do so?

In many cases, particularly when building Dockerfiles, you'd like to do a "system install", typically to /usr/local. It'd be great to document the recommended way to install "system-wide" in a docker image, this is something people will often be doing, and having to figure out the best way will result in unnecessary diversity of approaches.

### Example

I use this in my Dockerfiles:
```
curl -LsSf https://astral.sh/uv/0.5.18/install.sh | XDG_BIN_HOME=/usr/local/bin UV_NO_MODIFY_PATH=1 sh
```

UV's install.sh script is oriented toward installing as the current user, and thus:
1. Defaults its install location to XDG_BIN_HOME, which is usually in $HOME
2. Modifies the $PATH by messing with shell initialization scripts in $HOME

Another alternative approach people might use in Dockerfiles is to download static binaries directly from https://github.com/astral-sh/uv/releases/download/ and copy them directly into /usr/local. This is actually non-trivial in a Dockerfile context if you'd like have the Dockerfile be cross-arch, notably these days supporting both x86_64 and arm64. There are multiple competing ways of describing arch strings.

---

_Label `enhancement` added by @snickell on 2025-02-14 07:39_

---

_Renamed from "Document how to do a "system install" of uv, for Dockerfiles in particular" to "Document how "system install" uv in a Dockerfile" by @snickell on 2025-02-14 07:41_

---

_Renamed from "Document how "system install" uv in a Dockerfile" to "Document recommended way to install uv in a Dockerfile" by @snickell on 2025-02-14 07:41_

---

_Comment by @zanieb on 2025-02-14 13:16_

This is thoroughly documented in https://docs.astral.sh/uv/guides/integration/docker/

Is something missing there?

---

_Comment by @zanieb on 2025-02-14 13:17_

This is also linked from our installation page https://docs.astral.sh/uv/getting-started/installation/#docker

---

_Comment by @porn on 2025-05-19 11:55_

Usage in Dockerfiles is documented well.

However, the solution in the issue description is perfect, e.g., for the [packer](https://www.packer.io).

---

_Label `enhancement` removed by @konstin on 2025-05-19 12:20_

---

_Comment by @konstin on 2025-05-19 12:22_

There is also `UV_INSTALL_DIR` for the install script, otherwise I think this has been answered.

---

_Closed by @konstin on 2025-05-19 12:22_

---

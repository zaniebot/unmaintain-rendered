```yaml
number: 4335
title: Read Python version files during toolchain installs
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-install-python-version
created_at: 2024-06-14T22:54:21Z
updated_at: 2024-06-17T18:37:54Z
url: https://github.com/astral-sh/uv/pull/4335
synced_at: 2026-01-10T13:54:02Z
```

# Read Python version files during toolchain installs

---

_Pull request opened by @zanieb on 2024-06-14 22:54_

A bare `uv toolchain install` invocation now reads default requests from Python version files in the working directory. In order, a bare invocation means:

- requests from `.python-versions`
- a single request from`.python-version`
- any installed managed toolchain
- the latest managed toolchain download

This replaces all the functionality of `cargo dev fetch-python`, which we drop in #4337 

---

_Label `preview` added by @zanieb on 2024-06-14 22:54_

---

_Label `enhancement` added by @zanieb on 2024-06-15 00:57_

---

_Marked ready for review by @zanieb on 2024-06-15 01:01_

---

_@zanieb reviewed on 2024-06-15 14:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/install.rs`:108 on 2024-06-15 14:41_

Can't use `targets` here it's wrong when it's read from a file

---

_Comment by @konstin on 2024-06-17 08:56_

I think we should also allow defining them in the toml configuration, something like `tool.uv.python-versions`

---

_@konstin approved on 2024-06-17 08:56_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-17 16:58_

---

_@BurntSushi approved on 2024-06-17 17:33_

---

_@ibraheemdev approved on 2024-06-17 18:15_

---

_Comment by @zanieb on 2024-06-17 18:28_

> I think we should also allow defining them in the toml configuration, something like `tool.uv.python-versions`

Tracking in #4359 

---

_Merged by @zanieb on 2024-06-17 18:37_

---

_Closed by @zanieb on 2024-06-17 18:37_

---

_Branch deleted on 2024-06-17 18:37_

---

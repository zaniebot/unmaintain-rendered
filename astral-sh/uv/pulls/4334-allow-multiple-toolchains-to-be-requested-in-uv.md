```yaml
number: 4334
title: "Allow multiple toolchains to be requested in `uv toolchain install`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-install-many
created_at: 2024-06-14T22:33:41Z
updated_at: 2024-06-17T18:24:12Z
url: https://github.com/astral-sh/uv/pull/4334
synced_at: 2026-01-12T16:06:09Z
```

# Allow multiple toolchains to be requested in `uv toolchain install`

---

_@zanieb_

Allows installation of multiple toolchains in a single invocation because I don't want to be limited to one! Most of the implementation for concurrent downloads ported from `cargo dev fetch-python`.

---

_Label `preview` added by @zanieb on 2024-06-14 22:35_

---

_Review comment by @konstin on `crates/uv/src/commands/toolchain/install.rs`:97 on 2024-06-17 08:35_

Nit: Can we do this with a single collect?

---

_Review comment by @konstin on `crates/uv/src/commands/toolchain/install.rs`:115 on 2024-06-17 08:37_

Should we instrument `PythonDownload::fetch` instead?

---

_@konstin approved on 2024-06-17 08:38_

---

_@zanieb reviewed on 2024-06-17 16:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/install.rs`:115 on 2024-06-17 16:23_

I'm down for that, this was just copied from `cargo dev fetch-python`

---

_Review requested from @BurntSushi by @zanieb on 2024-06-17 16:58_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/toolchain/install.rs`:70 on 2024-06-17 17:20_

I think the idea here is that if this message is printed, then I think it's guaranteed that some other "Found installed toolchain ..." message is printed above it, right?

---

_Review comment by @BurntSushi on `crates/uv/src/commands/toolchain/install.rs`:70 on 2024-06-17 17:21_

Also, should this say, "all requested toolchains are already installed"? Instead of just saying only a single toolchain is already installed?

---

_@BurntSushi approved on 2024-06-17 17:21_

---

_@zanieb reviewed on 2024-06-17 18:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/install.rs`:70 on 2024-06-17 18:04_

Yeah that's the idea. The output is actually pretty reasonable with both messages, one is listing discovered toolchains and the other summarizes the result of the whole operation.

We only say this when the targets are empty, which means we're looking for _any_ managed toolchain. This is a special case added in https://github.com/astral-sh/uv/pull/4248

---

_Merged by @zanieb on 2024-06-17 18:24_

---

_Closed by @zanieb on 2024-06-17 18:24_

---

_Branch deleted on 2024-06-17 18:24_

---

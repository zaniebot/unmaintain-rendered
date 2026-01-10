```yaml
number: 237
title: Fallback to copy if hardlinking failed
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: copy-fallback
created_at: 2023-10-30T19:08:02Z
updated_at: 2023-11-01T12:21:38Z
url: https://github.com/astral-sh/uv/pull/237
synced_at: 2026-01-10T15:50:28Z
```

# Fallback to copy if hardlinking failed

---

_Pull request opened by @konstin on 2023-10-30 19:08_

_No description provided._

---

_@charliermarsh reviewed on 2023-10-30 19:09_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/installer.rs`:52 on 2023-10-30 19:09_

Nit: format as `Failed to install: {} @ {}` for consistency with other error messages.

---

_Merged by @konstin on 2023-10-30 19:10_

---

_Closed by @konstin on 2023-10-30 19:10_

---

_Branch deleted on 2023-10-30 19:10_

---

_@charliermarsh reviewed on 2023-10-30 19:10_

Do you know why it's failing? What's the root cause?

---

_@charliermarsh reviewed on 2023-10-30 19:11_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:389 on 2023-10-30 19:11_

This seems like it could be problematic, since we can end up with a mixed link -- where some files are symlinked and others are copied. Should we do this instead a layer higher? Try to hardlink; if it fails, remove, then do a full copy?

---

_Comment by @konstin on 2023-10-30 19:20_

> Do you know why it's failing? What's the root cause?

I'm using a cache on the host system and building inside a docker container, which are technically two different file systems, and you can't hard link between two different file systems.

---

_@konstin reviewed on 2023-10-30 19:21_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/linker.rs`:389 on 2023-10-30 19:21_

What about we track whether it's the first file, if so we switch strategies, otherwise we propagate the error?

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:389 on 2023-10-30 19:22_

Sure. If it’s not the first file, then we should probably hard error rather than falling back.


---

_@charliermarsh reviewed on 2023-10-30 19:22_

---

_Comment by @charliermarsh on 2023-10-30 19:22_

What does pnpm do there? Is there any way for us to detect this before trying to hardlink?

---

_Comment by @konstin on 2023-11-01 12:21_

pnpm always hardlinks or fails: https://github.com/pnpm/pnpm/blob/fc858f6a51099eabe92abd9a2b22f292726d145d/fs/hard-link-dir/src/index.ts#L55-L81

> Is there any way for us to detect this before trying to hardlink?

I don't think so, i'd be surprised if os/fs have apis to check this. Only relevant think i found quickly googling suggests the same: https://stackoverflow.com/a/5011016/3549270

---

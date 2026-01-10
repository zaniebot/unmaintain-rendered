```yaml
number: 4865
title: Add some decoration to tool CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/tool-cli
created_at: 2024-07-07T20:16:25Z
updated_at: 2024-07-08T13:21:07Z
url: https://github.com/astral-sh/uv/pull/4865
synced_at: 2026-01-10T13:42:52Z
```

# Add some decoration to tool CLI

---

_Pull request opened by @charliermarsh on 2024-07-07 20:16_

Mostly small things. I added entrypoint counts and bolded the executable names.

Closes https://github.com/astral-sh/uv/issues/4815.

---

_Review requested from @zanieb by @charliermarsh on 2024-07-07 20:16_

---

_Label `cli` added by @charliermarsh on 2024-07-07 20:16_

---

_Label `preview` added by @charliermarsh on 2024-07-07 20:16_

---

_@charliermarsh reviewed on 2024-07-07 20:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:16_

Are you opposed to using "executable" for user-facing copy?

---

_Comment by @charliermarsh on 2024-07-07 20:16_

(I will update fixtures once approved, just a bit tedious.)

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:17_

It feels a little weird, I don't have any great alternatives though. "tool entry points"? eh.

---

_@zanieb reviewed on 2024-07-07 20:17_

---

_@zanieb reviewed on 2024-07-07 20:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:269 on 2024-07-07 20:18_

These are always two words in the official documentation

https://packaging.python.org/en/latest/specifications/entry-points/

---

_@zanieb reviewed on 2024-07-07 20:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:269 on 2024-07-07 20:19_

(I like how entrypoints looks too, but I think we should be consistent. I've been following the official docs here)

---

_@zanieb reviewed on 2024-07-07 20:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:19_

We use tool entry points everywhere else in this output, right?

---

_@charliermarsh reviewed on 2024-07-07 20:20_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:269 on 2024-07-07 20:20_

Oh strange, thank you, I can change it.

---

_@charliermarsh reviewed on 2024-07-07 20:20_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:20_

But entry point is not as general... We also support data scripts which aren't entry points. I feel like entry point is sort of an implementation detail.

---

_@zanieb reviewed on 2024-07-07 20:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:22_

Fair. Why not just "commands" then?

---

_@zanieb reviewed on 2024-07-07 20:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:22_

I don't have strong feelings here.

---

_@zanieb reviewed on 2024-07-07 20:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:24_

Cargo uses `executable`, e.g.

```
   Replacing /Users/zb/.cargo/bin/cargo-nextest
    Replaced package `cargo-nextest v0.9.70` with `cargo-nextest v0.9.72` (executable `cargo-nextest`)
```

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/uninstall.rs`:74 on 2024-07-07 20:25_

Ok, I'm gonna run with executable. I can change other user-facing copy too for consistency.

---

_@charliermarsh reviewed on 2024-07-07 20:25_

---

_Merged by @charliermarsh on 2024-07-08 13:21_

---

_Closed by @charliermarsh on 2024-07-08 13:21_

---

_Branch deleted on 2024-07-08 13:21_

---

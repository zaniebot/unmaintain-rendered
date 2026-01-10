```yaml
number: 15881
title: fix misleading debug message in uv sync
type: pull_request
state: merged
author: harshithvh
labels:
  - tracing
assignees: []
merged: true
base: main
head: hvh/sync-debug-message
created_at: 2025-09-15T19:59:06Z
updated_at: 2025-09-16T18:39:09Z
url: https://github.com/astral-sh/uv/pull/15881
synced_at: 2026-01-10T06:36:15Z
```

# fix misleading debug message in uv sync

---

_Pull request opened by @harshithvh on 2025-09-15 19:59_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Added `RemovableReason` enum to track removal context
- Updated `OnExisting::Remove` to include source information
- Modified debug message to show appropriate context
- Updated all call sites to specify correct removal source

fixes: #14734


---

_@zanieb reviewed on 2025-09-15 20:18_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:127 on 2025-09-15 20:18_

It seems a little weird that there's a `None` variant here. Isn't this due to like, it being a temporary environment?

---

_@harshithvh reviewed on 2025-09-15 20:25_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:127 on 2025-09-15 20:25_

hm, would it make more sense to have explicit variants? like:
`TemporaryEnvironment` for build/cache/script temp dirs
`ToolEnvironment` for tool-specific environments
keep `SyncDefault` for sync ops
keep `ClearFlag` for the --clear flag

---

_@zanieb reviewed on 2025-09-15 20:34_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:127 on 2025-09-15 20:34_

Maybe? I think I'd probably tie it to the category, rather than the specific instance

- `TemporaryEnvironment`
- `ManagedEnvironment`
- `Requested`

or something?

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:662 on 2025-09-15 20:35_

I might call this enum something like, `RemovableReason` instead? since it's scoped to the `Remove` variant not all `OnExisting` variants?

---

_@zanieb reviewed on 2025-09-15 20:35_

---

_@harshithvh reviewed on 2025-09-15 20:38_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:662 on 2025-09-15 20:38_

yeh, this makes sense to me

---

_@harshithvh reviewed on 2025-09-15 20:40_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:127 on 2025-09-15 20:40_

sounds good to me!

---

_@zanieb reviewed on 2025-09-16 12:50_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 12:50_

I was imagining this being superseded by `Requested`

---

_@zanieb reviewed on 2025-09-16 12:50_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:670 on 2025-09-16 12:50_

Can we remove the `Option` here?

---

_@harshithvh reviewed on 2025-09-16 13:56_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 13:56_

We could retain this, the debug message "due to --clear" is much more informative than "as requested"
wdyt?

---

_@zanieb reviewed on 2025-09-16 14:08_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 14:08_

I think we could still say "due to --clear" unless there's another way it can be requested? (this does ignore `UV_VENV_CLEAR`, but... not sure what to do about that)

---

_@harshithvh reviewed on 2025-09-16 14:25_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 14:25_

Hm, I see two ways to trigger "clear" behavior
1. CLI flag: --clear
2. UV_VENV_CLEAR

I have few options here
1. Keep the current `ClearFlag` variant, accept that "due to --clear" might be slightly misleading for `UV_VENV_CLEAR`, minor trade-off
2. Merge into `Requested`
3. more granular: add variant `ClearEnvVar` triggered by `UV_VENV_CLEAR`

---

_@zanieb reviewed on 2025-09-16 14:36_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 14:36_

I would probably use `UserRequest` as a single variant and just use `--clear` as the only language. We don't have support for distinguishing between cases where an environment variable is used yet. We might add that in the future, but it's a more holistic change.

---

_@harshithvh reviewed on 2025-09-16 14:51_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 14:51_

sure, `UserRequest` should look good now

---

_Comment by @harshithvh on 2025-09-16 15:37_

@zanieb all looks good to me 
Mind giving a final review

---

_@zanieb reviewed on 2025-09-16 17:11_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 17:11_

We should remove this now

---

_@harshithvh reviewed on 2025-09-16 18:05_

---

_Review comment by @harshithvh on `crates/uv-virtualenv/src/virtualenv.rs`:652 on 2025-09-16 18:05_

done, thanks for pointing this

---

_@zanieb approved on 2025-09-16 18:32_

---

_Label `tracing` added by @zanieb on 2025-09-16 18:32_

---

_Merged by @zanieb on 2025-09-16 18:39_

---

_Closed by @zanieb on 2025-09-16 18:39_

---

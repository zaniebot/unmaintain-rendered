```yaml
number: 4416
title: Add internal options for managing toolchain discovery preferences
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/toolchain-preference
created_at: 2024-06-19T16:55:41Z
updated_at: 2024-06-20T13:57:06Z
url: https://github.com/astral-sh/uv/pull/4416
synced_at: 2026-01-12T16:06:13Z
```

# Add internal options for managing toolchain discovery preferences

---

_@zanieb_

Adds support for the toolchain discovery preferences outlined in https://github.com/astral-sh/uv/issues/4198 but we don't expose this to users yet, I'll do that next to make it easier to review.

I've made some refactors in the toolchain discovery implementation to enable this behavior and move us towards clearer abstractions. There's still remaining work here, but I'd prefer tackle things in follow-ups instead of expanding this pull request. I plan on opening a couple before merging this.

I'd like to shift the public toolchain API to focus on discovering either an **environment** or a **toolchain**. The first would be used by commands that operate on an environment, while the latter would be used by commands that just need an interpreter to create environments. I haven't changed this here, but some of the refactors are in preparation for supporting this idea.

In brief:

- We now allow different ordering of installed toolchain discovery based on a `ToolchainPreference` type. This is the type we will expose to users.
- `SystemPython` was changed into an `EnvironmentPreference` which is used to determine if we should prefer virtual or system Python environments. 
- We drop the whole `ToolchainSources` selection concept, it was confusing and the error messages from it were awkward. Most of the functionality is now captured by the preference enums, but you can't do things like "only find a toolchain from the parent interpreter" as easily anymore.

---

_Label `internal` added by @zanieb on 2024-06-19 16:55_

---

_Renamed from "Add setting for managing toolchain discovery preferences" to "Add internal options for managing toolchain discovery preferences" by @zanieb on 2024-06-19 16:56_

---

_Converted to draft by @zanieb on 2024-06-19 16:56_

---

_@zanieb reviewed on 2024-06-19 17:20_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:277 on 2024-06-19 17:20_

This is less clear but will address separately.

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:120 on 2024-06-19 17:21_

Interesting regression... will investigate this.

---

_@zanieb reviewed on 2024-06-19 17:21_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/toolchain.rs`:49 on 2024-06-19 17:23_

This is a big plus. We don't need to do multiple discovery attempts to get a different ordering. We just say what we want.

---

_@zanieb reviewed on 2024-06-19 17:23_

---

_@zanieb reviewed on 2024-06-19 17:32_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:120 on 2024-06-19 17:32_

These tests are not properly isolated from managed toolchains because they aren't following our `TestContext` pattern :sigh: 

---

_@zanieb reviewed on 2024-06-19 17:34_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:49 on 2024-06-19 17:34_

Addresses https://github.com/astral-sh/uv/pull/4416#discussion_r1646535200

---

_Marked ready for review by @zanieb on 2024-06-19 17:35_

---

_Review requested from @konstin by @zanieb on 2024-06-19 17:36_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-19 17:36_

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/toolchain.rs`:49 on 2024-06-20 12:40_

I like it.

---

_@BurntSushi approved on 2024-06-20 12:41_

Nice!

Was this the refactor you were talking about in DMs? I know you mentioned it being exploratory, but one thing that I find can help here is to put things like type/function renames into one commit separate from the other changes. The renames can touch a lot of code and add a lot of noise to the diff. When split out, the "more interesting" changes can be seen in a different commit.

(I know this isn't always feasible, especially in a more exploratory refactor. But I thought it might be valuable to throw it out there.)

---

_Comment by @zanieb on 2024-06-20 13:17_

Yeah this is a part of what I was mentioning. A lot of the exploration is figuring out how to add this setting while moving the API towards something that's easy to use correctly. I generally do try to split out renames, but yeah I didn't do a great job of that here :)

---

_Merged by @zanieb on 2024-06-20 13:57_

---

_Closed by @zanieb on 2024-06-20 13:57_

---

_Branch deleted on 2024-06-20 13:57_

---

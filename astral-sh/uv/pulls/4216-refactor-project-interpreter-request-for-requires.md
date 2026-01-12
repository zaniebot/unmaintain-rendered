```yaml
number: 4216
title: "Refactor project interpreter request for `requires-python` specifiers"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/fix-project-requires-python
created_at: 2024-06-10T20:11:21Z
updated_at: 2024-06-11T01:53:54Z
url: https://github.com/astral-sh/uv/pull/4216
synced_at: 2026-01-12T16:06:06Z
```

# Refactor project interpreter request for `requires-python` specifiers

---

_@zanieb_

Refactor following #4214 to avoid parsing the specifiers again

---

_Label `preview` added by @zanieb on 2024-06-10 20:11_

---

_Converted to draft by @zanieb on 2024-06-10 20:39_

---

_Marked ready for review by @zanieb on 2024-06-10 23:21_

---

_Renamed from "Fix project interpreter support for `requires-python` specifiers" to "Refactor project interpreter request for `requires-python` specifiers" by @zanieb on 2024-06-10 23:21_

---

_@zanieb reviewed on 2024-06-10 23:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:140 on 2024-06-10 23:22_

Guess I might want to make `find` take a `Option<ToolchainRequest>` as well.

---

_@charliermarsh reviewed on 2024-06-11 01:25_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:140 on 2024-06-11 01:25_

Personally find it a little easier to read as:

```rust
let interpreter = python
    .map(ToolchainRequest::parse)
    .or(requires_python
        .map(|specifiers| ToolchainRequest::Version(VersionRequest::Range(specifiers.clone()))))
    .map_or_else(
        || {
            Toolchain::find(
                None,
                // Otherwise we'll try to use the venv we just deleted.
                SystemPython::Required,
                preview,
                cache,
            )
        },
        |request| {
            Toolchain::find_requested(
                &request,
                // Otherwise we'll try to use the venv we just deleted.
                SystemPython::Required,
                preview,
                cache,
            )
        },
    )?
    .into_interpreter();
```

But totally subjective.

---

_@charliermarsh approved on 2024-06-11 01:25_

---

_@zanieb reviewed on 2024-06-11 01:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:140 on 2024-06-11 01:31_

Oooh that's really nice haha but... I think I still want to refactor `Toolchain::find` to just not take strings anymore so I'll probably just merge and change it later.

---

_Merged by @zanieb on 2024-06-11 01:32_

---

_Closed by @zanieb on 2024-06-11 01:32_

---

_Branch deleted on 2024-06-11 01:32_

---

_@charliermarsh reviewed on 2024-06-11 01:41_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:140 on 2024-06-11 01:41_

Wow...

---

_@zanieb reviewed on 2024-06-11 01:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:140 on 2024-06-11 01:53_

I want it in the release and it's after work hours! hehe

---

_@zanieb reviewed on 2024-06-11 01:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:140 on 2024-06-11 01:53_

I do really like it actually <3

---

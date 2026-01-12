```yaml
number: 14322
title: "Add missing validations for disallowed `uv.toml` fields"
type: pull_request
state: merged
author: Gankra
labels:
  - error messages
  - breaking
assignees: []
merged: true
base: release/080
head: gankra/check-fields
created_at: 2025-06-27T18:26:38Z
updated_at: 2025-07-25T19:45:43Z
url: https://github.com/astral-sh/uv/pull/14322
synced_at: 2026-01-12T16:11:09Z
```

# Add missing validations for disallowed `uv.toml` fields

---

_@Gankra_

We weren't following our usual "destructure all the options" pattern in this function, and several "this isn't actually read from uv.toml" fields slipped through the cracks over time since folks forgot it existed.

Fixes part of #14308, although we could still try to make the warning in FilesystemOptions more accurate?

You could argue this is a breaking change, but I think it ultimately isn't really, because we were already silently ignoring these fields. Now we properly error.

---

_Label `error messages` added by @Gankra on 2025-06-27 18:26_

---

_Comment by @zanieb on 2025-06-27 18:36_

We should probably just roll this into 0.8 since it's coming up

---

_Label `breaking` added by @Gankra on 2025-06-27 18:41_

---

_Added to milestone `v0.8.0` by @Gankra on 2025-06-27 18:41_

---

_Comment by @Gankra on 2025-06-27 18:41_

Works for me

---

_Comment by @zanieb on 2025-06-27 18:46_

Can you rebase?

---

_Comment by @Gankra on 2025-06-27 19:09_

Rebased

---

_@zanieb approved on 2025-07-11 17:00_

---

_Merged by @zanieb on 2025-07-11 17:01_

---

_Closed by @zanieb on 2025-07-11 17:01_

---

_Branch deleted on 2025-07-11 17:01_

---

_@zanieb reviewed on 2025-07-25 19:45_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:210 on 2025-07-25 19:45_

Should these all be `pyproject.toml`-only? starting at `override_dependencies: _,`

---

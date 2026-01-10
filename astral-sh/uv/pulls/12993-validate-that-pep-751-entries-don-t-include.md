```yaml
number: 12993
title: "Validate that PEP 751 entries don't include multiple sources"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/pep-751-validate
created_at: 2025-04-21T01:31:24Z
updated_at: 2025-04-21T22:22:05Z
url: https://github.com/astral-sh/uv/pull/12993
synced_at: 2026-01-10T11:10:40Z
```

# Validate that PEP 751 entries don't include multiple sources

---

_Pull request opened by @charliermarsh on 2025-04-21 01:31_

## Summary

The spec defines these as mutually exclusive, so we now error when trying to install such a package.


---

_Label `error messages` added by @charliermarsh on 2025-04-21 01:31_

---

_Marked ready for review by @charliermarsh on 2025-04-21 01:31_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:581 on 2025-04-21 17:52_

It _seems_ like this should be composable using a single error variant and an enum for displaying the names, but... it's also fine if you think that's not worth it since these should be static. I do think if we want to handle this error downstream at some point, it'll be annoying it's not a single error variant.

---

_@zanieb reviewed on 2025-04-21 17:52_

---

_@zanieb approved on 2025-04-21 17:52_

---

_@charliermarsh reviewed on 2025-04-21 18:42_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:581 on 2025-04-21 18:42_

Yeah, I did consider this... Basically, we'd use a different representation for the `pylock.toml` types, and treat the current representations as "wire" types. So we'd do all this validation at deserialization time, and we'd never have to do this kind of thing in our own code. That's roughly what we do in our own `uv.lock` logic. But... it's kind of a lot of work, and it didn't really seem worth it honestly. There's also some benefit to having a single set of structs that exactly match the spec.


---

_Merged by @charliermarsh on 2025-04-21 22:22_

---

_Closed by @charliermarsh on 2025-04-21 22:22_

---

_Branch deleted on 2025-04-21 22:22_

---

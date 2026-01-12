```yaml
number: 1888
title: "Use `rustls-tls-native-roots` in `uv` crate"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/native-tls
created_at: 2024-02-22T22:44:26Z
updated_at: 2025-09-10T12:56:45Z
url: https://github.com/astral-sh/uv/pull/1888
synced_at: 2026-01-12T16:04:46Z
```

# Use `rustls-tls-native-roots` in `uv` crate

---

_@zanieb_

I'm confused that we have this separate specification of `reqwests`? I'm not sure this has any effect, but it seems like it should be done for correctness.

Follows #1512 

---

_Label `bug` added by @zanieb on 2024-02-22 22:44_

---

_@charliermarsh approved on 2024-02-22 23:01_

This is just for tests, right?

---

_Comment by @zanieb on 2024-02-22 23:18_

Ahhh I see it's a dev dependency that makes sense. I'm not sure we need this then? Not sure what do you think?

---

_Comment by @charliermarsh on 2024-02-22 23:56_

I think it's fine either way. Honestly we should be able to omit the feature entirely since features are additive. We should only need `blocking`...

---

_@zanieb reviewed on 2024-02-23 00:30_

---

_Review comment by @zanieb on `crates/uv/Cargo.toml`:85 on 2024-02-23 00:30_

```suggestion
reqwest = { version = "0.11.23", features = ["blocking"], default-features = false }
```

---

_Merged by @charliermarsh on 2024-02-23 00:46_

---

_Closed by @charliermarsh on 2024-02-23 00:46_

---

_Branch deleted on 2024-02-23 00:46_

---

_Label `bug` removed by @zanieb on 2024-02-23 04:33_

---

_Label `internal` added by @zanieb on 2024-02-23 04:33_

---

_Comment by @Henry-sun1974 on 2025-09-10 12:30_

I have same question.


---

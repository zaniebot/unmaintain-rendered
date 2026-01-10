```yaml
number: 4191
title: "Don't panic with invalid wheel source"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/dont-panic-invalid-source
created_at: 2024-06-10T10:49:07Z
updated_at: 2024-06-10T15:51:25Z
url: https://github.com/astral-sh/uv/pull/4191
synced_at: 2026-01-10T13:54:02Z
```

# Don't panic with invalid wheel source

---

_Pull request opened by @konstin on 2024-06-10 10:49_

Remove the panic when there is an invalid wheel source, instead surface the error. This error can only occur when manually editing the lock file, but since it's an external file, we should error and not panic.

This change is helpful since the method needs to be able to error for relative path support.

---

_Label `preview` added by @konstin on 2024-06-10 10:49_

---

_@konstin reviewed on 2024-06-10 10:49_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:1785 on 2024-06-10 10:49_

@BurntSushi Is there are reason why we implement as of those manually instead of deriving them with thiserror?

---

_@charliermarsh approved on 2024-06-10 12:12_

---

_Merged by @charliermarsh on 2024-06-10 12:44_

---

_Closed by @charliermarsh on 2024-06-10 12:44_

---

_Branch deleted on 2024-06-10 12:44_

---

_@BurntSushi reviewed on 2024-06-10 13:47_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1785 on 2024-06-10 13:47_

No real reason other than my fingers are just very used to typing out the error types. I'm fine if you want to convert over to `thiserror`.

(I have also been thinking that we might perhaps just want to use an `anyhow::Error` in most places.)

---

_@BurntSushi reviewed on 2024-06-10 13:48_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:592 on 2024-06-10 13:48_

I think the original intent here was that these cases were actually impossible as a contract of constructing a `Distribution`. I wonder if that got flubbed somehow.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:592 on 2024-06-10 13:59_

Yeah this should be impossible.

---

_@charliermarsh reviewed on 2024-06-10 13:59_

---

_@konstin reviewed on 2024-06-10 15:44_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:592 on 2024-06-10 15:44_

I wouldn't know a good way to tell serde about this correspondence, any hints?

---

_@BurntSushi reviewed on 2024-06-10 15:51_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:592 on 2024-06-10 15:51_

If I can't make the types do it for me, then I usually define a `TryFrom` impl and point Serde to that. For example: https://github.com/astral-sh/uv/blob/04c4da4e65d61347a19960f1ba94cc26d4d01458/crates/uv-resolver/src/lock.rs#L333

---

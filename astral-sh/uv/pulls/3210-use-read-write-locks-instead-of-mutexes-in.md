```yaml
number: 3210
title: Use read-write locks instead of mutexes in authentication handling
type: pull_request
state: merged
author: zanieb
labels:
  - performance
assignees: []
merged: true
base: main
head: zb/locks
created_at: 2024-04-23T14:41:02Z
updated_at: 2024-04-24T15:17:18Z
url: https://github.com/astral-sh/uv/pull/3210
synced_at: 2026-01-12T16:05:29Z
```

# Use read-write locks instead of mutexes in authentication handling

---

_@zanieb_

- Use `RwLock` for `KeyringProvider` cache
- Use `RwLock` for `CredentialsCache`


---

_Label `performance` added by @zanieb on 2024-04-23 14:41_

---

_@zanieb reviewed on 2024-04-23 14:41_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:69 on 2024-04-23 14:41_

Once again, we can avoid holding this lock across await points — should it be sync?

---

_Comment by @zanieb on 2024-04-23 14:42_

cc @naosense


---

_Review requested from @BurntSushi by @zanieb on 2024-04-23 14:44_

---

_Review comment by @BurntSushi on `crates/uv-auth/src/keyring.rs`:69 on 2024-04-23 15:04_

Yeah if you don't need to hold it over an await point, I think we should default to synchronous primitives.

---

_@BurntSushi approved on 2024-04-23 15:04_

:+1:

---

_@zanieb reviewed on 2024-04-23 15:06_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:69 on 2024-04-23 15:06_

We could also take the write lock immediately, since we take it later? Idk if you should take read  then take write or take write once.

---

_@BurntSushi reviewed on 2024-04-23 15:12_

---

_Review comment by @BurntSushi on `crates/uv-auth/src/keyring.rs`:69 on 2024-04-23 15:12_

I would just do the simplest thing. I'm not sure what happens if you take a read lock while also holding a write lock though. (Taking a write lock while holding a read lock is I think a deadlock? That is, std provides no way to "upgrade" a read lock to a write lock.)

---

_@naosense reviewed on 2024-04-24 03:03_

---

_Review comment by @naosense on `crates/uv-auth/src/keyring.rs`:69 on 2024-04-24 03:03_

> Yeah if you don't need to hold it over an await point, I think we should default to synchronous primitives.

+1，an asynchronous rwlock doesn't seem necessary here, quote from tokio document
> an asynchronous mutex is more expensive than an ordinary mutex

---

_Merged by @zanieb on 2024-04-24 15:17_

---

_Closed by @zanieb on 2024-04-24 15:17_

---

_Branch deleted on 2024-04-24 15:17_

---

```yaml
number: 1257
title: Offer tip when users omits pip prefix
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/tip
created_at: 2024-02-06T04:55:19Z
updated_at: 2024-02-06T19:25:08Z
url: https://github.com/astral-sh/uv/pull/1257
synced_at: 2026-01-12T16:04:32Z
```

# Offer tip when users omits pip prefix

---

_@charliermarsh_

## Summary

Open to other opinions here. We could just continue (and warn), prompt the user with a confirmation, etc.

(The weird thing about those two options is we might need to validate the command-line arguments _before_ we do that -- so you could get errors for bad arguments, and then get a warning that your subcommand is wrong. I can probably avoid that with more work if it feels like a better out come though.)

Closes https://github.com/astral-sh/puffin/issues/1256.


---

_Review requested from @zanieb by @charliermarsh on 2024-02-06 04:55_

---

_Review requested from @konstin by @charliermarsh on 2024-02-06 04:55_

---

_Label `error messages` added by @charliermarsh on 2024-02-06 04:55_

---

_Review comment by @konstin on `crates/puffin/tests/pip_sync.rs`:60 on 2024-02-06 14:36_

Is the `&mut` required? If so i'll fix that

---

_Review comment by @konstin on `crates/puffin/src/main.rs`:129 on 2024-02-06 14:36_

✂️ 

---

_@konstin approved on 2024-02-06 14:43_

---

_@charliermarsh reviewed on 2024-02-06 14:48_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_sync.rs`:60 on 2024-02-06 14:48_

It is!

---

_@zanieb approved on 2024-02-06 15:01_

---

_@konstin reviewed on 2024-02-06 15:03_

---

_Review comment by @konstin on `crates/puffin/tests/pip_sync.rs`:60 on 2024-02-06 15:03_

What's the error? It passes for me without `&mut`

---

_Review comment by @konstin on `crates/puffin/src/main.rs`:648 on 2024-02-06 15:04_

```suggestion
                    "uninstall" | "remove" => {
```

If we can sneak that in before merging 

---

_@konstin reviewed on 2024-02-06 15:04_

---

_@konstin reviewed on 2024-02-06 15:04_

---

_Review comment by @konstin on `crates/puffin/src/main.rs`:648 on 2024-02-06 15:04_

```suggestion
                    "freeze" | "lock" => {
```

---

_Review comment by @charliermarsh on `crates/puffin/src/main.rs`:648 on 2024-02-06 16:42_

Requires: https://github.com/astral-sh/puffin/pull/1259

---

_@charliermarsh reviewed on 2024-02-06 16:42_

---

_@charliermarsh reviewed on 2024-02-06 19:22_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_sync.rs`:60 on 2024-02-06 19:22_

Oh sorry, I need `Command::new(get_bin()).arg("sync")` without any reference.

---

_Merged by @charliermarsh on 2024-02-06 19:25_

---

_Closed by @charliermarsh on 2024-02-06 19:25_

---

_Branch deleted on 2024-02-06 19:25_

---

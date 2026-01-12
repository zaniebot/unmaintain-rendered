```yaml
number: 9648
title: "Create missing dir for `uv export`/ `uv pip compile`"
type: pull_request
state: merged
author: blueraft
labels:
  - bug
assignees: []
merged: true
base: main
head: create-missing-dir
created_at: 2024-12-04T20:40:44Z
updated_at: 2024-12-04T21:20:31Z
url: https://github.com/astral-sh/uv/pull/9648
synced_at: 2026-01-12T16:08:55Z
```

# Create missing dir for `uv export`/ `uv pip compile`

---

_@blueraft_

## Summary

Closes #9643.

I modified the `commit` fn so this applies to `uv compile --output-file` too. But I can move it to the export module if we want to restrict this to `uv export` only. 

## Test Plan

`cargo test`


---

_@zanieb reviewed on 2024-12-04 20:51_

---

_Review comment by @zanieb on `crates/uv/src/commands/mod.rs`:236 on 2024-12-04 20:51_

Don't we need [`create_dir_all`](https://doc.rust-lang.org/beta/std/fs/fn.create_dir_all.html)?


---

_@charliermarsh reviewed on 2024-12-04 20:51_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/mod.rs`:236 on 2024-12-04 20:51_

I think we should just unequivocally call `create_dir_all`.

---

_Review comment by @zanieb on `crates/uv/src/commands/mod.rs`:235 on 2024-12-04 20:52_

Should this be `try_exists`? What happens if the parent is a link to another directory? What happens if it's a file?

---

_@zanieb reviewed on 2024-12-04 20:52_

---

_@blueraft reviewed on 2024-12-04 21:00_

---

_Review comment by @blueraft on `crates/uv/src/commands/mod.rs`:236 on 2024-12-04 21:00_

Changed it to `create_dir_all`

---

_@blueraft reviewed on 2024-12-04 21:00_

---

_Review comment by @blueraft on `crates/uv/src/commands/mod.rs`:235 on 2024-12-04 21:00_

makes sense, I've changed it to `try_exists`

---

_Review comment by @zanieb on `crates/uv/src/commands/mod.rs`:236 on 2024-12-04 21:06_

It might make sense to skip `try_exists` as suggested by Charlie, it already has a behavior for existing directories and probably handles race conditions?

Also, I think you can skip the empty check

> If the empty path is passed to this function, it always succeeds without creating any directories.

---

_@zanieb reviewed on 2024-12-04 21:06_

---

_@blueraft reviewed on 2024-12-04 21:10_

---

_Review comment by @blueraft on `crates/uv/src/commands/mod.rs`:236 on 2024-12-04 21:10_

ah ok done 

---

_@zanieb approved on 2024-12-04 21:11_

Thank you

---

_Label `enhancement` added by @zanieb on 2024-12-04 21:11_

---

_Label `enhancement` removed by @charliermarsh on 2024-12-04 21:19_

---

_Label `bug` added by @charliermarsh on 2024-12-04 21:19_

---

_Merged by @zanieb on 2024-12-04 21:20_

---

_Closed by @zanieb on 2024-12-04 21:20_

---

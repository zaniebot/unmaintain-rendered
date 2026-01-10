```yaml
number: 2782
title: Simplify sorting of site package entries
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/refactor
created_at: 2024-04-02T20:20:10Z
updated_at: 2024-04-03T04:33:12Z
url: https://github.com/astral-sh/uv/pull/2782
synced_at: 2026-01-10T14:49:08Z
```

# Simplify sorting of site package entries

---

_Pull request opened by @zanieb on 2024-04-02 20:20_

per https://github.com/astral-sh/uv/pull/2780#discussion_r1548340373

---

_Label `internal` added by @zanieb on 2024-04-02 20:20_

---

_@charliermarsh reviewed on 2024-04-02 20:22_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:54 on 2024-04-02 20:22_

I think this is losing `file_type` errors because they're being cast to `None`.

---

_@zanieb reviewed on 2024-04-02 20:26_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:54 on 2024-04-02 20:26_

Ah yeah that is what I was having trouble avoiding.

cc @snowsignal 



---

_@charliermarsh reviewed on 2024-04-02 20:29_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:54 on 2024-04-02 20:29_

```rust
let directories: BTreeSet<_> = site_packages
    .filter_map(|read_dir| match read_dir {
        Ok(entry) => {
            let file_type = match entry.file_type() {
                Ok(file_type) => file_type,
                Err(err) => return Some(Err(err)),
            };
            file_type.is_dir().then_some(Ok(entry.path()))
        }
        Err(err) => Some(Err(err)),
    })
    .collect::<Result<_, std::io::Error>>()?;
```

---

_@snowsignal reviewed on 2024-04-02 20:31_

---

_Review comment by @snowsignal on `crates/uv-installer/src/site_packages.rs`:54 on 2024-04-02 20:31_

Haha you beat me to it 😆 

---

_@zanieb reviewed on 2024-04-02 20:33_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:54 on 2024-04-02 20:33_

Okay yuck though is this actually easier to read than before this pull request?

---

_@snowsignal reviewed on 2024-04-02 20:41_

---

_Review comment by @snowsignal on `crates/uv-installer/src/site_packages.rs`:54 on 2024-04-02 20:41_

I tried to clean it up a bit:

```rust
let directories: BTreeSet<_> = site_packages
    .filter_map(|read_dir| match read_dir {
        Ok(entry) => match entry.file_type() {
                Ok(file_type) => file_type.is_dir().then_some(Ok(entry.path())),
                Err(err) => Some(Err(err)),
            }
        Err(err) => Some(Err(err)),
    })
    .collect::<Result<_>>()?;
```

---

_@charliermarsh approved on 2024-04-03 02:40_

---

_@snowsignal approved on 2024-04-03 04:10_

---

_Merged by @zanieb on 2024-04-03 04:33_

---

_Closed by @zanieb on 2024-04-03 04:33_

---

_Branch deleted on 2024-04-03 04:33_

---

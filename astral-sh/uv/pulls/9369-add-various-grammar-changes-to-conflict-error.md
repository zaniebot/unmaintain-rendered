```yaml
number: 9369
title: Add various grammar changes to conflict error messages
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/pl
created_at: 2024-11-22T20:27:19Z
updated_at: 2024-11-22T22:23:15Z
url: https://github.com/astral-sh/uv/pull/9369
synced_at: 2026-01-10T12:00:00Z
```

# Add various grammar changes to conflict error messages

---

_Pull request opened by @charliermarsh on 2024-11-22 20:27_

## Summary

If all items are the same kind, we can avoid repeating "extra" and "group". If there are two, we now use "X and Y", etc.


---

_Label `error messages` added by @charliermarsh on 2024-11-22 20:27_

---

_Comment by @charliermarsh on 2024-11-22 20:27_

This is a little silly, but 🤷‍♂️ 

---

_Review requested from @zanieb by @charliermarsh on 2024-11-22 20:28_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-22 20:28_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:2364 on 2024-11-22 20:29_

Capitalizing here feels a bit off but we do capitalize everywhere else...

---

_@charliermarsh reviewed on 2024-11-22 20:29_

---

_@zanieb reviewed on 2024-11-22 21:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/mod.rs`:262 on 2024-11-22 21:03_

Hilariously, there's also

```rust
fn conjunction(items: &[&str]) -> String {
    match items.len() {
        1 => items[0].to_string(),
        2 => format!("{} or {}", items[0], items[1]),
        _ => {
            let last = items.last().unwrap();
            format!(
                "{}, or {}",
                items.iter().take(items.len() - 1).join(", "),
                last
            )
        }
    }
}
```

---

_@zanieb approved on 2024-11-22 21:03_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/mod.rs`:276 on 2024-11-22 21:22_

Boooo Oxford comma!

---

_@BurntSushi approved on 2024-11-22 21:24_

Nice

---

_@charliermarsh reviewed on 2024-11-22 22:11_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/mod.rs`:262 on 2024-11-22 22:11_

Lol. Do we like that impl more?

---

_Merged by @charliermarsh on 2024-11-22 22:23_

---

_Closed by @charliermarsh on 2024-11-22 22:23_

---

_Branch deleted on 2024-11-22 22:23_

---

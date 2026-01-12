```yaml
number: 1258
title: "Add hint for `--no-index` without `--find-links`"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/find-links-err
created_at: 2024-02-06T14:57:15Z
updated_at: 2024-02-06T19:19:45Z
url: https://github.com/astral-sh/uv/pull/1258
synced_at: 2026-01-12T16:04:32Z
```

# Add hint for `--no-index` without `--find-links`

---

_@zanieb_

Since unavailable packages with `--no-index` can be confusing when the user does not also provide `--find-links` we add a hint for this case. Required some plumbing to get the required information to the `NoSolution` error.

---

_Marked ready for review by @zanieb on 2024-02-06 15:02_

---

_@zanieb reviewed on 2024-02-06 15:05_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile.rs`:2998 on 2024-02-06 15:05_

@konstin why are the color codes in this one but not the others? 😕 

---

_Label `error messages` added by @zanieb on 2024-02-06 16:20_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:347 on 2024-02-06 16:23_

What's listing?

---

_@charliermarsh reviewed on 2024-02-06 16:56_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:240 on 2024-02-06 16:56_

Should this also take `visited`?

---

_@charliermarsh approved on 2024-02-06 16:57_

---

_@charliermarsh reviewed on 2024-02-06 16:57_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:240 on 2024-02-06 16:57_

Oh, or does this by definition only contain packages we visited? It's not filled via prefetching?

---

_@zanieb reviewed on 2024-02-06 16:59_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:240 on 2024-02-06 16:59_

I considered it but I think it's okay not to in this case

---

_Merged by @zanieb on 2024-02-06 17:04_

---

_Closed by @zanieb on 2024-02-06 17:04_

---

_Branch deleted on 2024-02-06 17:04_

---

_Review comment by @konstin on `crates/puffin/tests/pip_compile.rs`:2998 on 2024-02-06 19:18_

The `eprint!` was using the `std` in the prelude instead of the `anstream` print, lacking the color code stripping.

---

_@konstin reviewed on 2024-02-06 19:19_

---

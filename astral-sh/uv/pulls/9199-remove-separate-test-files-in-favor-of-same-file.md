```yaml
number: 9199
title: "Remove separate test files in favor of same-file `mod tests`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2024-11-18T14:21:16Z
updated_at: 2024-11-18T20:11:47Z
url: https://github.com/astral-sh/uv/pull/9199
synced_at: 2026-01-12T16:08:42Z
```

# Remove separate test files in favor of same-file `mod tests`

---

_@charliermarsh_

## Summary

These were moved as part of a broader refactor to create a single integration test module. That "single integration test module" did indeed have a big impact on compile times, which is great! But we aren't seeing any benefit from moving these tests into their own files (despite the claim in [this blog post](https://matklad.github.io/2021/02/27/delete-cargo-integration-tests.html), I see the same compilation pattern regardless of where the tests are located). Plus, we don't have many of these, and same-file tests is such a strong Rust convention.


---

_Label `testing` added by @charliermarsh on 2024-11-18 19:56_

---

_@BurntSushi approved on 2024-11-18 19:58_

---

_Marked ready for review by @charliermarsh on 2024-11-18 20:05_

---

_Merged by @charliermarsh on 2024-11-18 20:11_

---

_Closed by @charliermarsh on 2024-11-18 20:11_

---

_Branch deleted on 2024-11-18 20:11_

---

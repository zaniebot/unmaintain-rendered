```yaml
number: 1930
title: Add long-form version output
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/version
created_at: 2024-02-23T18:44:34Z
updated_at: 2024-02-23T19:45:02Z
url: https://github.com/astral-sh/uv/pull/1930
synced_at: 2026-01-10T14:54:43Z
```

# Add long-form version output

---

_Pull request opened by @zanieb on 2024-02-23 18:44_

Similar to https://github.com/astral-sh/ruff/pull/8034

Adds more version information so it's clear what revision the user is on

```
❯ cargo run -q -- --version
uv 0.1.10 (daa8565a7 2024-02-23)
❯ cargo run -q -- -V
uv 0.1.10
❯ cargo run -q -- version
uv 0.1.10 (daa8565a7 2024-02-23)
❯ cargo run -q -- version --output-format json
{
  "version": "0.1.10",
  "commit_info": {
    "short_commit_hash": "daa8565a7",
    "commit_hash": "daa8565a75249305821fdc34ace085060c082ba3",
    "commit_date": "2024-02-23",
    "last_tag": null,
    "commits_since_last_tag": 0
  }
}
```

---

_Label `cli` added by @zanieb on 2024-02-23 18:44_

---

_@charliermarsh approved on 2024-02-23 18:45_

---

_@zanieb reviewed on 2024-02-23 18:49_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:58 on 2024-02-23 18:49_

We could port this over to Ruff if we want

---

_@zanieb reviewed on 2024-02-23 18:49_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:130 on 2024-02-23 18:49_

Idk about the json output format, included because it's that way in Ruff.

---

_Merged by @zanieb on 2024-02-23 19:45_

---

_Closed by @zanieb on 2024-02-23 19:45_

---

_Branch deleted on 2024-02-23 19:45_

---

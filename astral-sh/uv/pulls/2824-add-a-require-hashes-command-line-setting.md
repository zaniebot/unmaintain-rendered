```yaml
number: 2824
title: "Add a `--require-hashes` command-line setting"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - no-build
assignees: []
merged: true
base: main
head: charlie/check-hashes
created_at: 2024-04-04T18:56:03Z
updated_at: 2024-04-10T18:07:05Z
url: https://github.com/astral-sh/uv/pull/2824
synced_at: 2026-01-12T16:05:14Z
```

# Add a `--require-hashes` command-line setting

---

_@charliermarsh_

## Summary

I'll likely only merge this once the PR chain is further along, but this PR wires up the setting fro the CLI.

---

_Label `enhancement` added by @charliermarsh on 2024-04-04 18:56_

---

_Label `do-not-merge` added by @charliermarsh on 2024-04-04 18:56_

---

_Label `no-build` added by @charliermarsh on 2024-04-05 01:59_

---

_Review comment by @konstin on `crates/uv/src/main.rs`:598 on 2024-04-10 11:59_

Can we require a full length hash for git, or is that just a follow up PR?

---

_@konstin approved on 2024-04-10 11:59_

---

_@charliermarsh reviewed on 2024-04-10 18:06_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:598 on 2024-04-10 18:06_

We could but pip doesn't support these so I was hesitant to deviate.

---

_Marked ready for review by @charliermarsh on 2024-04-10 18:06_

---

_Label `do-not-merge` removed by @charliermarsh on 2024-04-10 18:06_

---

_Merged by @charliermarsh on 2024-04-10 18:07_

---

_Closed by @charliermarsh on 2024-04-10 18:07_

---

_Branch deleted on 2024-04-10 18:07_

---

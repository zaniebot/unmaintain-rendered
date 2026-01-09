---
number: 202
title: Support VCS dependencies
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - wish
assignees: []
created_at: 2023-10-26T13:46:37Z
updated_at: 2023-11-02T15:14:57Z
url: https://github.com/astral-sh/uv/issues/202
synced_at: 2026-01-07T13:12:16-06:00
---

# Support VCS dependencies

---

_Issue opened by @charliermarsh on 2023-10-26 13:46_

_No description provided._

---

_Label `enhancement` added by @charliermarsh on 2023-10-26 13:46_

---

_Label `wish` added by @charliermarsh on 2023-10-26 13:46_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-26 13:46_

---

_Comment by @konstin on 2023-10-26 15:17_

Monotrail implementation: https://github.com/konstin/poc-monotrail/blob/10730ea1a84c58af6b35fb74c89ed0578ab042b6/crates/monotrail/src/install.rs#L380-L411

For monotrail the openssl linked by libgit2 was a huge pain to build and deploy.

If we can, i'd like to use https://docs.rs/gix/latest/gix/, it's native rust, supposed to be faster, has a rustls option and is stable enough that it's used by cargo (e.g. https://github.com/rust-lang/cargo/pull/11448)


---

_Comment by @charliermarsh on 2023-10-26 15:18_

Awesome, thank you.

---

_Label `wish` removed by @charliermarsh on 2023-10-29 19:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-29 19:34_

---

_Label `wish` added by @charliermarsh on 2023-10-29 19:34_

---

_Referenced in [astral-sh/uv#283](../../astral-sh/uv/pulls/283.md) on 2023-11-02 01:29_

---

_Closed by @charliermarsh on 2023-11-02 15:14_

---

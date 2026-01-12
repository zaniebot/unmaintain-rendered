```yaml
number: 4720
title: Lock for the duration of tool commands
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: zb/tool-lock
created_at: 2024-07-02T02:43:03Z
updated_at: 2024-07-10T16:16:15Z
url: https://github.com/astral-sh/uv/pull/4720
synced_at: 2026-01-12T16:06:24Z
```

# Lock for the duration of tool commands

---

_@zanieb_

Feedback from https://github.com/astral-sh/uv/pull/4501#discussion_r1655391958

---

_Label `enhancement` added by @zanieb on 2024-07-02 02:43_

---

_Label `preview` added by @zanieb on 2024-07-02 02:43_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:120 on 2024-07-02 02:44_

We don't actually use this now. In what case does this makes sense?

---

_@zanieb reviewed on 2024-07-02 02:44_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-02 02:44_

---

_Review requested from @konstin by @zanieb on 2024-07-02 02:44_

---

_Marked ready for review by @zanieb on 2024-07-02 02:45_

---

_Comment by @zanieb on 2024-07-02 02:51_

Ah this is a bit annoying because now we're locking the directory before we make sure it exists, which I preferred to have happen once we knew we wanted to mutate it so we weren't creating during inspection. I'm not sure how much this matters in practice.

---

_@konstin approved on 2024-07-02 07:58_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:120 on 2024-07-02 23:09_

I think this would make sense for `tool run`, `tool install`, `tool uninstall`, right?

---

_@charliermarsh reviewed on 2024-07-02 23:09_

---

_Comment by @charliermarsh on 2024-07-02 23:10_

> Ah this is a bit annoying because now we're locking the directory before we make sure it exists, which I preferred to have happen once we knew we wanted to mutate it so we weren't creating during inspection. I'm not sure how much this matters in practice.

Where is this happening in the code? I think it's ok though.

---

_Comment by @zanieb on 2024-07-02 23:18_

@charliermarsh you can see the CI failures are due to the lock being acquired without `init` first which throws an error. Previously, we waited to call `init` until we needed do something.

---

_Comment by @charliermarsh on 2024-07-02 23:19_

Ahh sorry yes, that makes sense.

---

_@zanieb reviewed on 2024-07-02 23:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:19 on 2024-07-02 23:19_

@charliermarsh e.g. here we do not call `init()?` because this is an inspection-only command.

---

_@zanieb reviewed on 2024-07-02 23:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:128 on 2024-07-02 23:20_

@charliermarsh and here the `init` is deferred all the way to [L305](https://github.com/astral-sh/uv/pull/4720/files#diff-0dfdec55a014c8bde81440471c7410d26a840618e095bdb007abedf8866e51e3R305)

---

_@zanieb reviewed on 2024-07-02 23:21_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:120 on 2024-07-02 23:21_

But then `tool list` could try to inspect a tool mid-installation.

---

_@charliermarsh reviewed on 2024-07-10 15:24_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:120 on 2024-07-10 15:24_

Can probably just always-lock then.

---

_@BurntSushi reviewed on 2024-07-10 15:28_

My little nit here is to use a different word than "safe" in the comments, just to avoid it being confusable with the Rust concept of safety. Maybe, "correct" instead?

---

_Merged by @zanieb on 2024-07-10 16:16_

---

_Closed by @zanieb on 2024-07-10 16:16_

---

_Branch deleted on 2024-07-10 16:16_

---

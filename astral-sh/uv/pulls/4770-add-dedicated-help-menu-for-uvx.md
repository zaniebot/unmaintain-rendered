```yaml
number: 4770
title: "Add dedicated help menu for `uvx`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/uvx-help
created_at: 2024-07-03T14:14:42Z
updated_at: 2024-07-03T16:38:55Z
url: https://github.com/astral-sh/uv/pull/4770
synced_at: 2026-01-12T16:06:27Z
```

# Add dedicated help menu for `uvx`

---

_@zanieb_

Closes #4749 

---

_Label `cli` added by @zanieb on 2024-07-03 14:14_

---

_Label `preview` added by @zanieb on 2024-07-03 14:14_

---

_Comment by @zanieb on 2024-07-03 14:16_

e.g.

```
❯ cargo run --bin uvx -- -h
Run a tool.

Usage: uvx [OPTIONS] <COMMAND>
```

---

_@charliermarsh reviewed on 2024-07-03 14:21_

---

_Review comment by @charliermarsh on `crates/uv/src/bin/uvx.rs`:14 on 2024-07-03 14:21_

Maybe... `uvx` instead of `x`?

---

_@charliermarsh approved on 2024-07-03 14:21_

---

_@zanieb reviewed on 2024-07-03 14:28_

---

_Review comment by @zanieb on `crates/uv/src/bin/uvx.rs`:14 on 2024-07-03 14:28_

I'm roughly opinionless here, do you have a concern? I also considered `uv x`.

---

_Review comment by @charliermarsh on `crates/uv/src/bin/uvx.rs`:14 on 2024-07-03 15:08_

I think `x` just felt a bit random, where `uvx` is clearer RE why it exists and less confusing if someone accidentally runs it.

---

_@charliermarsh reviewed on 2024-07-03 15:08_

---

_Merged by @zanieb on 2024-07-03 16:38_

---

_Closed by @zanieb on 2024-07-03 16:38_

---

_Branch deleted on 2024-07-03 16:38_

---

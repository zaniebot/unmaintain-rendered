```yaml
number: 4773
title: "Drop the crate description from the `uv` help menu"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/cli-about
created_at: 2024-07-03T14:35:51Z
updated_at: 2024-07-03T16:12:51Z
url: https://github.com/astral-sh/uv/pull/4773
synced_at: 2026-01-10T13:48:28Z
```

# Drop the crate description from the `uv` help menu

---

_Pull request opened by @zanieb on 2024-07-03 14:35_

Previously this displayed:

```
❯ cargo run -q -- --help
The command line interface for the uv binary.

Usage: uv [OPTIONS] <COMMAND>
```

This is.. weird. Here I remove it entirely. I could see adding `about_long` text being helpful in the future.

---

_Label `cli` added by @zanieb on 2024-07-03 14:35_

---

_Marked ready for review by @zanieb on 2024-07-03 14:36_

---

_Comment by @charliermarsh on 2024-07-03 15:09_

Should we just replace this with the tagline we have on the repo? "An extremely fast Python package installer and resolver, written in Rust."

---

_@charliermarsh approved on 2024-07-03 15:09_

---

_Comment by @zanieb on 2024-07-03 16:06_

Well... I liked that but I guess I want something a little more succinct and less marketing-oriented. It's already installed, ya know.

---

_Merged by @zanieb on 2024-07-03 16:11_

---

_Closed by @zanieb on 2024-07-03 16:11_

---

_Branch deleted on 2024-07-03 16:11_

---

_Comment by @zanieb on 2024-07-03 16:12_

Will consider a new one in a separate change because I think this one is bad enough to remove immediately.

---

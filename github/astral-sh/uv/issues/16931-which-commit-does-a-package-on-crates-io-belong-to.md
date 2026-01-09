---
number: 16931
title: Which commit does a package on crates.io belong to?
type: issue
state: closed
author: pavelzw
labels:
  - question
assignees: []
created_at: 2025-12-02T14:18:01Z
updated_at: 2025-12-02T20:02:23Z
url: https://github.com/astral-sh/uv/issues/16931
synced_at: 2026-01-07T13:12:19-06:00
---

# Which commit does a package on crates.io belong to?

---

_Issue opened by @pavelzw on 2025-12-02 14:18_

### Question

@zanieb I'm struggling a bit to find out which commit a package on crates.io belongs to.
for example, https://crates.io/crates/uv-trampoline-builder/0.0.4 doesn't indicate which commit it belongs to.
https://github.com/conda/rattler solves it by pushing a separate tag per crate:

<img width="332" height="563" alt="Image" src="https://github.com/user-attachments/assets/8e5cefa1-442b-4f80-9e43-9584798508fa" />

They are using `release-plz` https://github.com/conda/rattler/blob/755f6ec5d2deb9659b64c1480458950f137ab379/.github/workflows/release-rust.yaml#L54, https://github.com/conda/rattler/pull/1888 for handling multiple releases which works pretty well imo. @baszalmstra might be able to tell you more about it if you are interested))

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @pavelzw on 2025-12-02 14:18_

---

_Comment by @zanieb on 2025-12-02 14:33_

The crates are all released simultaneously with the uv release, so the tag for the commit is 0.9.14.

I'm a little wary to add 60x tags for our workspace members on every release.

---

_Comment by @pavelzw on 2025-12-02 14:44_

are you planning to always use the same versions for all subcrates? if so, maybe just pushing `crates-v0.0.4`?

---

_Comment by @zanieb on 2025-12-02 14:51_

For now, yes. The tag is always going to be on the same commit as a uv version tag, so I think I'd rather try to find a way to just indicate the uv version in each crate when we publish 🤔 perhaps just in the READMEs

---

_Referenced in [astral-sh/uv#16939](../../astral-sh/uv/pulls/16939.md) on 2025-12-02 19:08_

---

_Closed by @zanieb on 2025-12-02 20:02_

---

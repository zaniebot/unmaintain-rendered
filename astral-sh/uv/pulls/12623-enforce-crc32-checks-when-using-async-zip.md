```yaml
number: 12623
title: enforce crc32 checks when using async-zip
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/crc
created_at: 2025-04-02T14:31:54Z
updated_at: 2025-04-02T15:21:26Z
url: https://github.com/astral-sh/uv/pull/12623
synced_at: 2026-01-10T11:10:40Z
```

# enforce crc32 checks when using async-zip

---

_Pull request opened by @Gankra on 2025-04-02 14:31_

Fixes #12618 

Instead of succeeding the user now gets:

```
uvdloc pip install osqp==1.0.2 --reinstall --python-platform=linux
Resolved 7 packages in 171ms
  × Failed to download `osqp==1.0.2`
  ├─▶ Failed to extract archive
  ╰─▶ a computed CRC32 value did not match the expected value
```

I am not entirely sure if we have infra for testing this kind of thing, but it would be nice to check in a test or two. I'm also not entirely clear if there's any cases where these checks are overzealous.

---

_Label `bug` added by @Gankra on 2025-04-02 14:31_

---

_@zanieb approved on 2025-04-02 14:36_

Can we wrap the error message to include the filename and note it's a problem with the package?

You could add a test case that `uv pip install <url>` for that bad wheel?

---

_Comment by @zanieb on 2025-04-02 14:45_

Or, if you can construct a small bad wheel, you could probably check it in?

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:8911 on 2025-04-02 15:15_

I'd probably say "Bad CRC for file `{filename}`: got ..." — it's not clear what this filename means as written? I guess "Bad CRC (got ...) for file: <path>" or similar works too? Maybe all I want is "for file" in the message :D

Maybe we should also attach the archive name to "Failed to extract archive"?

---

_@zanieb reviewed on 2025-04-02 15:15_

---

_Merged by @Gankra on 2025-04-02 15:21_

---

_Closed by @Gankra on 2025-04-02 15:21_

---

_Branch deleted on 2025-04-02 15:21_

---

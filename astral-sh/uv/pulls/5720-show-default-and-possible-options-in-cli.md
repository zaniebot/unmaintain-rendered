```yaml
number: 5720
title: Show default and possible options in CLI reference documentation
type: pull_request
state: merged
author: eth3lbert
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: doc-help-value
created_at: 2024-08-02T08:56:46Z
updated_at: 2024-08-02T13:20:34Z
url: https://github.com/astral-sh/uv/pull/5720
synced_at: 2026-01-10T13:37:23Z
```

# Show default and possible options in CLI reference documentation

---

_Pull request opened by @eth3lbert on 2024-08-02 08:56_

## Summary

Closes #5692 .



---

_Renamed from "Doc help value" to "Show default and possible options in CLI reference documentation" by @eth3lbert on 2024-08-02 08:57_

---

_Comment by @eth3lbert on 2024-08-02 09:00_

Partial screenshot:

<img width="720" alt="image" src="https://github.com/user-attachments/assets/6f8f70a5-dc59-4989-a7a9-1df29112c6e7">


---

_Review requested from @zanieb by @konstin on 2024-08-02 09:05_

---

_Comment by @eth3lbert on 2024-08-02 09:09_

Currently the `cli.md` is a bit hard to read without a browser. 😂 

---

_Label `documentation` added by @zanieb on 2024-08-02 12:36_

---

_Label `preview` added by @zanieb on 2024-08-02 12:36_

---

_@zanieb reviewed on 2024-08-02 12:36_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_cli_reference.rs`:245 on 2024-08-02 12:36_

Can you prefix these with `emit` (i.e., `emit_default_option`) for clarity?

---

_Comment by @zanieb on 2024-08-02 12:39_

Did you base this format on someone elses? Trying to figure out what the best way to display it is.

---

_@eth3lbert reviewed on 2024-08-02 12:42_

---

_Review comment by @eth3lbert on `crates/uv-dev/src/generate_cli_reference.rs`:245 on 2024-08-02 12:42_

sure!

---

_Comment by @eth3lbert on 2024-08-02 12:44_

> Did you base this format on someone elses? Trying to figure out what the best way to display it is.

This is based on the output of `uv help python`.

---

_Comment by @zanieb on 2024-08-02 12:49_

That's good enough for me! We can revisit later if we want, i.e. this is different from our settings reference page.

---

_@zanieb approved on 2024-08-02 12:49_

---

_Comment by @eth3lbert on 2024-08-02 12:54_

> Did you base this format on someone elses? Trying to figure out what the best way to display it is.

Oh, I could implement it in the same format as in settings if you'd like.

---

_Merged by @zanieb on 2024-08-02 12:55_

---

_Closed by @zanieb on 2024-08-02 12:55_

---

_Branch deleted on 2024-08-02 12:56_

---

_Comment by @zanieb on 2024-08-02 13:20_

I kind of prefer it as-is, I think. We'll see.

---

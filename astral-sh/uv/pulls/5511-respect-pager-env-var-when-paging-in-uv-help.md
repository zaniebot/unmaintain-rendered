```yaml
number: 5511
title: "Respect `PAGER` env var when paging in `uv help` command"
type: pull_request
state: merged
author: krishnan-chandra
labels:
  - cli
assignees: []
merged: true
base: main
head: support-pager-env-in-help
created_at: 2024-07-27T21:24:46Z
updated_at: 2024-10-24T17:57:46Z
url: https://github.com/astral-sh/uv/pull/5511
synced_at: 2026-01-12T16:06:51Z
```

# Respect `PAGER` env var when paging in `uv help` command

---

_@krishnan-chandra_

## Summary

Closes https://github.com/astral-sh/uv/issues/4931.

## Test Plan

Tried running the following commands locally to make sure that all cases work:

```
unset PAGER
cargo run -- help venv
```

With no pager set, `uv` correctly finds `less` on the system as it did before and passes the help output to it.

---

```
PAGER= cargo run -- help venv
```

This correctly prints out to stdout and does not use any pager.

---

```
PAGER=most cargo run -- help venv
```

This correctly opens the `most` pager as shown below:

<img width="1917" alt="Screenshot 2024-07-27 at 5 14 42 PM" src="https://github.com/user-attachments/assets/dfaa5a83-b47e-4f5c-9be1-b0b1e9818932">

---

_Comment by @charliermarsh on 2024-07-27 21:31_

Thanks for taking this on!

---

_Renamed from "Respect PAGER env var when paging in help command" to "Respect `PAGER` env var when paging in `uv help` command" by @krishnan-chandra on 2024-07-27 21:41_

---

_Assigned to @zanieb by @zanieb on 2024-07-30 14:35_

---

_Comment by @zanieb on 2024-07-30 15:31_

Is it valid to set `PAGER` to a path or a pager program with arguments e.g. `foo -x`?

---

_Comment by @krishnan-chandra on 2024-07-30 15:46_

Looks like paths are valid, but args are not:

<img width="688" alt="Screenshot 2024-07-30 at 11 45 28 AM" src="https://github.com/user-attachments/assets/a5ffe9de-92b5-4ed1-a74c-7442c10cbae4">

---

_Comment by @zanieb on 2024-07-30 15:58_

Thanks for checking! Seems like we'll need to support parsing it as a path and checking the final component name to see if it's `less`.

---

_Comment by @krishnan-chandra on 2024-07-30 20:12_

Turns out I was testing the wrong thing earlier - the output failed because the Rust code didn't parse the pager args correctly, not because they were invalid. It turns out that you can pass args as part of `PAGER`, because this works:

```
PAGER='less -R' man wc
```

So I added in parsing for both paths and args; in the special case where the pager command is `less` and `PAGER` already contains the args for it, we use the provided args rather than trying to inject defaults (which seems sensible to me?)

---

_Review requested from @zanieb by @zanieb on 2024-08-01 20:23_

---

_Comment by @zanieb on 2024-10-01 17:44_

I refactored the implementation in https://github.com/astral-sh/uv/pull/5511/commits/bbefd11ee1e133c2b01fb76f0be2eb12a2d8d622 in the hopes it will be easier to maintain.

---

_Label `cli` added by @zanieb on 2024-10-01 17:44_

---

_@zanieb approved on 2024-10-01 17:45_

---

_Merged by @zanieb on 2024-10-01 17:50_

---

_Closed by @zanieb on 2024-10-01 17:50_

---

_Branch deleted on 2024-10-01 19:38_

---

_Comment by @krishnan-chandra on 2024-10-01 19:39_

Got it, thanks for the refactor and merge!

---

_Comment by @zanieb on 2024-10-01 19:45_

Yeah sorry for the delay! Been on my backburner for a while. Let me know if you have any thoughts or questions on the changes.

---

_Comment by @twmr on 2024-10-23 20:41_

@krishnan-chandra For me `PAGER='less' uv help` doesn't work, but `PAGER='less' git log` does. 
Tested with both 0.4.25 and 0.4.26.
Should I create a new ticket for that?

---

_Comment by @krishnan-chandra on 2024-10-23 22:22_

Can you elaborate on what "doesn't work" means in this context? What is the issue that you're seeing?

---

_Comment by @twmr on 2024-10-24 05:33_

Sure, for me the output is not paged at all. So it looks like if no pager is set.
As I said above it works with `git`. (OS: Ubuntu 24.04 and zsh)

---

_Comment by @zanieb on 2024-10-24 12:19_

`uv help` doesn't page because it's really short. Does `PAGER='less' uv help run` page?

---

_Comment by @twmr on 2024-10-24 17:57_

Thx @zanieb! The output of `uv help run` is indeed paged.

---

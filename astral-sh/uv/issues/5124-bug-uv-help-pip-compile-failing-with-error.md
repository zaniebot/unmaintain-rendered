```yaml
number: 5124
title: "Bug: `uv help pip compile` failing with `error: program not found`"
type: issue
state: closed
author: jbw-vtl
labels:
  - bug
  - help wanted
  - windows
  - cli
assignees: []
created_at: 2024-07-16T19:37:25Z
updated_at: 2024-07-22T14:08:59Z
url: https://github.com/astral-sh/uv/issues/5124
synced_at: 2026-01-12T15:58:54Z
```

# Bug: `uv help pip compile` failing with `error: program not found`

---

_@jbw-vtl_

Running the command `uv help pip compile -v` is failing with

```text
DEBUG uv 0.2.25
error: program not found
```

As advised by @zanieb, `uv help pip compile --no-pager` is working as expected.
Additionally `uv pip compile --help` is working aswell, however the output differs from the above command in structure & content.

```
uv==0.2.25
python==3.11.9
Running on Windows
```

---

_Comment by @charliermarsh on 2024-07-16 19:42_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-07-16 19:42_

---

_Label `cli` added by @charliermarsh on 2024-07-16 19:42_

---

_Label `help wanted` added by @zanieb on 2024-07-16 20:09_

---

_Label `windows` added by @zanieb on 2024-07-16 20:09_

---

_Comment by @zanieb on 2024-07-16 20:10_

This sounds like the dreaded "`which` finds executables differently then spawning a process on Windows". I have no idea why Cargo doesn't have this problem, as we used their implementation here. We might be able to just pass the path discovered by `which` to the subprocess invocation?

---

_Comment by @jbw-vtl on 2024-07-16 20:22_

Running the `uv help pip compile` in bash on windows works (with the same versions as stated above).
Running within Powershell (tried multiple versions) and CMD both fails with the `error: program not found`

`which` is by default not available in Powershell (using `get-command uv`) / Cmd (using `where uv`)

---

_Comment by @zanieb on 2024-07-16 20:24_

Note I'm referring to the `which` implementation in Rust, not the executable program.

---

_Comment by @zanieb on 2024-07-16 20:25_

I've remembered that Cargo uses a custom "resolve_executables" method instead of `which` which is probably why they do not suffer from this problem.

---

_Comment by @zanieb on 2024-07-18 14:37_

Just to confirm, this works as intended if the pager is not found by `which`:

```
PATH="" ~/.cargo/bin/uv help pip
```

---

_Comment by @charliermarsh on 2024-07-18 14:47_

@zanieb - Do you think we can fix this for a release today?

---

_Comment by @zanieb on 2024-07-18 14:52_

@charliermarsh if someone with a Windows machine wants to test, it's either a trivial "pass the full path to the subprocess invocation" or turn off paging on Windows to resolve the issue immediately and explore the proper solution.

---

_Comment by @charliermarsh on 2024-07-18 14:54_

I can test it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-18 18:50_

---

_Closed by @charliermarsh on 2024-07-18 20:29_

---

_Comment by @jbw-vtl on 2024-07-22 14:03_

Can confirm working well with the latest version, thanks a bunch

---

_Comment by @charliermarsh on 2024-07-22 14:08_

Awesome, thank you.

---

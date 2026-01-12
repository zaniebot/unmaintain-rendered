```yaml
number: 8847
title: "Warn when `.python-version` conflicts with `uv venv -p <version>` in projects"
type: issue
state: open
author: tleonhardt
labels:
  - error messages
assignees: []
created_at: 2024-11-05T23:23:08Z
updated_at: 2025-05-08T19:28:48Z
url: https://github.com/astral-sh/uv/issues/8847
synced_at: 2026-01-12T15:59:36Z
```

# Warn when `.python-version` conflicts with `uv venv -p <version>` in projects

---

_@tleonhardt_

# Summary

I ask `uv` to install a Python 3.12 virtual environment and then ask it to run a command using what I assume is the default virtual environment and it actually runs it using Python 3.13.

Perhaps I am misunderstanding how `uv` is supposed to work but this is a very non-intuitive customer experience.

## How to reproduce

```sh
$ uv venv --python 3.12
Using CPython 3.12.6
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ uv run python -V
Using CPython 3.13.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv
...
```

## Platform details
```sh
$  uv --version
uv 0.4.30 (Homebrew 2024-11-05)
```
- OS: macOS Sequoia 15.1 (Apple Silicon)

---

_Comment by @charliermarsh on 2024-11-05 23:24_

Do you have a `.python-version` file in the directory that pins to 3.13, maybe?

---

_Comment by @tleonhardt on 2024-11-05 23:26_

Doh!  I do have a `.python-version` file in that directory pinned to 3.13.  I'm not sure where it came from.

I removed it and now it works as expected.

@charliermarsh You are the best.  Your response time is truly insane.  Thank you sir!

---

_Closed by @tleonhardt on 2024-11-05 23:26_

---

_Comment by @charliermarsh on 2024-11-05 23:27_

Really glad I could help! (I wonder if we should've warned in the first `uv venv` invocation? \cc @zanieb)

---

_Comment by @tleonhardt on 2024-11-05 23:28_

I view this as 100% my own fault.

But if you could warn users to let them know about this potentially unanticipated behavior, that would be fantastic.

---

_Comment by @zanieb on 2024-11-05 23:36_

Thanks for the ping, yeah it seems like we should probably warn there if you're changing to virtual environment to something we won't use in future project invocations.

`uv run` should also say where it sourced that version from. I might have made that easier in #6370.


---

_Label `error messages` added by @zanieb on 2024-11-05 23:36_

---

_Assigned to @zanieb by @zanieb on 2024-11-05 23:36_

---

_Reopened by @zanieb on 2024-11-05 23:36_

---

_Renamed from "[Bug?] uv venv --python 3.12 creates a Python 3.13.0 virtual environment" to "Warn when `.python-version` conflicts with `uv venv -p <version>` in projects" by @zanieb on 2024-11-05 23:37_

---

_Comment by @Belval on 2025-05-08 19:28_

Any update on this? We encountered a situation where it delete a `.venv` that took some time to install...

---

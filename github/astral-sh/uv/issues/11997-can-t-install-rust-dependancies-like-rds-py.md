---
number: 11997
title: "can't install rust dependancies like rds.py"
type: issue
state: closed
author: dcpc007
labels:
  - question
assignees: []
created_at: 2025-03-06T07:09:03Z
updated_at: 2025-03-06T19:20:51Z
url: https://github.com/astral-sh/uv/issues/11997
synced_at: 2026-01-07T13:12:18-06:00
---

# can't install rust dependancies like rds.py

---

_Issue opened by @dcpc007 on 2025-03-06 07:09_

### Summary

Trying to install ansible 11.3, i got this error about a dependacies which relies on cargo and rust.

Is it normal ? 
What is the best way to solve that, and maybe improve the output if missing elements ?


Using Python 3.14.0a5 environment at: ansible-11.3-env
Resolved 16 packages in 77ms
  × Failed to build `rpds-py==0.23.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `maturin.build_wheel` failed (exit status: 1)

      [stdout]
      Running `maturin pep517 build-wheel -i /home/dchalon/.cache/uv/builds-v0/.tmpdDa0lD/bin/python --compatibility off`

      [stderr]
      💥 maturin failed
        Caused by: Cargo metadata failed. Do you have cargo in your PATH?
        Caused by: No such file or directory (os error 2)
      Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/home/dchalon/.cache/uv/builds-v0/.tmpdDa0lD/bin/python', '--compatibility', 'off'] returned non-zero exit status 1

      hint: This usually indicates a problem with the package or the build environment.
  help: `rpds-py` (v0.23.1) was included because `ansible-compat` (v25.1.4) depends on `jsonschema` (v4.23.0) which depends on `rpds-py`


### Platform

debian

### Version

12.9

### Python version

3.14

---

_Label `bug` added by @dcpc007 on 2025-03-06 07:09_

---

_Comment by @charliermarsh on 2025-03-06 13:59_

I would suggest using Python 3.13 or a version of Python that includes pre-built wheels for that package. As-is, you need to build that package from source, and doing so requires installing Rust.

---

_Label `bug` removed by @charliermarsh on 2025-03-06 13:59_

---

_Label `question` added by @charliermarsh on 2025-03-06 13:59_

---

_Comment by @charliermarsh on 2025-03-06 13:59_

(The starting point would be `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh` or similar.)

---

_Comment by @dcpc007 on 2025-03-06 17:28_

I installed rust with curl and it works. 
But a little boring those external dependencies not detected.
Because I need to manage this for quick deploy tools for all teams members when they need 
I write a readme with prerequisites at minima.

---

_Comment by @zanieb on 2025-03-06 19:20_

> But a little boring those external dependencies not detected.

This isn't really supported by the Python standards, so uv can't solve this for you. See https://peps.python.org/pep-0725/

I think there's been some discussion about having Maturin bootstrap Rust behind the scenes though.

---

_Closed by @charliermarsh on 2025-03-06 19:20_

---

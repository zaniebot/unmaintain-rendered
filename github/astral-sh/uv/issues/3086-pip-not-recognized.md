---
number: 3086
title: pip not recognized
type: issue
state: closed
author: dlasusa
labels: []
assignees: []
created_at: 2024-04-17T03:40:33Z
updated_at: 2024-12-30T05:25:54Z
url: https://github.com/astral-sh/uv/issues/3086
synced_at: 2026-01-07T13:12:17-06:00
---

# pip not recognized

---

_Issue opened by @dlasusa on 2024-04-17 03:40_

Not sure if this is a bug or expected behavior.

Environment:
Win11
Shell: Powershell
UV: 0.1.31 (7bcca28b1 2024-04-09)

When trying to run the standard `pip` command in a newly created `.venv` (created via uv) I get the following error:
```txt
pip: The term 'pip' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
```
Steps to reproduce (once inside project folder):
```ps1
uv venv
```
Output:
```
Using Python 3.12.2 interpreter at: C:\Python\Python312\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
```
Activate virtual environment
```
.venv\Scripts\activate
```
Since it's a new environment I expect `pip` to run, but return nothing.
```
uv pip list
```
No output as expected.

```
pip list
```
The error as listed above.

If I run the following (inside the activated environment):
```
python.exe -m pip list
```
I get the correct "no output"

Why do I still need `pip` ?

I often update my packages using `pip list --outdated` which isn't yet support in `uv` but it's [on it's way?](https://github.com/astral-sh/uv/issues/2150)

Also not a huge deal since there is a work around which isn't awful.  Just wasn't sure if it was expected behavior?  Or if I'm doing something incorrectly.



---

_Comment by @charliermarsh on 2024-04-17 03:41_

You may be looking for `uv venv --seed`. We don't include `pip` in the virtualenv by default, but you can opt-in to adding it with `--seed`.

---

_Comment by @dlasusa on 2024-04-17 04:19_

Perfect!  Thanks @charliermarsh I'm really loving uv (and ruff)! 

---

_Closed by @dlasusa on 2024-04-17 04:19_

---

_Comment by @charliermarsh on 2024-04-17 04:21_

Love to hear that!

---

_Comment by @youkaichao on 2024-12-30 03:59_

just running into this issue.

I think we need to at least document it, I don't see any doc recommending the `uv venv --seed` , and I have to search over issues to summarize and understand the situation :(

---

_Comment by @zanieb on 2024-12-30 04:44_

@youkaichao why do you need `pip` in the environment?

---

_Comment by @youkaichao on 2024-12-30 05:25_

I'm migrating to `uv`, but there are, of course, some legacy workflow using `pip` directly.

---

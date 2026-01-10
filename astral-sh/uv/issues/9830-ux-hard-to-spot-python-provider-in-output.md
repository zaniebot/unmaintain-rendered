---
number: 9830
title: "UX: hard to spot python provider in output"
type: issue
state: open
author: hwine
labels:
  - cli
assignees: []
created_at: 2024-12-12T01:47:29Z
updated_at: 2024-12-12T18:01:53Z
url: https://github.com/astral-sh/uv/issues/9830
synced_at: 2026-01-10T01:24:46Z
---

# UX: hard to spot python provider in output

---

_Issue opened by @hwine on 2024-12-12 01:47_

_[Filing per @zanieb request in [#9819 comment](https://github.com/astral-sh/uv/issues/9819#issuecomment-2536854906)]_ 

I'm a definite corner case configuration, so there may not be much that can be done. I have a MS Surface X (arm64 processor) running windows 11, and spend most of my python time in a Git-bash shell. The sequence of events that led to me uncovering the issue in #9819 is roughly:
1. upgrade `uv` to latest
2. see if there is yet an arm64 version of python available via `uv python list`
3. misread the output, and took a wrong guess at how to request installation
4. uncovered the #9819 issue

While an arm64 box can transparently run x86_64 binaries, there is a performance hit which is most noticeable during python startup. So I've been trying to figure out how to use `uv` with a native arm64 python. (That's been a challenge I haven't solved cleanly yet, but not high on my list.)

Today, step 2 above (the only command I knew at the time) produced the following output:
```shell
❯ uv python list
cpython-3.13.1+freethreaded-windows-x86_64-none    <download available>
cpython-3.13.1-windows-x86_64-none                 <download available>
cpython-3.13.0-windows-x86_64-none                 AppData\Roaming\uv\python\cpython-3.13.0-windows-x86_64-none\python.exe
cpython-3.13.0-windows-aarch64-none                AppData\Local\Programs\Python\Python313-arm64\python.exe
cpython-3.12.8-windows-x86_64-none                 <download available>
cpython-3.12.2-windows-x86_64-none                 AppData\Roaming\uv\python\cpython-3.12.2-windows-x86_64-none\python.exe
cpython-3.12.2-windows-aarch64-none                .pyenv\pyenv-win\versions\3.12.2-arm\python.exe
cpython-3.11.11-windows-x86_64-none                <download available>
cpython-3.11.10-windows-x86_64-none                AppData\Roaming\uv\python\cpython-3.11.10-windows-x86_64-none\python.exe
cpython-3.10.16-windows-x86_64-none                <download available>
cpython-3.10.15-windows-x86_64-none                AppData\Roaming\uv\python\cpython-3.10.15-windows-x86_64-none\python.exe
cpython-3.9.21-windows-x86_64-none                 <download available>
cpython-3.8.20-windows-x86_64-none                 <download available>
cpython-3.7.9-windows-x86_64-none                  <download available>
pypy-3.10.14-windows-x86_64-none                   <download available>
pypy-3.9.19-windows-x86_64-none                    <download available>
pypy-3.8.16-windows-x86_64-none                    <download available>
pypy-3.7.13-windows-x86_64-none                    <download available>
```
Focusing on the left column, I saw the "windows-aarch64-none" and got excited. I glanced to the right column and (wrongly) ended up on a "\<download available\>" line, and jumped to the incorrect conclusion that `uv` could now install arm64 python versions on windows. After @zanieb pointed me to the _correct_ command to use (`uv python list --only-downloads`) it was clear that windows arm64 builds are not yet available from `uv`.

What would help reduce the confusion, I think, would be some distinction in the listing between `uv` installed pythons, and all other. In my case, I have both a `pyenv` python and python.org python in the mix. Maybe using a different color for the non-`uv` ones in the right column (which are all blue for installed versions)? Or a prefix character on the left column (similar to `ls -l` output)?



---

_Comment by @zanieb on 2024-12-12 02:05_

Ah I thought you were saying `uv python list --only-downloads` was showing system interpreters. This is less surprising, but perhaps we can do better.

---

_Label `cli` added by @charliermarsh on 2024-12-12 14:21_

---

_Referenced in [astral-sh/uv#9841](../../astral-sh/uv/pulls/9841.md) on 2024-12-12 15:26_

---

_Comment by @zanieb on 2024-12-12 15:27_

What do you think of https://github.com/astral-sh/uv/pull/9841

---

_Comment by @hwine on 2024-12-12 18:01_

Oops - clicked the wrong link and answered [over here](https://github.com/astral-sh/uv/pull/9841#issuecomment-2539673465)

---

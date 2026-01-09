---
number: 10627
title: Build ruff on Alpine s390x
type: issue
state: closed
author: raspbeguy
labels:
  - help wanted
  - dependencies
assignees: []
created_at: 2024-03-27T12:33:29Z
updated_at: 2024-04-11T07:02:19Z
url: https://github.com/astral-sh/ruff/issues/10627
synced_at: 2026-01-07T13:12:15-06:00
---

# Build ruff on Alpine s390x

---

_Issue opened by @raspbeguy on 2024-03-27 12:33_

Hello, I wish to fix ruff package on Alpine Linux. Currently the package won't build on s390x arch because of a dependancy that won't update its own dependancies. I'm talking about clearscreen which uses an old version on nix which contains a bug for s390x. The bug has been patched on nix but clearscreen seems almost abandoned.

Here is the issue https://github.com/watchexec/clearscreen/pull/20

I don't know rust so I can't contribute, but anyway I think it would be wise to get rid of a dependancy in such state. 

---

_Comment by @MichaReiser on 2024-03-27 12:45_

> The bug has been patched on nix but clearscreen seems almost abandoned.

It seems that the package is still maintained. The maintainer commented on https://github.com/watchexec/clearscreen/pull/20 with that they're happy to upgrade the `nix` package once the PR builds successfully. 

That's why I think the best forward is to help push the upgrade PR forward rather than vendoring, forking or replacing 

An alternative is to use something like `crossterm` but its clear command but that comes with a lot of functionality that we don't need today.

---

_Label `help wanted` added by @MichaReiser on 2024-03-27 12:45_

---

_Label `dependencies` added by @MichaReiser on 2024-03-27 12:45_

---

_Renamed from "Get rid of clearscreen as a dependancy" to "Build ruff on Alpine s390x" by @MichaReiser on 2024-03-27 12:52_

---

_Comment by @charliermarsh on 2024-04-11 02:31_

I'll try to fix the upstream issue: https://github.com/watchexec/clearscreen/pull/21

---

_Referenced in [astral-sh/ruff#10869](../../astral-sh/ruff/pulls/10869.md) on 2024-04-11 04:19_

---

_Comment by @charliermarsh on 2024-04-11 04:20_

I was able to fix the issue in clearscreen, and the maintainer kindly cut a release. I've upgraded Ruff's version in https://github.com/astral-sh/ruff/pull/10869.

---

_Closed by @charliermarsh on 2024-04-11 04:41_

---

_Comment by @raspbeguy on 2024-04-11 07:02_

many thanks @charliermarsh 

---

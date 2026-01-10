```yaml
number: 7670
title: "packaged app: use executable named for the project and main function"
type: pull_request
state: merged
author: bluss
labels:
  - cli
assignees: []
merged: true
base: main
head: init-package
created_at: 2024-09-24T19:09:35Z
updated_at: 2024-09-30T23:29:40Z
url: https://github.com/astral-sh/uv/pull/7670
synced_at: 2026-01-10T12:53:52Z
```

# packaged app: use executable named for the project and main function

---

_Pull request opened by @bluss on 2024-09-24 19:09_

## Summary

Create a function main as the default for a packaged app. Configure the default executable as:

`example-packaged-app = "example_packaged_app:main"`

Which is often what you want - the executable has the same name as the app.
The purpose is to more often hit what the user wants, so they don't have
to even rename the command to start developing.

## Test Plan

- existing tests are updated


---

_@bluss reviewed on 2024-09-24 19:11_

---

_Review comment by @bluss on `crates/uv/tests/init.rs`:1426 on 2024-09-24 19:11_

Maybe I'm mistaken here, but I think this test shows that `uv init --virtual` is already creating something with a build system and executable, this is fishy.

---

_Renamed from "Use main as default function in packaged app" to "packaged app: use executable named for the project and function main" by @bluss on 2024-09-24 19:13_

---

_Renamed from "packaged app: use executable named for the project and function main" to "packaged app: use executable named for the project and main function" by @bluss on 2024-09-24 19:13_

---

_Comment by @bluss on 2024-09-24 19:39_

I asked @zanieb briefly in discord before posting this one. It's a small but "opinionated" change I guess. Next step could be to add a [minimal](https://docs.python.org/3/library/__main__.html#main-py-in-python-packages) `__main__.py`.

---

_@zanieb reviewed on 2024-09-24 19:40_

---

_Review comment by @zanieb on `crates/uv/tests/init.rs`:1426 on 2024-09-24 19:40_

Yeah this is fishy — `--virtual` is legacy now but shouldn't be doing this. Let's tackle it separately though.

---

_Review comment by @zanieb on `docs/concepts/projects.md`:250 on 2024-09-24 19:41_

This should be `:main` right?

---

_@zanieb reviewed on 2024-09-24 19:41_

---

_Assigned to @zanieb by @zanieb on 2024-09-24 19:43_

---

_@bluss reviewed on 2024-09-24 19:48_

---

_Review comment by @bluss on `docs/concepts/projects.md`:250 on 2024-09-24 19:48_

oops, fixed.

Also I'm very used to force pushing but shouldn't do that since you squash merge here anyway. I just haven't learned the lesson yet..

---

_@zanieb reviewed on 2024-09-24 20:04_

---

_Review comment by @zanieb on `docs/concepts/projects.md`:250 on 2024-09-24 20:04_

Oh we force push all the time, don't worry about that.

---

_Comment by @zanieb on 2024-09-27 13:30_

I'm off today but I'll try to get this in early next week.

---

_@zanieb approved on 2024-09-30 22:19_

---

_Merged by @zanieb on 2024-09-30 22:19_

---

_Closed by @zanieb on 2024-09-30 22:19_

---

_Label `cli` added by @zanieb on 2024-09-30 22:19_

---

_Branch deleted on 2024-09-30 23:29_

---

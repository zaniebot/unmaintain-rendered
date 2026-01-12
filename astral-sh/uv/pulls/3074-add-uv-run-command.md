```yaml
number: 3074
title: "Add `uv run` command"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cli
assignees: []
merged: true
base: main
head: zb/uv-run
created_at: 2024-04-16T20:50:38Z
updated_at: 2024-04-17T16:20:44Z
url: https://github.com/astral-sh/uv/pull/3074
synced_at: 2026-01-12T16:05:24Z
```

# Add `uv run` command

---

_@zanieb_

Adds `uv run` which executes a command in your current virtual environment.

This is a simple first milestone, lots of remaining work and behavior. The command is hidden.

---

_Label `internal` added by @zanieb on 2024-04-16 20:50_

---

_Label `cli` added by @zanieb on 2024-04-16 20:50_

---

_Marked ready for review by @zanieb on 2024-04-16 23:15_

---

_Review comment by @konstin on `crates/uv/src/cli.rs`:1315 on 2024-04-17 09:34_

This needs some explanation what command means and some examples, esp. compared to
```
usage: python [option] ... [-c cmd | -m mod | file | -] [arg] ...
```

---

_Review comment by @konstin on `crates/uv/src/commands/run.rs`:53 on 2024-04-17 09:35_

Can you use `.status()` here?

---

_Review comment by @konstin on `crates/uv/src/commands/run.rs`:57 on 2024-04-17 09:42_

This is important for scripts that check the output status of the python code (e.g. exit status 1 vs exit status 2)

---

_@konstin approved on 2024-04-17 09:43_

---

_Comment by @konstin on 2024-04-17 09:43_

Some things i learned i'm gonna dump here:

You can load dlopen libpython and use the c api to load libpython, and run `Py_Main`. This is more efficient and has a better integration than a subprocess, but it requires that uv and python have the same libc, which we can't assume.

Again on unix, you use `execv` to have the current process been taken over by a new one (instead of nested and handling subprocess status). This is apparently not supported on windows, where you always spawn a new process (https://github.com/konstin/poc-monotrail/blob/810f7e1ff43c1f95c96d318691241225352357d1/crates/monotrail/src/monotrail.rs#L759-L794)

---

_@zanieb reviewed on 2024-04-17 14:22_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1315 on 2024-04-17 14:22_

I'm going to reserve the real documentation for when it's a little more nailed down what the interface is. I think I extend these a bit in the next pull request though.

---

_@zanieb reviewed on 2024-04-17 14:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:53 on 2024-04-17 14:22_

I could switch yeah, I thought I might need to do something while it's running but perhaps not?

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:57 on 2024-04-17 14:22_

Yeah agree that's just not built into our exit system so I want to do it separately

---

_@zanieb reviewed on 2024-04-17 14:22_

---

_Comment by @zanieb on 2024-04-17 14:24_

Thanks for the notes! Do you see a strong benefit to doing `execv` I feel like we might _want_ to be able to take action after the command? I'll include a note about that somewhere regardless as it does seem kind of nice.

---

_Comment by @konstin on 2024-04-17 14:53_

Not really, `execv` just seemed more elegant

---

_Merged by @zanieb on 2024-04-17 16:20_

---

_Closed by @zanieb on 2024-04-17 16:20_

---

_Branch deleted on 2024-04-17 16:20_

---

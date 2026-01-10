---
number: 7805
title: "no error on `uv sync --upgrade-package` when specifying a package that isn't included in the project's dependencies"
type: issue
state: open
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2024-09-30T07:05:48Z
updated_at: 2025-07-01T01:26:17Z
url: https://github.com/astral-sh/uv/issues/7805
synced_at: 2026-01-10T01:24:19Z
---

# no error on `uv sync --upgrade-package` when specifying a package that isn't included in the project's dependencies

---

_Issue opened by @DetachHead on 2024-09-30 07:05_

when running `uv sync --upgrade-package` with a package that is not present in the project's dependencies (or a transient dependency in the lockfile), uv should report an error
```
> uv sync --upgrade-package asdfasdf
Resolved 139 packages in 2.06s
Audited 139 packages in 1ms
```

---

_Comment by @bluss on 2024-09-30 09:20_

I wanted to report a similar bug for `uv sync --no-build-isolation-package foo`, so I'll just mention it here. Same issue, uv could report at least a warning for this.

---

_Comment by @charliermarsh on 2024-09-30 12:48_

I think we can do these.

---

_Label `bug` added by @charliermarsh on 2024-09-30 12:48_

---

_Comment by @bluss on 2024-10-01 19:26_

I found an unintentional feature, I'll share for the fun of it. 

`uv lock -P notexist` will update the lockfile with trivial enforced version updates such as local path dependencies and the own workspace.

---

_Comment by @charliermarsh on 2024-11-23 03:47_

My only concern is that this is technically a breaking change, since you could in theory rely on this (e.g., maybe someone _always_ passes `--upgrade-package foo` to upgrade regardless of whether `foo` is in the tree).

---

_Comment by @DetachHead on 2024-11-23 06:27_

imo it's far more likely that it's a mistake most of the time. when tools silently ignore things like this it allows mistakes to go undetected potentially causing even bigger problems in the future. i guess if people rely on it there can be an option to disable the error which can be suggested to the user in the error message.

i strongly believe it's always better to show the user an error to make sure they're aware of a potential problem and giving them the option to suppress instead of just silently ignoring it just on the off chance that it's intentional.

---

_Comment by @qiuxiaomu on 2025-05-16 13:05_

I would also like to raise that when we do `uv sync --upgrade` or `uv lock --upgrade`, the upgrade is silently done and no message is present to user to signal which packages are (not) upgraded. I thought that the command didn't work but when I check the `uv.lock` I read that the packages were upgraded (checking them one by one could also be troublesome). 



---

_Referenced in [astral-sh/uv#14370](../../astral-sh/uv/issues/14370.md) on 2025-06-30 23:41_

---

_Comment by @johnthagen on 2025-07-01 01:26_

Wanted to explicitly list that this same issue exists for `uv lock --upgrade-package` as well since we've merged #14370 into this issue.

---

```yaml
number: 12672
title: "Unset `SCRIPT_PATH` in relocatable activation script"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2025-04-04T12:53:57Z
updated_at: 2025-04-07T18:11:49Z
url: https://github.com/astral-sh/uv/pull/12672
synced_at: 2026-01-10T11:10:40Z
```

# Unset `SCRIPT_PATH` in relocatable activation script

---

_Pull request opened by @charliermarsh on 2025-04-04 12:53_

## Summary

Restore it if it was previously set, etc.

Closes https://github.com/astral-sh/uv/issues/12662.


---

_Label `breaking` added by @charliermarsh on 2025-04-04 12:54_

---

_Comment by @charliermarsh on 2025-04-04 12:54_

This _might_ be breaking? I have to trace it...

---

_Label `breaking` removed by @charliermarsh on 2025-04-04 13:00_

---

_Label `enhancement` added by @charliermarsh on 2025-04-04 13:00_

---

_Comment by @charliermarsh on 2025-04-04 13:01_

Okay, I think this _isn't_ breaking because it's all confined to the activation script. So existing scripts will continue to work, and new scripts will use the new variables consistently.

---

_@zsimic reviewed on 2025-04-04 18:00_

---

_Review comment by @zsimic on `crates/uv-virtualenv/src/activator/activate`:27 on 2025-04-04 18:00_

I'm not sure how the variable is actually used, but I think "for good measure", you should also unset it in `deactivate()` function, as in `unset _UV_VENV_ACTIVATOR`.

Otherwise, a de-activated venv could fool the Rust code below, it could make things look as if the venv is still active (based on the existence and value of the shell variable)

---

_Review requested from @Gankra by @zanieb on 2025-04-04 23:18_

---

_Review requested from @konstin by @zanieb on 2025-04-04 23:18_

---

_Comment by @konstin on 2025-04-07 12:39_

Can we do a `unset SCRIPT_PATH` at the end of the script? I could be missing something, but grepping for `SCRIPT_PATH` I didn't find any usages outside of the script or any delayed evaluation.

---

_Comment by @charliermarsh on 2025-04-07 13:04_

Yeah, I think we probably can unset it. I will try.

---

_Review requested from @zsimic by @charliermarsh on 2025-04-07 14:24_

---

_Renamed from "Use `_UV_VENV_ACTIVATOR` instead of `SCRIPT_PATH` in relocatable activation script" to "Unset `SCRIPT_PATH` in relocatable activation script" by @charliermarsh on 2025-04-07 14:25_

---

_Comment by @charliermarsh on 2025-04-07 14:25_

Okay, using @zsimic script, I think this just sets the var locally, then unsets and restores it.

---

_Comment by @konstin on 2025-04-07 17:48_

In case anyone else is wondering about env variable vs. local variable: bash (at least) is using a local variable if it isn't defined as an env var, and overrides the env var otherwise:

```bash
$ env | grep VAR
$ source a.sh 
$ env | grep VAR
VAR_A2=bar
$ export VAR_A1="value"
$ env | grep VAR
VAR_A1=value
VAR_A2=bar
$ source a.sh 
$ env | grep VAR
VAR_A1=foo
VAR_A2=bar
```

---

_@konstin approved on 2025-04-07 17:48_

---

_Merged by @zanieb on 2025-04-07 18:11_

---

_Closed by @zanieb on 2025-04-07 18:11_

---

_Branch deleted on 2025-04-07 18:11_

---

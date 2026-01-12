```yaml
number: 13254
title: uv version leaves lock file out-of-date
type: issue
state: closed
author: StephenRobin
labels:
  - bug
assignees: []
created_at: 2025-05-01T17:57:30Z
updated_at: 2025-05-21T13:46:10Z
url: https://github.com/astral-sh/uv/issues/13254
synced_at: 2026-01-12T16:01:23Z
```

# uv version leaves lock file out-of-date

---

_@StephenRobin_

### Summary

Thanks for adding the `uv version` command! I'm not sure if this is a bug or not, but it feels strange that the version number in uv.lock is not updated by `uv version`.

We currently have a dynamic version number in pyproject.toml. In the CI pipeline we set the version of the build in an env var, then do:

```
uv sync --locked # locked to ensure the build is reproduceable and pyproject.toml matches uv.lock
uv run --no-sync ruff check
# Some other stuff
uv build
# Some other stuff
```

I would like to change this so we just set the version number in pyproject.toml at the start of the CI run using `uv version 1.2.3`. This doesn't work as the subsequent `uv sync --locked` step gives the error "The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`".

The error makes sense, but wouldn't it be better to either update the lock file as part of `uv version` or perhaps even omit the version from uv.lock like when dynamic versions are used?

I could solve this by doing `uv version 1.2.3 && uv lock` after the sync (some of our later stages require it to be locked) but it feels wrong.

### Platform

Ubuntu 22

### Version

uv 0.7.2

### Python version

_No response_

---

_Label `bug` added by @StephenRobin on 2025-05-01 17:57_

---

_Assigned to @Gankra by @zanieb on 2025-05-01 18:16_

---

_Comment by @Gankra on 2025-05-01 18:28_

Yep good call, we should definitely do this.

---

_Comment by @HenriBlacksmith on 2025-05-05 15:25_

> Yep good call, we should definitely do this.

I think this is even breaking the `uv version` command, not sure if there is a priority for bugs but this one is definitely an important one

---

_Comment by @albireox on 2025-05-06 15:22_

Additionally, it seems that `lock` and `sync` do something slightly different. If I do a `lock` after a `version` update the lock file gets updated, but then if I later do a `sync` I get a message saying that the package version is updated. The latter only happens once and doesn't seem to change the lock file, but it's unclear why it does that if all the files are already up to date.

```bash
$ uv version 0.3.13a0
lvmcryo 0.3.12 => 0.3.13a0

$ uv lock
Resolved 98 packages in 1.77s
Updated lvmcryo v0.3.12 -> v0.3.13a0

$ uv sync --all-groups --all-extras
Resolved 98 packages in 8ms
      Built lvmcryo @ file:///home/gallegoj/code/lvmcryo
Prepared 1 package in 530ms
Uninstalled 1 package in 3ms
Installed 1 package in 1ms
 - lvmcryo==0.3.12a0 (from file:///home/gallegoj/code/lvmcryo)
 + lvmcryo==0.3.13a0 (from file:///home/gallegoj/code/lvmcryo)
```

---

_Comment by @Gankra on 2025-05-06 19:08_

> I think this is even breaking the uv version command, not sure if there is a priority for bugs but this one is definitely an important one

Good news the fix is written and will hopefully land soon (it's unfortunately a bit of a non-trivial change, in spite of the net effect usually being "edit this one string in a toml file").

---

_Closed by @Gankra on 2025-05-21 13:46_

---

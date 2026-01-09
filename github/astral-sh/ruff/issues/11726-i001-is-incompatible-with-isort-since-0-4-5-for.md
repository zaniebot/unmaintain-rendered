---
number: 11726
title: "I001 is incompatible with ``isort`` since 0.4.5 for builtins starting with an underscore"
type: issue
state: closed
author: DanielNoord
labels:
  - isort
assignees: []
created_at: 2024-06-03T19:57:31Z
updated_at: 2024-06-08T20:10:52Z
url: https://github.com/astral-sh/ruff/issues/11726
synced_at: 2026-01-07T13:12:15-06:00
---

# I001 is incompatible with ``isort`` since 0.4.5 for builtins starting with an underscore

---

_Issue opened by @DanielNoord on 2024-06-03 19:57_

Hi, when bumping from `0.4.4` to `0.4.5` in `pylint` we saw that I001 is no longer compatible with `isort`. See: https://github.com/pylint-dev/pylint/pull/9679

I also noticed at my day job and other repositories.

You've probably improved here since `_string` is indeed a builtin and should be in the builtins block. However, this makes `ruff` incompatible with `isort` and by extension `pylint` which uses `isort` under the hood.
I didn't see this in the release notes and think such a breaking change should at least be acknowledged.

I'll leave it up to you whether this needs to be reverted

---

_Referenced in [pylint-dev/pylint#9679](../../pylint-dev/pylint/pulls/9679.md) on 2024-06-03 19:57_

---

_Comment by @charliermarsh on 2024-06-08 19:51_

Thanks, and sorry for the trouble. We moved to `stdlibs` (#11347) which has better coverage of the standard library, and this led to a few new modules being added. I agree that we should document it (so I'll use this issue to do so) and, in hindsight, probably should've sent this out in a minor release -- that's my fault.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-08 19:51_

---

_Label `isort` added by @charliermarsh on 2024-06-08 19:51_

---

_Referenced in [astral-sh/ruff#11804](../../astral-sh/ruff/pulls/11804.md) on 2024-06-08 20:07_

---

_Closed by @charliermarsh on 2024-06-08 20:10_

---

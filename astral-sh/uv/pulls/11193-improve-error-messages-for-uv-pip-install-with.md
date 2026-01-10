```yaml
number: 11193
title: "Improve error messages for `uv pip install` with `--extra` or `--all-extras` and invalid sources"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/pip-inst-err
created_at: 2025-02-03T19:44:55Z
updated_at: 2025-02-03T22:12:40Z
url: https://github.com/astral-sh/uv/pull/11193
synced_at: 2026-01-10T11:10:34Z
```

# Improve error messages for `uv pip install` with `--extra` or `--all-extras` and invalid sources

---

_Pull request opened by @zanieb on 2025-02-03 19:44_

Closes https://github.com/astral-sh/uv/issues/11190
Closes https://github.com/astral-sh/uv/issues/7845

This error message was copied over from `uv pip compile` (presumably) but makes way more sense there than here.

---

_Label `error messages` added by @zanieb on 2025-02-03 19:44_

---

_@zanieb reviewed on 2025-02-03 19:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:71 on 2025-02-03 19:46_

Not in love, but... what am I to do?

---

_Comment by @charliermarsh on 2025-02-03 19:55_

Part of me wonders if we should... allow these for `.` etc.?

---

_Comment by @zanieb on 2025-02-03 20:01_

I was also thinking that. It.. wouldn't be unreasonable. Just another way to do the same thing which could be confusing.

---

_Comment by @notatallshaw-gts on 2025-02-03 20:37_

Maybe also fixes https://github.com/astral-sh/uv/issues/7845 ? (Sorry I never remembered to file a doc PR for this)

---

_Marked ready for review by @zanieb on 2025-02-03 20:49_

---

_@Gankra reviewed on 2025-02-03 21:17_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/operations.rs`:71 on 2025-02-03 21:17_

Wait really, that works but these alt flags don't?

---

_@zanieb reviewed on 2025-02-03 21:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:71 on 2025-02-03 21:20_

Yeah...

So.. `pip install package[extra]` is what's supported upstream. Downstream, we added support for `pip install -r pyproject.toml` but there's no syntax to select extras there so we have `--extra` and `--all-extras`. These are also copied over from `uv pip compile pyproject.toml` where they make a little more sense. `uv pip compile .` doesn't exist.

---

_@charliermarsh approved on 2025-02-03 22:07_

---

_@Gankra approved on 2025-02-03 22:08_

Only thought is "it'd be nice if we could "rewrite" the cli command they passed" (or at least substitute the extras into the syntax) but that's a huge can of worms and this is better than nothing for sure.

---

_Comment by @zanieb on 2025-02-03 22:12_

Yeah I would be way more down to do that if it weren't an iterator of arbitrary length. Maybe worth doing for singletons as a follow-up? Going to merge as an incremental improvement.

---

_Merged by @zanieb on 2025-02-03 22:12_

---

_Closed by @zanieb on 2025-02-03 22:12_

---

_Branch deleted on 2025-02-03 22:12_

---

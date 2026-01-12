```yaml
number: 14113
title: "Allow `[project]` to be missing from a `pyproject.toml`"
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/unleash-virt
created_at: 2025-06-17T17:39:58Z
updated_at: 2025-09-17T15:48:58Z
url: https://github.com/astral-sh/uv/pull/14113
synced_at: 2026-01-12T16:11:02Z
```

# Allow `[project]` to be missing from a `pyproject.toml`

---

_@Gankra_

Closes #8666 
Closes https://github.com/astral-sh/uv/issues/6838

---

_@Gankra reviewed on 2025-06-17 17:42_

---

_Review comment by @Gankra on `crates/uv-workspace/src/workspace.rs`:1438 on 2025-06-17 17:42_

This first commit has an unserious impl because I just want to check in CI if there's any other tests this trips over.

---

_@Gankra reviewed on 2025-06-17 17:44_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/run.rs`:507 on 2025-06-17 17:44_

This is the more serious/targeted approach.

---

_@zanieb reviewed on 2025-06-17 17:57_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:3342 on 2025-06-17 17:57_

We should probably hide this warning when `[project]` is missing entirely, right?

---

_@zanieb reviewed on 2025-06-17 17:57_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:3085 on 2025-06-17 17:57_

Reminder

---

_Comment by @Gankra on 2025-06-17 18:03_

The difference between the "do it for everything" and "precisely do this just for run" impls are:

* ✅ `uv export` ...maybe makes sense with a lock for only dependency-groups??
* ✅ `uv sync` ...i suppose makes sense because you could sync only dependency-groups?
* ✅ `uv python find` ...not clearly useful to me but it has a --no-project flag
* ✅ `uv python pin` ...not clearly useful to me but it has a --no-project flag
* ✅ `uv venv` ...maybe makes sense? we use the pyproject.toml's location as a way to find where the `.venv`, i think?
* ✅ `uv add` only makes sense with dependency-groups, which the code already explicitly supports/checks!
* ✅ `uv remove` only makes sense with dependency-groups, which the code already explicitly supports/checks!
* ❌ `uv version` ...doesn't make sense! `uv version` probably shouldn't be using VirtualProject as much as it does! mea culpa... but really it's potato-potatoh since we already have checks against missing `[project]` in this command, so it just changes the error message to something more useful?

---

_@Gankra reviewed on 2025-06-17 18:08_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:3342 on 2025-06-17 18:08_

I'm a bit of two minds because I think we still kinda want to discourage this usecase even if we support it..?

---

_Comment by @Gankra on 2025-06-17 19:14_

I've pushed up a series of commits -- the first adds tests for how each command behaves with this kind of pyproject.toml *today*, and the every subsequent commit migrates a specific command to use discover_defaulted, allowing the code to support that situation explicitly.

---

_Comment by @Gankra on 2025-06-17 19:15_

Notably `uv python find`, `uv python pin` and `uv venv` resulted in no snapshot changes so my tests were probably not doing the right thing (probably running the command in a subdir would do something more interesting?).

---

_Comment by @Gankra on 2025-06-17 19:19_

~~The churn on `python_find` is interesting...~~
nevermind, copy-paste error, not a thing

---

_Assigned to @zanieb by @zanieb on 2025-06-18 23:40_

---

_Comment by @karanr-invent on 2025-08-20 10:21_

Any update on this ?

---

_Comment by @zanieb on 2025-08-20 23:07_

It's on my immense todo list.

---

_Comment by @karanr-invent on 2025-08-21 04:43_

> It's on my immense todo list.

Thanks for all the great work, really appreciate it 

---

_Renamed from "Allow project-less pyprojects in more places" to "Allow `[project]` to be missing from a `pyproject.toml`" by @zanieb on 2025-09-17 14:34_

---

_Merged by @zanieb on 2025-09-17 15:48_

---

_Closed by @zanieb on 2025-09-17 15:48_

---

_Branch deleted on 2025-09-17 15:48_

---

```yaml
number: 14225
title: use $TMPDIR/uv-locks instead of putting lockfiles directly in $TMPDIR
type: pull_request
state: closed
author: oconnor663
labels: []
assignees: []
base: jack/lock_wheel_builds
head: jack/uv_lock_dir
created_at: 2025-06-23T22:06:51Z
updated_at: 2025-06-25T18:05:52Z
url: https://github.com/astral-sh/uv/pull/14225
synced_at: 2026-01-10T11:10:43Z
```

# use $TMPDIR/uv-locks instead of putting lockfiles directly in $TMPDIR

---

_Pull request opened by @oconnor663 on 2025-06-23 22:06_

As @zanieb has pointed out in chat, moving file locks around makes us vulnerable to races between different versions of `uv` that don't agree on the file lock path. I'm comfortable ignoring that as rare enough not to matter, but I'm curious what other folks think.

Atomically creating a directory with universal write permissions also turned out to be surprisingly complicated, requiring us to spawn a shell command that first sets `umask`. My instinct is that this command will be portable to all flavors of Unix, but if we'd rather skip shelling out and tolerate the extremely brief mkdir-then-chmod race, that could also make sense. Feedback needed.

This PR sits on top of https://github.com/astral-sh/uv/pull/14174.

---

_Comment by @oconnor663 on 2025-06-25 18:05_

I'm closing this for now. Ad-hoc proxy file locking is done in a lot of different ways, e.g.: https://github.com/astral-sh/uv/blob/4b348512c270d4fc45d4c5ab55df0bae4e2d93cd/crates/uv/src/commands/project/mod.rs#L721-L746

It might be nice to refactor that (and `interpreter.rs` and others) to use a common abstraction, but playing with that made it clear that it's going to grow the scope of https://github.com/astral-sh/uv/pull/14174 more than I'd prefer.

---

_Closed by @oconnor663 on 2025-06-25 18:05_

---

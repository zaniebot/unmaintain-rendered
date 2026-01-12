```yaml
number: 11643
title: "Support conflict markers in `uv export`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2025-02-19T22:29:28Z
updated_at: 2025-02-21T12:10:38Z
url: https://github.com/astral-sh/uv/pull/11643
synced_at: 2026-01-12T16:09:56Z
```

# Support conflict markers in `uv export`

---

_@charliermarsh_

## Summary

Today, if you have a lockfile that includes conflict markers, we write those markers out to `requirements.txt` in `uv export`. This is problematic, since no tool will ever evaluate those markers correctly downstream.

This PR adds handling for the conflict markers, though it's quite involved. Specifically, we have a new reachability algorithm that tracks, for each node, the reachable marker for that node _and_ the marker conditions under which each conflict item is `true` (at that node).

I'm slightly worried that this algorithm could be wrong for graphs with cycles, but we only use this logic for lockfiles with conflicts anyway, so I think it's a strict improvement over the status quo.

Closes https://github.com/astral-sh/uv/issues/11559.

Closes https://github.com/astral-sh/uv/issues/11548.


---

_Label `bug` added by @charliermarsh on 2025-02-19 22:29_

---

_Review requested from @BurntSushi by @charliermarsh on 2025-02-19 22:29_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/requirements_txt.rs`:55 on 2025-02-20 15:19_

It looks like `MarkerTree::TRUE` is the only thing that's ever stored as a value in this particular map? Or am I wrong about that? If so, I think you could use a `FxHashSet` here instead.

---

_@BurntSushi approved on 2025-02-20 15:26_

This LGTM. The comments in the code (especially the examples) were actually incredibly helpful for being able to follow what was going on.

One question I had here that I'm a little fuzzy on is: why is it okay to completely remove conflict markers from the output of `uv export`, but we can't do the same for `uv.lock`?

---

_Comment by @charliermarsh on 2025-02-20 20:19_

> One question I had here that I'm a little fuzzy on is: why is it okay to completely remove conflict markers from the output of uv export, but we can't do the same for uv.lock?

I think the difference is: for `uv export`, we know the set of user-enabled extras and groups (whereas we don't for the lockfile). Like, `uv export` is invoked via `uv export --extra cpu`. The output isn't intended to work for arbitrary extras and groups; the set of enabled extras and groups is encoded at the time the user runs the command, unlike with the lockfile.

---

_@charliermarsh reviewed on 2025-02-20 20:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/requirements_txt.rs`:55 on 2025-02-20 20:19_

We end up cloning it when doing the `conflict_marker_reachability` thing, and then the markers can later vary, so this felt a bit easier.

---

_Merged by @charliermarsh on 2025-02-20 20:19_

---

_Closed by @charliermarsh on 2025-02-20 20:19_

---

_Branch deleted on 2025-02-20 20:19_

---

_Comment by @BurntSushi on 2025-02-21 12:10_

> The output isn't intended to work for arbitrary extras and groups

Ah! That's what I was missing. Thanks for explaining!

---

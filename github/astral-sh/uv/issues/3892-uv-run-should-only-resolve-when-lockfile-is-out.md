---
number: 3892
title: "`uv run` should only resolve when lockfile is out-of-sync"
type: issue
state: closed
author: charliermarsh
labels:
  - performance
  - preview
assignees: []
created_at: 2024-05-28T23:07:29Z
updated_at: 2024-07-12T22:49:29Z
url: https://github.com/astral-sh/uv/issues/3892
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv run` should only resolve when lockfile is out-of-sync

---

_Issue opened by @charliermarsh on 2024-05-28 23:07_

As long as the lockfile satisfies the requirements, we should accept it (even if dependencies are outdated). Updating the lockfile should be done via `uv update` or `uv lock` or comparable.


---

_Label `preview` added by @charliermarsh on 2024-05-28 23:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 23:09_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-06-01 00:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-08 20:20_

---

_Comment by @charliermarsh on 2024-06-09 00:22_

I'm looking into this now.

---

_Comment by @charliermarsh on 2024-06-10 02:58_

There are two parts to this:

1. Statically extracting the requirements for the workspace.
2. Comparing the extracted requirements to the lockfile.

I am working on (1). It's not totally trivial, we currently defer that extraction to the resolver.

There are several things that make (2) hard:

1. The markers might've been normalized when writing to the lockfile, so they may not match the requirements as defined by the user. We could also have multiple requirements from (1) that map to a single requirement in (2), if it's (e.g.) a single requirement repeated with different markers (which would get OR'd when writing to the lockfile).
2. We have to check workspace dependencies recursively.
3. We need to ensure that there are no entries in the lockfile that no longer exist as requirements -- otherwise, we'll install too many packages. So it's not just checking that each requirement is present; we also need to check that nothing is present that isn't a requirement. We need to be able to map from requirement to lock entry, but also the other way around.


---

_Unassigned @charliermarsh by @charliermarsh on 2024-06-10 13:27_

---

_Comment by @charliermarsh on 2024-06-10 13:43_

@ibraheemdev and I discussed this in Discord. A summary:

- We think that the right way to solve this is to enable the resolver to read from the lockfile directly. Then, this could be reframed as: can we solve the graph with _just_ the lockfile? (Overlaps heavily with https://github.com/astral-sh/uv/issues/3925.) If we can implement resolution atop the lockfile, all of the challenges that I described above go away.
- Separately, we may want to read from the lockfile directly during "normal" resolution, but to implement this ticket, we could just do a first-pass where we try to solve from _only_ the lockfile. So there could be additional follow-up work for #3925 later.
- Another idea we discussed was: write a hash or checksum of the requirements somewhere in the cache, and avoid re-resolving if they line up with the lockfile. This would be conceptually similar to writing a hash to the lockfile to capture its inputs, but we would write it out-of-band (to the cache) to avoid causing merge conflicts, etc. (This would only work locally after performing at least one resolution.) But we weren't sure if this would be necessary once we do the "resolve from the lockfile" idea. Either way, we'd need to read the lockfile from disk; and either way, we'd need to then validate that the virtual environment satisfies the requirements (unless we used the hash / checksum to skip this too, which could work, but risks getting out-of-sync when the user modifies the environment manually). So we opted _not_ do to that for now, and to instead focus on the harder thing.


---

_Comment by @charliermarsh on 2024-06-10 15:55_

There's something confusing in https://github.com/astral-sh/uv/issues/3925 though... In the lockfile, we don't actually store the version _constraints_, we just store the locked version. That might be fine? But if we translate that back to a requirement, we will have to translate to an `==` requirement. I haven't thought through the implications of that.

---

_Label `performance` added by @ibraheemdev on 2024-06-19 20:36_

---

_Referenced in [astral-sh/uv#4495](../../astral-sh/uv/pulls/4495.md) on 2024-06-24 22:50_

---

_Comment by @konstin on 2024-07-07 18:16_

To add more motivation here: When running `uv lock` transformers with all extras with a fitting lock, we take 100ms on main, but 60ms with https://github.com/astral-sh/uv/pull/4495.

One thing we lose by doing this is the check for yanked releases, those versions that are part of the lock file but have yanked since.

---

_Assigned to @ibraheemdev by @ibraheemdev on 2024-07-09 18:44_

---

_Closed by @charliermarsh on 2024-07-12 22:49_

---

_Closed by @charliermarsh on 2024-07-12 22:49_

---

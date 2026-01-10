```yaml
number: 13701
title: "Add an `ArchMode` to filter displayed architectures in `uv python list`"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/arch-mode
created_at: 2025-05-28T14:04:21Z
updated_at: 2025-05-29T21:57:23Z
url: https://github.com/astral-sh/uv/pull/13701
synced_at: 2026-01-10T11:10:42Z
```

# Add an `ArchMode` to filter displayed architectures in `uv python list`

---

_Pull request opened by @zanieb on 2025-05-28 14:04_

I'm trying to unblock 

- https://github.com/astral-sh/uv/pull/13722
- https://github.com/astral-sh/uv/pull/13475

which currently fail due to showing more downloads in `uv python list` and selecting non-native installs during `uv python install`. I don't really want to show non-native downloads without opt-in (it'll just be too noisy), and we should prefer native installs, so this pull request adds a change to apply some filtering. This should have no effect on the existing behavior, but will resolve the failing CI in those pull requests.

Note, the modes do not yet capture the semantics we'll need for arm64 Windows, but I'll try to figure that out afterwards.

This is a little forward-looking, in that it adds support for filtering based on architecture variants as needed for

- #9788 

That pull request is currently blocked on allowing the user to express they only want to use `x86-64-v1` even if they have a `v3` compatible machine. I expect the spelling of the options for "best" and "compatible" native architectures to change a little.

I don't make this user-facing here, but we probably will in a subsequent change.

---

_Label `internal` added by @zanieb on 2025-05-28 14:04_

---

_Review requested from @Gankra by @zanieb on 2025-05-28 14:53_

---

_Assigned to @Gankra by @zanieb on 2025-05-28 14:53_

---

_Converted to draft by @zanieb on 2025-05-28 16:26_

---

_Comment by @zanieb on 2025-05-28 16:27_

Eek, this is actually quite hard.

---

_Comment by @zanieb on 2025-05-28 19:19_

The ordering actually needs to change, per https://github.com/astral-sh/uv/pull/13474#discussion_r2112586405, see https://github.com/astral-sh/uv/pull/13709

---

_Comment by @zanieb on 2025-05-29 18:00_

Need to make sure this reconciles with https://github.com/astral-sh/uv/pull/13719

---

_Comment by @Gankra on 2025-05-29 19:27_

Hmm in my heart we want to be internally producing a 2d list of pythons:

* cpython-3.13.3
  * windows x64
  * windows arm64
* cpython-3.13.2
  * windows x64
  * windows arm64
  
And then unless the user asks we only show the top entry in each one. I'm not sure if that would be too disruptive architecturally.

---

_Comment by @Gankra on 2025-05-29 19:48_

Another subtlety: if the user *does* have a copy of python installed that is compatible but for whatever reason not preferred, we should probably still show it in `uv python list`, even though we'd otherwise not want to?

---

_Comment by @zanieb on 2025-05-29 19:56_

>  if the user does have a copy of python installed that is compatible but for whatever reason not preferred, we should probably still show it in uv python list, even though we'd otherwise not want to?

We will! This is just for listing available downloads and selection of downloads for installation.

---

_Comment by @zanieb on 2025-05-29 19:57_

> And then unless the user asks we only show the top entry in each one. I'm not sure if that would be too disruptive architecturally.

I think the arm Windows case is a bit of a separate concern. It's also plausible people will build wheels for older CPython versions on arm once it's commonplace, so it's not clear if we'll need that matrix yet.

---

_Comment by @Gankra on 2025-05-29 20:07_

Right I've been confused why the tests were failing in those other PRs given we already do a ton of special filtering, and I think I just realized you might just be working around a bug in the filtering at the end of list?

---

_Comment by @Gankra on 2025-05-29 20:29_

Put up an alternative here: https://github.com/astral-sh/uv/pull/13721

---

_Comment by @zanieb on 2025-05-29 21:57_

The alternative was convincing for now.

---

_Closed by @zanieb on 2025-05-29 21:57_

---

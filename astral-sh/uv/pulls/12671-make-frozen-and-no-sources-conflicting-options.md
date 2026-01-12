```yaml
number: 12671
title: "Make `--frozen` and `--no-sources` conflicting options"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - breaking
assignees: []
merged: true
base: release/070
head: charlie/err
created_at: 2025-04-04T12:51:09Z
updated_at: 2025-04-24T20:31:26Z
url: https://github.com/astral-sh/uv/pull/12671
synced_at: 2026-01-12T16:10:20Z
```

# Make `--frozen` and `--no-sources` conflicting options

---

_@charliermarsh_

## Summary

Alternatively, we could just warn.

Closes https://github.com/astral-sh/uv/issues/12653.


---

_Review requested from @zanieb by @charliermarsh on 2025-04-04 12:51_

---

_Label `error messages` added by @charliermarsh on 2025-04-04 12:51_

---

_Comment by @zanieb on 2025-04-04 16:28_

What about other things, like `--index` and `--frozen`?

---

_@zanieb approved on 2025-04-04 16:28_

---

_Comment by @zanieb on 2025-04-04 16:29_

Seems like an improvement, should we consider this breaking?

---

_Label `breaking` added by @Gankra on 2025-04-10 19:59_

---

_Added to milestone `v0.7.0` by @Gankra on 2025-04-10 19:59_

---

_Comment by @Gankra on 2025-04-10 19:59_

Temporarily marking as breaking so it doesn't slip through the cracks of 0.7.0.

---

_Comment by @zanieb on 2025-04-15 20:51_

@Gankra can you own looking into what should happen with the other argument conflicts?

---

_Assigned to @Gankra by @zanieb on 2025-04-21 19:17_

---

_Comment by @Gankra on 2025-04-24 18:35_

Yeah I *think* basically every single ResolveInstallerArgs flag logically contradicts with `--frozen` (still looking through them... there's a lot...).

Funnily enough `--index` *specifically* is valid for *exactly* `uv add` because it's "recycled" by that command to also mean "and add this this index to the project metadata"

---

_Comment by @Gankra on 2025-04-24 19:08_

Although for a few commands that's actually muddier because there's situations where `--frozen` is a dynamic noop with a warning (scripts without lockfiles).

---

_Comment by @Gankra on 2025-04-24 20:27_

Ok I think I've convinced myself that while, strictly speaking, a lot of flags like `--index` could be made exclusive to `--frozen`, that it's probably not the *best* idea to do so.

Specifically because a lot of these arguments can be set with environment variables like `UV_INDEX="aaa"`, and because we tell clap about them, *clap is smart enough to see UV_INDEX is set and error any time you pass `--frozen`*.

I could see this being *really* hostile to any org that's like "ok we setup all our index stuff in environment variables we keep set all the time... and now `--frozen` is randomly exploding!".

---

_Comment by @Gankra on 2025-04-24 20:31_

As such I'm going to merge this as-is, as a fairly reasonable and conservative tightening.

---

_Merged by @Gankra on 2025-04-24 20:31_

---

_Closed by @Gankra on 2025-04-24 20:31_

---

_Branch deleted on 2025-04-24 20:31_

---

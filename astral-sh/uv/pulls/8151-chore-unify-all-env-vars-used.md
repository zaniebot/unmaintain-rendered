```yaml
number: 8151
title: "chore: unify all env vars used"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: unify-env-vars
created_at: 2024-10-12T19:50:13Z
updated_at: 2024-10-14T22:11:03Z
url: https://github.com/astral-sh/uv/pull/8151
synced_at: 2026-01-10T12:54:03Z
```

# chore: unify all env vars used

---

_Pull request opened by @samypr100 on 2024-10-12 19:50_

## Summary

This PR declares and documents all environment variables that are used in one way or another in `uv`, either internally, or externally, or transitively under a common struct.

I think over time as uv has grown there's been many environment variables introduced. Its harder to know which ones exists, which ones are missing, what they're used for, or where are they used across the code. The docs only documents a handful of them, for others you'd have to dive into the code and inspect across crates to know which crates they're used on or where they're relevant.

This PR is a starting attempt to unify them, make it easier to discover which ones we have, and maybe unlock future posibilities in automating generating documentation for them.

I think we can split out into multiple structs later to better organize, but given the high influx of PR's and possibly new environment variables introduced/re-used, it would be hard to try to organize them all now into their proper namespaced struct while this is all happening given merge conflicts and/or keeping up to date.

I don't think this has any impact on performance as they all should still be inlined, although it may affect local build times on changes to the environment vars as more crates would likely need a rebuild. Lastly, some of them are declared but not used in the code, for example those in `build.rs`. I left them declared because I still think it's useful to at least have a reference.

Did I miss any? Are their initial docs cohesive?

Note, `uv-static` is a terrible name for a new crate, thoughts? Others considered `uv-vars`, `uv-consts`.

## Test Plan

Existing tests


---

_@samypr100 reviewed on 2024-10-12 20:07_

---

_Review comment by @samypr100 on `crates/uv/tests/it/lock_scenarios.rs`:1 on 2024-10-12 20:07_

TODO: fix this as it's generated from templates

---

_@samypr100 reviewed on 2024-10-12 20:19_

---

_Review comment by @samypr100 on `crates/uv/src/version.rs`:1 on 2024-10-12 20:19_

This seems to have been replaced by `crates/uv-cli/src/version.rs`?

---

_Marked ready for review by @samypr100 on 2024-10-12 21:06_

---

_@samypr100 reviewed on 2024-10-12 23:20_

---

_Review comment by @samypr100 on `crates/uv/tests/it/lock_scenarios.rs`:1 on 2024-10-12 23:20_

Fixed

---

_Comment by @samypr100 on 2024-10-12 23:38_

Unrelated `uv-test-publish` failing seems to be due to permissions as it requires `id-token: write`, not much I can do to fix that one 😆 

---

_Comment by @konstin on 2024-10-13 08:16_

That's on my todo list, the `github.repository` check to not run this test from forks doesn't work as intended.

---

_Comment by @samypr100 on 2024-10-14 01:58_

> That's on my todo list, the `github.repository` check to not run this test from forks doesn't work as intended.

Make sense, I think we could add a `github.event.pull_request.head.repo.fork != true` check. 

---

_Comment by @charliermarsh on 2024-10-14 14:46_

This generally seems good to me.

---

_Comment by @zanieb on 2024-10-14 14:49_

I'm hyped for this. I'd love to generate the environment variables reference doc from it.

---

_Label `internal` added by @zanieb on 2024-10-14 14:50_

---

_Comment by @konstin on 2024-10-14 15:47_

> Make sense, I think we could add a github.event.pull_request.head.repo.fork != true check.

Thanks, fixed in https://github.com/astral-sh/uv/pull/8168

---

_@zanieb approved on 2024-10-14 21:48_

---

_Merged by @zanieb on 2024-10-14 21:48_

---

_Closed by @zanieb on 2024-10-14 21:48_

---

_Branch deleted on 2024-10-14 22:11_

---

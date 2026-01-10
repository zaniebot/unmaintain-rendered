---
number: 201
title: Allow stale dependencies from local cache when building source distributions
type: issue
state: closed
author: charliermarsh
labels:
  - performance
  - wish
assignees: []
created_at: 2023-10-26T13:33:05Z
updated_at: 2023-11-02T03:25:36Z
url: https://github.com/astral-sh/uv/issues/201
synced_at: 2026-01-10T01:23:03Z
---

# Allow stale dependencies from local cache when building source distributions

---

_Issue opened by @charliermarsh on 2023-10-26 13:33_

If a source distribution requires `hatching>=2.0.0`, and we have `2.0.1` in the cache, but `2.0.2` is available on PyPI, we should just use `2.0.1` (whereas the resolver would often choose `2.0.2`).

---

_Label `performance` added by @charliermarsh on 2023-10-26 13:33_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-26 13:33_

---

_Comment by @charliermarsh on 2023-10-26 19:06_

See: https://github.com/astral-sh/puffin/pull/208

---

_Comment by @konstin on 2023-10-26 19:17_

I'm a little worried about this causing hard to debug build failures, we use a seemingly random version in the middle between oldest and latest possible version that might lack bugfixes, even though it works on another machine that doesn't have the cache. As a user, always using the latest version would be more intuitive for me.

We should consider printing the build environment when reporting build failures. When we stick with the cache strategy, we should also tell the user how to override it.

We also can't save the build dependencies in the lockfile because they are not static (`get_requires_for_build_wheel`) and the build often only happens on some platforms. Using the main resolution as base for the sdist resolution would be certainly helpful though.

One other interesting approach: If we had the possible to do a resolution as of a specific timestamp, we could do the resolution at the same timestamp as the lockfile. I'm not sure if this is a good idea (it's definitive not obvious to a developer trying to debug a build failure), but it would be an alternative that's similarly predictable to always picking the latest version.

---

_Comment by @konstin on 2023-10-26 19:20_

source distribution building is so slow that i don't think the resolution matters anyway, so i'm also fine with just moving this to the future milestone.

---

_Comment by @charliermarsh on 2023-10-26 22:59_

Okay, that sounds fine to me.

---

_Removed from milestone `Initial release` by @charliermarsh on 2023-10-26 22:59_

---

_Added to milestone `Future` by @charliermarsh on 2023-10-26 22:59_

---

_Label `wish` added by @charliermarsh on 2023-10-26 22:59_

---

_Closed by @charliermarsh on 2023-11-02 03:25_

---

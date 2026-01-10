```yaml
number: 21197
title: Use system jemalloc
type: pull_request
state: closed
author: WhyNotHugo
labels: []
assignees: []
draft: true
base: main
head: system-jemalloc
created_at: 2025-11-02T01:49:05Z
updated_at: 2025-12-10T17:36:03Z
url: https://github.com/astral-sh/ruff/pull/21197
synced_at: 2026-01-10T16:42:11Z
```

# Use system jemalloc

---

_Pull request opened by @WhyNotHugo on 2025-11-02 01:49_

The version compiled by tikv_jemallocator by default assumes 16K pages, and doesn't work on systems which have 64K pages.

Link the system jemalloc to ensure we ruff always uses a compatible jemalloc which aligns with local system conventions.

We're currently shipping this patch downstream as otherwise builds simply don't work on systems with 64K pages.

---

_Comment by @MichaReiser on 2025-11-02 02:13_

I'm far from an expert on this but the way we solve this in our own built pipeline is by setting `JEMALLOC_SYS_WITH_LG_PAGE=16`. We haven't had any issues since we increased the page size.

https://github.com/astral-sh/ruff/blob/bff32a41dc440b30764e9414767794e01d25c265/.github/workflows/build-binaries.yml#L280

---

_Comment by @WhyNotHugo on 2025-11-02 02:19_

That workaround only affects builds made by CI, not builds made downstream with release tarballs.

---

_Comment by @WhyNotHugo on 2025-11-02 02:22_

Configuring the build system in releases to use `JEMALLOC_SYS_WITH_LG_PAGE=16` would also work, but most distributions prefer using the system jemalloc regardless, so doing that is likely a better fit.

---

_Comment by @MichaReiser on 2025-11-02 02:49_

but what if the target system doesn't have jemalloc installed. Wouldn't the build fail in that case?

---

_Converted to draft by @WhyNotHugo on 2025-11-02 02:56_

---

_Comment by @WhyNotHugo on 2025-11-02 02:56_

Hang on though, looks like this final syntax isn't working the same as my previous approach.

---

_Comment by @WhyNotHugo on 2025-11-02 12:57_

The following works:

Okay, this works:

    [target.x86_64-alpine-linux-musl.jemalloc]

But this does not:

    [target.'cfg(all(target_env = "musl", target_os = "linux"))'.jemalloc]

I'm not entirely sure why. Maybe we'll need another approach to address this.

---

_Comment by @WhyNotHugo on 2025-11-02 12:58_

> but what if the target system doesn't have jemalloc installed. Wouldn't the build fail in that case?

Right, jemalloc would need to be installed like all other build dependencies. I don't think there's any musl-based distribution which doesn't ship jemalloc.

---

_Comment by @MichaReiser on 2025-12-10 17:36_

I'll close this as the PR has been stale for a while. Please feel free to submit a new PR if you'd like to keep working on this.

---

_Closed by @MichaReiser on 2025-12-10 17:36_

---

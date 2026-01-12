```yaml
number: 12323
title: "docs: Remove note about musl binary distributions"
type: pull_request
state: closed
author: Wingysam
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2025-03-19T21:07:35Z
updated_at: 2025-04-16T15:41:42Z
url: https://github.com/astral-sh/uv/pull/12323
synced_at: 2026-01-12T16:10:13Z
```

# docs: Remove note about musl binary distributions

---

_@Wingysam_

## Summary

Now that uv installs Python in musl-based environments (https://github.com/astral-sh/uv/pull/12121), the docs no longer need to call out that it cannot do this.

## Test Plan

Docs-only change.

## Question

Should this documentation still call out that it can't install Python on arm systems?

---

_@zanieb approved on 2025-03-19 21:24_

Thank you! Should we amend this to note that this is the case for aarch64 images instead of deleting it?

---

_Comment by @zanieb on 2025-03-19 21:24_

ha, I see you asked this — I'd be more comfortable mentioning aarch64 / arm64 until we ship support there.

---

_Label `documentation` added by @konstin on 2025-03-20 14:02_

---

_Comment by @Wingysam on 2025-03-25 15:15_

I've restored the note but clarified that it only applies to ARM now.

---

_Assigned to @zanieb by @zanieb on 2025-03-26 19:51_

---

_Comment by @zanieb on 2025-04-16 15:41_

I thought this had merged, I think now @charliermarsh did this in #12825 — sorry about that. Let me know if anything is missing still.

---

_Closed by @zanieb on 2025-04-16 15:41_

---

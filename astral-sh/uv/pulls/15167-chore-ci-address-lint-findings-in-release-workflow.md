```yaml
number: 15167
title: "chore(ci): address lint findings in release workflow"
type: pull_request
state: open
author: woodruffw
labels:
  - internal
assignees: []
base: main
head: ww/release-ci-lint
created_at: 2025-08-08T14:36:01Z
updated_at: 2025-08-08T17:54:33Z
url: https://github.com/astral-sh/uv/pull/15167
synced_at: 2026-01-12T16:11:36Z
```

# chore(ci): address lint findings in release workflow

---

_@woodruffw_

## Summary

This should be the last of the main linting changes; with this, there will be no more non-pedantic zizmor findings.

## Test Plan

See what happens in CI. I'll also and manually dispatch a dry-run.

---

_Review requested from @zanieb by @woodruffw on 2025-08-08 14:36_

---

_Review requested from @konstin by @woodruffw on 2025-08-08 14:36_

---

_Label `internal` added by @woodruffw on 2025-08-08 14:36_

---

_Comment by @woodruffw on 2025-08-08 14:37_

Oh hm. I guess I need to update `cargo-dist` itself for this?

---

_Comment by @zanieb on 2025-08-08 14:38_

cc @Gankra 

---

_Review comment by @konstin on `.github/workflows/release.yml`:19 on 2025-08-08 14:52_

iirc those are actually used, we're pushing a tag and creating a release

---

_@konstin reviewed on 2025-08-08 14:52_

---

_Comment by @konstin on 2025-08-08 14:54_

The cargo-dist workflow is generated from https://github.com/astral-sh/cargo-dist/blob/622170f09a1521bc0782332317076e4805737cef/cargo-dist/templates/ci/github/release.yml.j2, where we can update it.

---

_Review comment by @woodruffw on `.github/workflows/release.yml`:19 on 2025-08-08 15:09_

> iirc those are actually used, we're pushing a tag and creating a release

Oh hmm, I thought each job had its permissions explicitly set already but I see I missed `announce`. I'll fix this.

---

_@woodruffw reviewed on 2025-08-08 15:09_

---

_Comment by @woodruffw on 2025-08-08 17:54_

> The cargo-dist workflow is generated from [astral-sh/cargo-dist@`622170f`/cargo-dist/templates/ci/github/release.yml.j2](https://github.com/astral-sh/cargo-dist/blob/622170f09a1521bc0782332317076e4805737cef/cargo-dist/templates/ci/github/release.yml.j2), where we can update it.

Ah, this looks like it's architecturally non-trivial to fix: looks like `cargo-dist` favors reusable workflows and doesn't have a view of which exact secrets a given reusable workflow needs, so it shares all of them with `secrets: inherit`:

https://github.com/astral-sh/cargo-dist/blob/622170f09a1521bc0782332317076e4805737cef/cargo-dist/templates/ci/github/release.yml.j2#L478

This makes sense as a constraint there, but it also means that I can't remediate these directly. I'm inclined to WONTFIX them for now, since handling secret inheritance with full generality in cargo-dist seems complicated 🙂 

---

```yaml
number: 16238
title: Bump version to 0.9.2
type: pull_request
state: merged
author: woodruffw
labels: []
assignees: []
merged: true
base: main
head: ww/092
created_at: 2025-10-10T17:48:26Z
updated_at: 2025-10-10T18:20:16Z
url: https://github.com/astral-sh/uv/pull/16238
synced_at: 2026-01-10T06:36:15Z
```

# Bump version to 0.9.2

---

_Pull request opened by @woodruffw on 2025-10-10 17:48_

## Summary

Releases 0.9.2.

## Test Plan

N/A.

---

_Assigned to @woodruffw by @woodruffw on 2025-10-10 17:48_

---

_@zanieb reviewed on 2025-10-10 17:53_

---

_Review comment by @zanieb on `CHANGELOG.md`:22 on 2025-10-10 17:53_

This should be removed and we should add a `### Python` section with the relevant changes. See the other changelogs for Python bumps.

---

_@zanieb reviewed on 2025-10-10 17:53_

---

_Review comment by @zanieb on `CHANGELOG.md`:21 on 2025-10-10 17:53_

The "Other changes" section are just unlabeled pull requests, we always recategorize these. This should be an enhancement, and we should editorialize it to include backticks for the command.

---

_@zanieb reviewed on 2025-10-10 17:54_

---

_Review comment by @zanieb on `CHANGELOG.md`:12 on 2025-10-10 17:54_

For consistency with our usual style

```suggestion
- Avoid inferring check URLs for pyx in `uv publish` ([#16234](https://github.com/astral-sh/uv/pull/16234))
```

---

_@zanieb reviewed on 2025-10-10 17:54_

---

_Review comment by @zanieb on `CHANGELOG.md`:17 on 2025-10-10 17:54_

I'd drop this entry, personally, but it doesn't really matter.

---

_Review comment by @geofft on `CHANGELOG.md`:27 on 2025-10-10 18:08_

```suggestion
```

---

_@geofft approved on 2025-10-10 18:08_

---

_@woodruffw reviewed on 2025-10-10 18:11_

---

_Review comment by @woodruffw on `CHANGELOG.md`:27 on 2025-10-10 18:11_

Oops, thanks.

---

_@zanieb approved on 2025-10-10 18:11_

---

_@zanieb reviewed on 2025-10-10 18:11_

---

_Review comment by @zanieb on `CHANGELOG.md`:12 on 2025-10-10 18:11_

non-blocking nit: I don't think we use periods for lists in the changelog

---

_Comment by @woodruffw on 2025-10-10 18:20_

Merging; will not proceed with the release until the smoke test (which is flaking on network) goes green on `main`.

---

_Merged by @woodruffw on 2025-10-10 18:20_

---

_Closed by @woodruffw on 2025-10-10 18:20_

---

_Branch deleted on 2025-10-10 18:20_

---

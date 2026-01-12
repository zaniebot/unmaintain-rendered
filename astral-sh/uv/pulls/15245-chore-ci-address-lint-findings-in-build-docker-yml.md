```yaml
number: 15245
title: "chore(ci): address lint findings in build-docker.yml"
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/build-docker-lint
created_at: 2025-08-12T18:10:19Z
updated_at: 2025-08-12T18:59:29Z
url: https://github.com/astral-sh/uv/pull/15245
synced_at: 2026-01-12T16:11:39Z
```

# chore(ci): address lint findings in build-docker.yml

---

_@woodruffw_

## Summary

This re-creates #15145, with fixes following the revert in #15174.

The overall approach is the same, except that I've added an explicit permissions block to `docker-annotate-base` that should cover the needed permissions in that job.

(One confusion is around how that wasn't failing before -- FWICT it was receiving the default `GITHUB_TOKEN`, which doesn't include `id-token: write` or `packages: write`. So it _should_ have been failing even before I explicitly did `permissions: {}`...)

Edit: Oh, I see why -- the actual release process does a `workflow_call`, so this inherits its `GITHUB_TOKEN` from `release.yml:custom-build-docker`, which in turn has the right permissions granted to it.

## Test Plan

See what happens in CI. Plus maybe we could do a release dry-run?

---

_Review requested from @zanieb by @woodruffw on 2025-08-12 18:10_

---

_Assigned to @woodruffw by @woodruffw on 2025-08-12 18:10_

---

_Label `internal` added by @woodruffw on 2025-08-12 18:10_

---

_Comment by @zanieb on 2025-08-12 18:13_

Just fyi we don't really have a release dry-run option here. We'll run the Docker builds in the pull request, but we can't test annotating publishes without publishing.

---

_Comment by @woodruffw on 2025-08-12 18:41_

> Just fyi we don't really have a release dry-run option here. We'll run the Docker builds in the pull request, but we can't test annotating publishes without publishing.

Gotcha, OK -- in that case I don't have a great way to test my reusable workflow permissions theory 😅. However, I cross-checked that the permissions are consistent here:

https://github.com/astral-sh/uv/blob/959c9521c8ef38d7409e6b34aa4a42097b670b6a/.github/workflows/release.yml#L112-L116

So I _think_ this is good to go.

---

_@zanieb approved on 2025-08-12 18:59_

---

_Merged by @zanieb on 2025-08-12 18:59_

---

_Closed by @zanieb on 2025-08-12 18:59_

---

_Branch deleted on 2025-08-12 18:59_

---

```yaml
number: 16989
title: Publish PyPI releases before crates.io artifacts
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/release-publish-order
created_at: 2025-12-04T19:30:39Z
updated_at: 2025-12-11T16:03:16Z
url: https://github.com/astral-sh/uv/pull/16989
synced_at: 2026-01-10T05:49:14Z
```

# Publish PyPI releases before crates.io artifacts

---

_Pull request opened by @woodruffw on 2025-12-04 19:30_

## Summary

Closes #16987.

## Test Plan

We need a good way to dry-run this...



---

_Review requested from @zanieb by @woodruffw on 2025-12-04 19:30_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-04 19:30_

---

_Label `internal` added by @woodruffw on 2025-12-04 19:30_

---

_@github-advanced-security[bot] reviewed on 2025-12-04 19:31_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/publish.yml`:22 on 2025-12-04 19:31_

secrets unconditionally inherited by called workflow

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/184)

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/publish.yml`:36 on 2025-12-04 19:31_

secrets unconditionally inherited by called workflow

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/185)

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/release.yml`:216 on 2025-12-04 19:31_

secrets unconditionally inherited by called workflow

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/186)

---

_@zanieb reviewed on 2025-12-04 23:07_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:229 on 2025-12-04 23:07_

Can't we just insert a `needs: custom-publish-pypi` here instead of changing the composition?

---

_@zanieb reviewed on 2025-12-04 23:08_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:229 on 2025-12-04 23:08_

this would either require an upstream cargo-dist feature or a dirty manifest cc @Gankra

It might just be easier to conceptualize than nesting the workflows?

---

_Review comment by @woodruffw on `.github/workflows/release.yml`:229 on 2025-12-05 00:56_

Yeah, that's the tradeoff here -- I figured this was a good initial fix, and I could send patches upstream for encoding the `needs:` (and for https://github.com/axodotdev/cargo-dist/issues/2216).

---

_@woodruffw reviewed on 2025-12-05 00:56_

---

_@zanieb reviewed on 2025-12-05 00:58_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:229 on 2025-12-05 00:58_

I just know using `needs:` won't break anything but am really not sure if this setup will.

---

_Review comment by @woodruffw on `.github/workflows/release.yml`:229 on 2025-12-05 20:45_

I traced this a bit, and I'm _pretty_ sure it'll work as expected: our top-level `release.yml` is triggered by `pull_request` and `workflow_dispatch`, neither of which is reusable. `release.yml` then has `custom-publish`, which dispatches to our first reusable workflow level. That then dispatches to the next level, which means we have at most three levels of calls.

That in turn should put us comfortably under the limit of 10, which is documented here: https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows#nesting-reusable-workflows

With that said, I still find this not confidence inspiring 😅 -- I'd love to be able to propagate the `dry-run` input into these reusable workflows so that I could test them, but from what I can find it seems like the `plan` object doesn't include it. So I'd need to send another patch up to `dist` for that, which puts it in the same bucket as the other changes.

---

_@woodruffw reviewed on 2025-12-05 20:45_

---

_@zanieb reviewed on 2025-12-05 20:46_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:229 on 2025-12-05 20:46_

I think I'd rather just have a dirty workflow file until we do the upstream work.

---

_@samypr100 reviewed on 2025-12-06 03:39_

---

_Review comment by @samypr100 on `.github/workflows/release.yml`:229 on 2025-12-06 03:39_

You could "dry-run" it with a real release on a fork 😅. I've done nested workflows like this at work, so in theory this should work as long as the permissions and secrets inherits is set correctly. The Dependency Graph UX is terrible though.

---

_@woodruffw reviewed on 2025-12-06 04:56_

---

_Review comment by @woodruffw on `.github/workflows/release.yml`:229 on 2025-12-06 04:56_

Yeah, it'll just be a mess with all the release state. I agree with @zanieb that having a dirty change is probably the best short term fix here 😅

(Long term I want to remove these reusable workflows entirely and replace them with actions, since they mess with the signing identity when publishing / attesting.)

---

_Comment by @woodruffw on 2025-12-09 16:58_

Okay, this is now a single-line change.

---

_Review requested from @zanieb by @woodruffw on 2025-12-09 16:58_

---

_@zanieb approved on 2025-12-09 17:03_

---

_Comment by @zanieb on 2025-12-09 17:03_

@Gankra if you could confirm please :)

---

_Renamed from "Enforce release publishing order" to "Publish PyPI releases before crates.io artifacts" by @zanieb on 2025-12-09 17:04_

---

_Merged by @zanieb on 2025-12-09 21:20_

---

_Closed by @zanieb on 2025-12-09 21:20_

---

_Branch deleted on 2025-12-09 21:20_

---

_Comment by @Gankra on 2025-12-11 15:27_

Oh yuck, ok yeah we'll do something upstream in cargo-dist for this. You can *almost* just move cargo-publish to a custom-host-job but it wouldn't respect dry-runs. Although, what was wrong with the "single publish job that just does both sub-jobs in serial"?

---

_Comment by @zanieb on 2025-12-11 15:33_

It sounded way more likely to break via some OIDC edge case and this is dead simple.

---

_Comment by @Gankra on 2025-12-11 15:58_

Hmm I'll have to take your word for it on OIDC.

---

_Comment by @zanieb on 2025-12-11 16:03_

I mean, it _should_ work but there's ~no consequence to a dirty dist workflow right now.

---

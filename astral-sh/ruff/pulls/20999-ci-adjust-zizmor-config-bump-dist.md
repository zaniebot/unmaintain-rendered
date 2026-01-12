```yaml
number: 20999
title: "ci: adjust zizmor config, bump dist"
type: pull_request
state: merged
author: woodruffw
labels:
  - ci
assignees: []
merged: true
base: main
head: ww/more-ci-cleanup
created_at: 2025-10-20T16:33:23Z
updated_at: 2025-10-22T21:48:19Z
url: https://github.com/astral-sh/ruff/pull/20999
synced_at: 2026-01-12T15:57:14Z
```

# ci: adjust zizmor config, bump dist

---

_@woodruffw_

## Summary

Also bumps `cargo dist` to 0.30, and moves us
back to the upstream copy of `dist` now that
the latest version has integrated our fork's
patches.

## Test Plan

See what happens in CI 🙂 

---

_Review requested from @zsol by @woodruffw on 2025-10-20 16:33_

---

_Review requested from @Gankra by @woodruffw on 2025-10-20 16:33_

---

_Review requested from @AlexWaygood by @woodruffw on 2025-10-20 16:33_

---

_Assigned to @woodruffw by @woodruffw on 2025-10-20 16:33_

---

_Review comment by @woodruffw on `.github/workflows/publish-ty-playground.yml`:41 on 2025-10-20 16:34_

Noting: the reason we disable this cache is because there's a dataflow from the cache here to a "release" artifact, i.e. the CloudFlare deployment artifact. 

(Whether or not this constitutes a significant cache poisoning risk is a matter of taste/discretion 😅)

---

_@woodruffw reviewed on 2025-10-20 16:34_

---

_@AlexWaygood reviewed on 2025-10-20 16:39_

---

_Review comment by @AlexWaygood on `dist-workspace.toml`:20 on 2025-10-20 16:39_

ew, I preferred the previous formatting 😆 maybe we should add a TOML formatter to pre-commit...

---

_Comment by @AlexWaygood on 2025-10-20 16:40_

Does this PR mean we can get rid of this ignore from our zizmor config?

https://github.com/astral-sh/ruff/blob/44e678a222b37164c4ded4b7b6a19c9feffb4707/.github/zizmor.yml#L12

---

_@woodruffw reviewed on 2025-10-20 16:41_

---

_Review comment by @woodruffw on `dist-workspace.toml`:20 on 2025-10-20 16:41_

Yeah, it's unfortunate these churned 😅

---

_Comment by @woodruffw on 2025-10-20 16:42_

> Does this PR mean we can get rid of this ignore from our zizmor config?

Oh hmm, that's for `publish-playground` not `publish-ty-playground`. I'll take a look there.

---

_@woodruffw reviewed on 2025-10-20 17:00_

---

_Review comment by @woodruffw on `.github/zizmor.yml`:14 on 2025-10-20 17:00_

I've removed these, since we now use inline ignore comments to ignore the exact acceptable cache risks rather than blanket ignoring all cache poisoning vectors 🙂 

(I did a review pass on each of these, and the primary risk in all cases is that a poisoned cache could influence one of the various HTML artifacts pushed to CloudFlare Pages. I believe that's an acceptable risk in practice!)

---

_Review comment by @woodruffw on `.github/zizmor.yml`:21 on 2025-10-20 17:00_

I went ahead and blanket-disabled these findings, since we don't have a remediation strategy until `cargo dist` uses fine-grained secret inheritance.

---

_@woodruffw reviewed on 2025-10-20 17:00_

---

_@woodruffw reviewed on 2025-10-20 17:01_

---

_Review comment by @woodruffw on `.github/zizmor.yml`:26 on 2025-10-20 17:01_

Same as above -- I manually reviewed these for risk and they're not a danger in their current trigger settings. We'll need cargo dist to remove them, at which point we can regenerate here.

---

_Label `ci` added by @AlexWaygood on 2025-10-20 17:02_

---

_Renamed from "ci: disable cache on ty playground" to "ci: adjust zizmor config, bump dist" by @woodruffw on 2025-10-20 17:04_

---

_Review comment by @AlexWaygood on `.github/workflows/release.yml`:1 on 2025-10-20 18:15_

Would like @Gankra's confirmation that migrating off the fork is okay and desirable 😄

---

_Review comment by @AlexWaygood on `dist-workspace.toml`:20 on 2025-10-20 18:17_

I would actually prefer it if we reverted this change, if that's okay (sorry to be a pain) -- this is now a really long line, and it's also going to make `git blame` muddier for this section of the config

---

_@AlexWaygood approved on 2025-10-20 18:17_

Thank you!

---

_@woodruffw reviewed on 2025-10-20 18:24_

---

_Review comment by @woodruffw on `dist-workspace.toml`:20 on 2025-10-20 18:24_

No worries, completely reasonable! 

---

_@woodruffw reviewed on 2025-10-20 18:26_

---

_Review comment by @woodruffw on `dist-workspace.toml`:20 on 2025-10-20 18:26_

abc011d9c..f5aaf9d50

---

_@zsol approved on 2025-10-21 08:15_

---

_Comment by @woodruffw on 2025-10-21 14:09_

CI is green, but waiting for @Gankra's blessing 🙂 

---

_@Gankra reviewed on 2025-10-22 15:30_

---

_Review comment by @Gankra on `.github/workflows/release.yml`:1 on 2025-10-22 15:30_

Yes

---

_@Gankra approved on 2025-10-22 15:31_

Looks good, thanks!

---

_Merged by @Gankra on 2025-10-22 21:48_

---

_Closed by @Gankra on 2025-10-22 21:48_

---

_Branch deleted on 2025-10-22 21:48_

---

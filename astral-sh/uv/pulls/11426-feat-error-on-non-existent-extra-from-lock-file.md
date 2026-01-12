```yaml
number: 11426
title: "feat: error on non-existent extra from lock file"
type: pull_request
state: merged
author: mkniewallner
labels: []
assignees: []
merged: true
base: tracking/060
head: feat/error-on-non-existent-extra-from-lock
created_at: 2025-02-11T20:51:18Z
updated_at: 2025-02-12T17:32:17Z
url: https://github.com/astral-sh/uv/pull/11426
synced_at: 2026-01-12T16:09:50Z
```

# feat: error on non-existent extra from lock file

---

_@mkniewallner_

## Summary

Closes #10597.

Recreated https://github.com/astral-sh/uv/pull/10925 that got closed as the base branch got merged.

## Test Plan

Snapshot tests.

---

_Review requested from @Gankra by @Gankra on 2025-02-11 20:52_

---

_@Gankra reviewed on 2025-02-11 20:53_

replicating review comment to this PR

---

_@Gankra reviewed on 2025-02-11 20:54_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:2627 on 2025-02-11 20:54_



I'm not sure if this is the right place to handle this but I believe this isn't our desired semantic for the case where provides_extras is undefined. We aren't doing a lockfile version bump, so we're still supporting provides_extras being missing.

Some part of this code should handle that as "I don't actually have information on if the extra is defined, and will conservatively not error, because I'm uncertain".


---

_Review comment by @mkniewallner on `crates/uv-resolver/src/lock/mod.rs`:2627 on 2025-02-11 20:54_

Oh yeah definitely, will update the logic.

---

_@mkniewallner reviewed on 2025-02-11 20:54_

---

_Comment by @Gankra on 2025-02-11 20:57_

I *think* the kind of check you want is something like... "if *no* package in the lockfile defines provides-extras, refuse to do the check" (because you don't want a single package missing provides-extras to falsely allow all extra names).

---

_Comment by @Gankra on 2025-02-11 21:02_

(also don't worry about the  python3.13 failure, that's fixed on main but not the feature branch aiui)

---

_Comment by @mkniewallner on 2025-02-11 22:05_

Thanks for the guidance and sorry for missing this case (although it was kinda obvious in retrospective :sweat_smile:). I hope https://github.com/astral-sh/uv/pull/11426/commits/743258b126488a6946e0216a05e8de8e4d239294 is what you had in mind.

I've moved `contains_extra` outside of `Lock` implementation as you suggested, to avoid misusages of it, since it would be easy, otherwise, to not think about the fact that the information could be missing because of older versions.

---

_Marked ready for review by @mkniewallner on 2025-02-11 22:05_

---

_@Gankra approved on 2025-02-12 16:00_

Awesome, looks great to me!

---

_Added to milestone `v0.6.0` by @Gankra on 2025-02-12 16:00_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/install_target.rs`:7 on 2025-02-12 16:49_

We don't enforce this but since this file already adheres to it, would you mind retaining:

```
<std>

<third-party>

<uv crates>

<crate::>
```

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/install_target.rs`:250 on 2025-02-12 16:49_

Can we use `Either` here and return an iterator in each case? I suspect we can avoid this clone?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:371 on 2025-02-12 16:50_

I'd probably move this below the settings destructure, just above "// Validate that the Python version is supported by the lockfile.", since it's semantically grouped with those operations.

---

_@charliermarsh approved on 2025-02-12 16:51_

---

_Merged by @Gankra on 2025-02-12 17:29_

---

_Closed by @Gankra on 2025-02-12 17:29_

---

_@mkniewallner reviewed on 2025-02-12 17:32_

---

_Review comment by @mkniewallner on `crates/uv/src/commands/project/install_target.rs`:250 on 2025-02-12 17:32_

Oh thanks, I did want to do that but did not think about using `Either`!

---

_Branch deleted on 2025-02-12 17:32_

---

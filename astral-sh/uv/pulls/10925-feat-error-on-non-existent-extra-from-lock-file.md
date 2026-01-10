```yaml
number: 10925
title: "feat: error on non-existent extra from lock file"
type: pull_request
state: closed
author: mkniewallner
labels: []
assignees: []
base: gankra/lock-extra
head: feat/error-on-non-existent-extra-from-lock
created_at: 2025-01-24T01:28:00Z
updated_at: 2025-02-11T20:53:44Z
url: https://github.com/astral-sh/uv/pull/10925
synced_at: 2026-01-10T11:10:34Z
```

# feat: error on non-existent extra from lock file

---

_Pull request opened by @mkniewallner on 2025-01-24 01:28_

## Summary

Based on https://github.com/astral-sh/uv/pull/11063.

Alternative to #10869 which follows the suggestion, in https://github.com/astral-sh/uv/pull/10869#issuecomment-2609916298, to validate extras from the lock file instead of `pyproject.toml`.

## Test Plan

Snapshot tests.

Some tests are failing, but it seems unrelated to the changes, and likely due to the branch being based on an outdated branch (rebasing the branch from latest `main` makes one of the failing test pass locally).

---

_Review comment by @mkniewallner on `crates/uv/src/commands/project/mod.rs`:157 on 2025-01-24 01:50_

Wording could probably be adapted here, as the check happens at lock file level, but not sure exactly how we would want to phrase this.

---

_@mkniewallner reviewed on 2025-01-24 01:51_

---

_Marked ready for review by @mkniewallner on 2025-02-09 14:44_

---

_Comment by @Gankra on 2025-02-11 15:53_

Oh rad, thanks for the rewrite/rebase! (We'll land the base branch this week at which point this can land too)

---

_Review requested from @Gankra by @Gankra on 2025-02-11 15:53_

---

_Closed by @Gankra on 2025-02-11 19:38_

---

_Comment by @Gankra on 2025-02-11 19:40_

*forehead slap*

What a terrible github behaviour. I can't even reopen this! Sorry! (It needs to be retargetted to [tracking/060](https://github.com/astral-sh/uv/tree/tracking/060))

---

_Comment by @Gankra on 2025-02-11 19:41_

In the meantime I'll still review this...

---

_Comment by @mkniewallner on 2025-02-11 20:41_

> _forehead slap_
> 
> What a terrible github behaviour. I can't even reopen this! Sorry! (It needs to be retargetted to [tracking/060](https://github.com/astral-sh/uv/tree/tracking/060))

No worries, will rebase and recreate the PR :)

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:2627 on 2025-02-11 20:45_

I'm not sure if this is the right place to handle this but I believe this isn't our desired semantic for the case where `provides_extras` is undefined. We aren't doing a lockfile version bump, so we're still supporting `provides_extras` being missing.

Some part of this code should handle that as "I don't actually have information on if the extra is defined, and will conservatively not error, because I'm uncertain".

---

_@Gankra reviewed on 2025-02-11 20:47_

Basic idea *looks* right but needs to handle a corner case.

---

_Review comment by @mkniewallner on `crates/uv-resolver/src/lock/mod.rs`:2627 on 2025-02-11 20:53_

Oh yeah definitely, will update the logic.

---

_@mkniewallner reviewed on 2025-02-11 20:53_

---

```yaml
number: 19380
title: "[ty] add an `I_KNOW_TY_IS_PRE_RELEASE` env var to turn off the pre-release warning"
type: pull_request
state: closed
author: oconnor663
labels:
  - ty
assignees: []
base: main
head: jack/i_know_ty_is_prerelease
created_at: 2025-07-16T01:16:47Z
updated_at: 2025-07-22T15:10:37Z
url: https://github.com/astral-sh/ruff/pull/19380
synced_at: 2026-01-10T17:58:13Z
```

# [ty] add an `I_KNOW_TY_IS_PRE_RELEASE` env var to turn off the pre-release warning

---

_Pull request opened by @oconnor663 on 2025-07-16 01:16_

_No description provided._

---

_Review requested from @carljm by @oconnor663 on 2025-07-16 01:16_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-07-16 01:16_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-16 01:16_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-16 01:16_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-16 01:16_

---

_Renamed from "add an `I_KNOW_TY_IS_PRE_RELEASE` env var to turn off the pre-release warning" to "[ty] add an `I_KNOW_TY_IS_PRE_RELEASE` env var to turn off the pre-release warning" by @oconnor663 on 2025-07-16 01:16_

---

_Label `ty` added by @oconnor663 on 2025-07-16 01:16_

---

_Comment by @charliermarsh on 2025-07-16 01:19_

For what it's worth, in uv we tend to use the `--preview` flag for this. We warn if you use features that are in preview without providing the `--preview` flag, and providing the `--preview` flag explicitly turns off those warnings.

---

_Comment by @github-actions[bot] on 2025-07-16 01:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @oconnor663 on 2025-07-16 01:51_

I'd be equally happy either way. I mostly run `ty` via a shell alias that builds my local checkout, and I could toss `--preview` in there.

---

_@sharkdp approved on 2025-07-16 06:13_

I'd be fine with adding this temporarily, seems like the easiest solution. And there's no risk of breaking anyone once we remove it. Should maybe start with `TY_` though for consistency (https://docs.astral.sh/ty/reference/env/)?

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:71 on 2025-07-16 07:07_

The environment variable needs. to be added to https://github.com/astral-sh/ruff/blob/35a33f045eecb6652b5c73f82dce26e6c610c3b8/crates/ty_static/src/env_vars.rs#L7

---

_@MichaReiser reviewed on 2025-07-16 07:09_

I feel a bit conflicted about this. It's obviously the easiest solution but it's not what we want to do long term and we haven't received any user reports yet that they find it annoying. 

It's also somewhat intentional that the warning is annoying because it was intended as a way to discourage using ty in production because we know it isn't ready yet. 

What are the cases where the message get sinto your way? Could you use `--quiet` instead?

---

_Comment by @charliermarsh on 2025-07-16 13:01_

> I'd be equally happy either way. I mostly run ty via a shell alias that builds my local checkout, and I could toss --preview in there.

(Just for completeness, if you used `--preview` to solve this problem, I'd also expect `TY_PREVIEW` to exist.)


---

_Comment by @oconnor663 on 2025-07-18 16:51_

> What are the cases where the message gets into your way?

Mostly I find myself running `ty check` on lots of small examples (do other folks have different workflows?), and it's slightly annoying to see that warning every time. But I wouldn't go so far as to say it gets in my way :)

> Could you use --quiet instead?

IIUC that hides the diagnostic messages too, but I want to see those.

---

_Review request for @carljm removed by @carljm on 2025-07-18 23:31_

---

_Comment by @carljm on 2025-07-18 23:32_

I have no opinion here and will leave this up to @MichaReiser :)

---

_Comment by @MichaReiser on 2025-07-19 13:47_

I'd prefer to either have a solution that works for users too (and that we recommend) or leave it as is (but I also don't feel strongly to object, it's just not high on my priority list). 

We probably want to have something like a preview mode and we could add that instead. 

A hacky solution for now would be to move the log message out of `lib.rs` and into another module so that it can be suppressed with `TY_LOG`: `TY_LOG=ty=info,ty::warn=error`

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-21 13:43_

---

_Comment by @oconnor663 on 2025-07-22 15:10_

Ok I'll close this for now.

---

_Closed by @oconnor663 on 2025-07-22 15:10_

---

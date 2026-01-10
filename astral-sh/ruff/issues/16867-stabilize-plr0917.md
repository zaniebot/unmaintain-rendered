```yaml
number: 16867
title: "Stabilize `PLR0917`"
type: issue
state: open
author: flying-sheep
labels:
  - rule
  - preview
assignees: []
created_at: 2025-03-20T11:55:09Z
updated_at: 2025-10-02T12:39:31Z
url: https://github.com/astral-sh/ruff/issues/16867
synced_at: 2026-01-10T11:09:58Z
```

# Stabilize `PLR0917`

---

_Issue opened by @flying-sheep on 2025-03-20 11:55_

I think it looks pretty stable, no?

---

_Label `rule` added by @ntBre on 2025-03-20 14:29_

---

_Label `preview` added by @ntBre on 2025-03-20 14:29_

---

_Comment by @ntBre on 2025-03-20 14:32_

I think it does look pretty stable. Searching for related issues and PRs turns up a lot of results, but the ones I checked only referred to PLR0917 and didn't actually modify it. We should consider stabilizing it in the next minor release.

---

_Comment by @MichaReiser on 2025-03-20 15:16_

Trying to remember what the reason was that we didn't stabilize the rules... 

The only thing that comes to my mind is that we had concerns around various complexity rules (this is a one-off because it tries to approximate complexity by the number of positional arguments). That would explain why many of the other *too-many* rules haven't been stabilized. But I wasn't able to find any mention in notion. @AlexWaygood do you remember why we haven't stabilized this rule (it was added in 2023)

---

_Added to milestone `v0.11` by @MichaReiser on 2025-03-20 15:22_

---

_Removed from milestone `v0.12` by @ntBre on 2025-06-10 20:38_

---

_Added to milestone `v0.13` by @ntBre on 2025-06-10 20:38_

---

_Comment by @ntBre on 2025-09-03 17:30_

I'm looking at this again for 0.13, and I think there are a couple of potential blockers for stabilization:
- It's a pretty opinionated rule, so we may want to wait for #1774 (I commented this [here](https://github.com/astral-sh/ruff/pull/19240#issuecomment-3053483488) too)
- We may want to update the rule to skip pytest hooks (https://github.com/astral-sh/ruff/issues/7286#issuecomment-2571570758)

Related to the first point, clippy's analogous [rule](https://rust-lang.github.io/rust-clippy/stable/index.html#too_many_arguments) has a default severity of `warn`, so https://github.com/astral-sh/ruff/issues/1256 may be related too.

I think we should probably resolve the second issue before stabilization, but I don't think being opinionated is necessarily a hard blocker on its own.

---

_Comment by @flying-sheep on 2025-09-03 18:32_

[`PLR0913` (`too-many-arguments`)](https://docs.astral.sh/ruff/rules/too-many-arguments/) used to work like this one and was changed to be less useful, this is why this rule exists. This rule has a strictly more narrow scope as `too-many-arguments`, so it’s also strictly less opinionated.

---

_Removed from milestone `v0.13` by @ntBre on 2025-09-05 13:06_

---

_Added to milestone `v0.14` by @ntBre on 2025-09-05 13:06_

---

_Comment by @flying-sheep on 2025-09-13 12:17_

Damn, I should have pushed this more, now 0.13 has been released without it

---

_Removed from milestone `v0.14` by @ntBre on 2025-10-02 12:39_

---

_Added to milestone `v0.15` by @ntBre on 2025-10-02 12:39_

---

```yaml
number: 8074
title: Allow safe fixes to be added in patch releases
type: issue
state: open
author: zanieb
labels:
  - linter
assignees: []
created_at: 2023-10-19T18:58:52Z
updated_at: 2024-01-30T14:08:41Z
url: https://github.com/astral-sh/ruff/issues/8074
synced_at: 2026-01-10T11:09:50Z
```

# Allow safe fixes to be added in patch releases

---

_Issue opened by @zanieb on 2023-10-19 18:58_

Adding new fixes isn't really a breaking change necessitating a minor version bump, however we have it categorized as such in our [current versioning policy](https://docs.astral.sh/ruff/versioning/#version-changes). Somewhat asymmetrically, we allow unsafe fixes to be added in patch releases. The idea was originally that unsafe fixes required opt-in so it was okay to add them in patch releases. In general, I think new fixes should be allowed in any release.

The downside here is that users may need to change their configuration to include new fixes as unfixable on patch releases if they do not want the fix. This goes against the intent to have patch releases be free of configuration changes. However, these fixes are unlikely to be applied in a context without manual review.

If accepted, we should make this change as part of v0.2.0 and respect the current rules until then.

---

_Added to milestone `v0.2.0` by @zanieb on 2023-10-19 18:58_

---

_Comment by @Avasam on 2023-10-24 21:36_

There's also the risk that a "safe" fix is actually unsafe and that wasn't caught until out of preview.

---

_Label `linter` added by @dhruvmanila on 2023-10-25 04:45_

---

_Comment by @charliermarsh on 2023-11-06 00:43_

I'd like to elevate `SIM110` and `SIM111`. I'll probably add more over time.

---

_Comment by @Avasam on 2023-12-16 18:53_

> There's also the risk that a "safe" fix is actually unsafe and that wasn't caught until out of preview.

#9156 is a perfect example of what I mean here. An unintended bug, but it happens, so it is to be considered.

---

_Removed from milestone `v0.2.0` by @zanieb on 2024-01-30 01:22_

---

_Comment by @zanieb on 2024-01-30 01:22_

I don't think we have a clear consensus that this is worth changing yet. Removing this from our v0.2.0 milestone.

---

_Comment by @charliermarsh on 2024-01-30 01:23_

I still want this :sob:

---

_Comment by @MichaReiser on 2024-01-30 07:03_

To me the discussion should be whether we allow adding fixes in releases in general:
IMO, the bar for adding a manual fix in a patch release is lower than that for adding a safe fix or an unsafe fix. I'm also leaning towards the bar for adding an unsafe fix being lower than that for adding a safe fix. 





---

_Comment by @AlexWaygood on 2024-01-30 10:59_

> There's also the risk that a "safe" fix is actually unsafe and that wasn't caught until out of preview.

To @Avasam's point here -- the fixes for RUF022 and RUF023 were both marked as "safe", and were included in the most recent patch release -- but in fact, they had the potential to introduce syntax errors in some unusual edge cases: https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916203707; https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916213693. (The rules themselves were still in preview here, but this is a good recent example of the way that fixes we think are safe might not actually be safe.)

---

_Comment by @charliermarsh on 2024-01-30 14:08_

A few reactions (and not a decision):

- The current setup is strange in that we can add unsafe fixes in a patch release, but not safe fixes. So we're actually not allowed (per the versioning policy) to give users access to the safest, most trivial fixes, while they _can_ get access to the much-less-safe fixes.
- There's a difference between what's allowed by the versioning policy, and what's a best practice. The versioning policy could allow us to ship safe fixes in a patch release, but we might still choose to gate fixes behind `--preview` if we feel they need more time (e.g., I would've gated the `RET` fixes either way).
- One option is to always ship "safe" fixes as "unsafe" in patch releases, and then upgrade them to "safe" in minor releases. (We could use "safe" in preview from the start -- i.e., "safe" in preview, "unsafe" in stable.) This would adhere to the versioning policy and would give users access to the fixes. It's a bit odd though, since "unsafe" isn't intended for "unstable", it's intended to indicate that the meaning of the code might change.

---

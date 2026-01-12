```yaml
number: 15891
title: Add a 45-day cooldown to reqwest upgrades
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
base: main
head: zb/renovate-reqwest
created_at: 2025-09-16T13:43:35Z
updated_at: 2026-01-09T13:33:55Z
url: https://github.com/astral-sh/uv/pull/15891
synced_at: 2026-01-12T16:12:00Z
```

# Add a 45-day cooldown to reqwest upgrades

---

_@zanieb_

To approximate N-1.

See https://github.com/astral-sh/uv/pull/15502


---

_Review requested from @charliermarsh by @zanieb on 2025-09-17 10:51_

---

_Review requested from @konstin by @zanieb on 2025-09-17 10:51_

---

_Comment by @konstin on 2025-09-17 11:06_

What about instead saying that we want to wait some time after a reqwest release so that regressions may get fixed, instead of waiting until after the next release? Otherwise we always have to check if something was already fixed in the latest version over being confident it's a regression.

Can we extend the list to contain the transitive h2 and http dependencies too? Our bugs are often in those rather than in reqwest itself. A possible list `"h2", "http", "http-body", "http-body-util", "hyper", "hyper-rustls", "hyper-util"`. We may also consider `"hyper-rustls", "rustls"`, though for rustls updating fast could be important security-wise? Apart from extending the list r+.

---

_Comment by @zanieb on 2025-10-09 14:10_

> What about instead saying that we want to wait some time after a reqwest release so that regressions may get fixed, instead of waiting until after the next release? 

Sorry, what do you mean by this?

---

_Comment by @zanieb on 2025-10-09 14:10_

(I sort of don't have the energy to pursue this if it's a more complicated task, it doesn't feel worth it)

---

_Comment by @konstin on 2025-10-09 14:16_

> > What about instead saying that we want to wait some time after a reqwest release so that regressions may get fixed, instead of waiting until after the next release?
> 
> Sorry, what do you mean by this?

I'm worried that a 45-day means we get to N-1 instead of N, and then N has a fix we want. The release cadence isn't very regular, so I don't have a good suggestion for the actual timing.

> (I sort of don't have the energy to pursue this if it's a more complicated task, it doesn't feel worth it)

What about extending `matchPackageNames` with `"h2", "http", "http-body", "http-body-util", "hyper", "hyper-rustls", "hyper-util"` and setting the cooldown shorter?

---

_Comment by @zanieb on 2025-10-09 14:23_

> I'm worried that a 45-day means we get to N-1 instead of N, and then N has a fix we want. The release cadence isn't very regular, so I don't have a good suggestion for the actual timing.

Isn't that a problem for any N-1 release cycle? I think a fix release usually comes at a much faster cadence, which gives us _some_ insulation from breakage by having a cooldown. The tooling doesn't support doing N-1, so this is sort of the best approximation we can do, I think — though it's unclear to me if N-1 solves the problem. What are you looking to specifically have change w.r.t. this point?


---

_Comment by @NMertsch on 2026-01-09 13:33_

Maybe relevant: I think upgrading to [reqwest >= v0.13.0](https://github.com/seanmonstar/reqwest/releases/tag/v0.13.0) would fix #9243.
(Please ignore if not relevant, thanks for the great work!)

---

```yaml
number: 16803
title: "Add a native 'cooldown' feature"
type: issue
state: closed
author: woodruffw
labels:
  - enhancement
assignees: []
created_at: 2025-11-21T04:38:01Z
updated_at: 2025-11-21T16:00:38Z
url: https://github.com/astral-sh/uv/issues/16803
synced_at: 2026-01-10T03:23:55Z
```

# Add a native 'cooldown' feature

---

_Issue opened by @woodruffw on 2025-11-21 04:38_

### Summary

uv supports `--exclude-newer` (and a corresponding config option) for limiting resolutions by a cutoff date. This is really great for reproducibility (assuming stable resolutions), since new versions won't be picked up.

A related but slightly different concept is "cooldowns": instead of being a fixed cutoff date, a cooldown is a _rolling_ duration. Dependency releases made within the cooldown period are not eligible for resolution. This doesn't have the reproducibility benefits of `exclude-newer`, but it *does* have supply-chain security benefits: most compromised dependencies are identified and remediated within hours or days of active exploitation, meaning that a cooldown of 1-2 weeks (as a default) is likely to catch the overwhelming majority of potentially malicious dependency changes.

Prior art for this exists in Dependabot and Renovate, as well as ecosystem-specific functionality in JS (pnpm's `minimumReleaseAge`). Dependabot and Renovate do a decent job, but supporting cooldowns directly in packaging tools conveys nontrivial benefits: it ensures that the cooldown is enforced uniformly (versus just on automated updates) and ensures that a single tool has a single source of truth about dependency ages (this is a problem for Dependabot in particular, which sometimes makes somewhat opaque resolution decisions/has opaque cooldown behavior).


### Example

A rough sketch design: uv grows a `--cooldown <duration>` option (and corresponding setting). This option would be used like:

```console
# add 'sampleproject', but only selecting releases/dists >= 7d old
# --cooldown defaults to 7d
uv add --cooldown sampleproject

# can be made explicit
uv add --cooldown=14d sampleproject
```

(And similarly for all other mutative uv commands.)

---

_Label `enhancement` added by @woodruffw on 2025-11-21 04:38_

---

_Comment by @notatallshaw on 2025-11-21 15:59_

Is this a duplicate of https://github.com/astral-sh/uv/issues/14992 ?

---

_Comment by @woodruffw on 2025-11-21 16:00_

Oops, yes. I searched for 'cooldown' as a keyword and missed that. Thanks @notatallshaw!

---

_Closed by @woodruffw on 2025-11-21 16:00_

---

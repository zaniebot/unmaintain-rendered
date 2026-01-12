```yaml
number: 296
title: "distinguish \"doesn't exist\" from \"doesn't exist on this Python version\""
type: issue
state: open
author: carljm
labels:
  - diagnostics
assignees: []
created_at: 2025-05-09T16:09:06Z
updated_at: 2025-11-18T16:10:26Z
url: https://github.com/astral-sh/ty/issues/296
synced_at: 2026-01-12T15:54:22Z
```

# distinguish "doesn't exist" from "doesn't exist on this Python version"

---

_@carljm_

If a user has their Python version mis-configured, and gets an error like `module 'datetime' has no attribute 'UTC'`, it's not necessarily obvious that the reason for this is that they are on the wrong Python version. It would be nice if we could offer an info context on the diagnostic saying "it exists on other Python versions, but you are running with Python 3.9"

We have basic support for this, but there's more we could do. Quoting a comment below: We now:

* Report this precise info when you import a stdlib module we have VERSIONS info for ([18403](github.com/astral-sh/ruff/issues/18403), [20908](https://github.com/astral-sh/ruff/pull/20908))
* Mention what version you're on when you fail to access an attribute on a stdlib module we believe *does* exist in *some* version/platform ([20909](https://github.com/astral-sh/ruff/pull/20909))

We do not:
* Report on attributes of types, optional function arguments, etc.
* Report the exact version(s) needed for attributes
* Report the exact platform(s) needed for attributes (or mention the current platform)

---

_Label `diagnostics` added by @carljm on 2025-05-09 16:09_

---

_Comment by @MichaReiser on 2025-05-09 16:25_

Yeah, that would be awesome. But it might be somewhat involved to implement the detection.

---

_Added to milestone `Beta` by @carljm on 2025-07-25 14:39_

---

_Comment by @carljm on 2025-08-15 15:22_

An easy initial step here would just be to always alert the user to their detected Python version, if an import from stdlib fails.

---

_Assigned to @Gankra by @Gankra on 2025-08-20 13:21_

---

_Comment by @Gankra on 2025-10-16 14:27_

We now:

* Report this precise info when you import a stdlib module we have VERSIONS info for ([18403](github.com/astral-sh/ruff/issues/18403), [20908](https://github.com/astral-sh/ruff/pull/20908))
* Mention what version you're on when you fail to access an attribute on a stdlib module we believe *does* exist in *some* version/platform ([20909](https://github.com/astral-sh/ruff/pull/20909))

We do not:
* Report on attributes of types, optional function arguments, etc.
* Report the exact version(s) needed for attributes
* Report the exact platform(s) needed for attributes (or mention the current platform)

---

_Comment by @Gankra on 2025-10-16 14:29_

If we want to leave this open for further improvements, I think we can downgrade this to GA.

---

_Removed from milestone `Beta` by @Gankra on 2025-10-16 14:29_

---

_Added to milestone `GA` by @Gankra on 2025-10-16 14:29_

---

_Unassigned @Gankra by @Gankra on 2025-10-17 14:27_

---

_Comment by @carljm on 2025-11-13 00:00_

I think what we have here is good, I'm not sure it's a priority to extend it before stable, given everything else we need to do? I haven't seen users running into this since we added the basic stdlib support. Removing this from stable milestone entirely, but @AlexWaygood lmk if you'd like to argue to put it back as a P2.

---

_Removed from milestone `Stable` by @carljm on 2025-11-13 00:00_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:51_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

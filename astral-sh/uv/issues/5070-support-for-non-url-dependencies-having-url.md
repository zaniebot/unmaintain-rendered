```yaml
number: 5070
title: Support for non-URL dependencies having URL dependencies?
type: issue
state: open
author: mchromin
labels:
  - wish
assignees: []
created_at: 2024-07-15T12:20:35Z
updated_at: 2024-07-19T17:23:01Z
url: https://github.com/astral-sh/uv/issues/5070
synced_at: 2026-01-12T15:58:53Z
```

# Support for non-URL dependencies having URL dependencies?

---

_@mchromin_

Hi!

I know documentation states that `uv also makes the assumption that non-URL dependencies won't introduce URL dependencies`, but as we are taking care of around a hundred modules (some of which depends on external URL dependencies) it seems to be a blocker for us as we're not able to install package from registry, which introduces URL dependencies.
I was not able to find more information about this UV assumption - is such support planned to be implemented?
As we have a lot of dependencies between our modules it's impossible for us to add transitive URL dependencies as direct dependencies whenever it's needed.
Is there any way we can use UV in such situation?
Thank you!

---

_Comment by @mchromin on 2024-07-19 09:00_

@charliermarsh Hey, friendly ping ;) 

---

_Comment by @charliermarsh on 2024-07-19 17:05_

Hi @mchromin. We're not actively working on lifting this assumption right now and I don't think it will be prioritized in the near future -- it breaks some assumptions in our resolver and isn't super common. Sorry about that. We can keep the issue open though -- perhaps we'll see more demand and can reconsider.

---

_Label `wish` added by @charliermarsh on 2024-07-19 17:05_

---

_Comment by @slochower on 2024-07-19 17:20_

This also affects almost all of our internal packages and blocks us from using `uv`. Lots of internal packages depend on a core package and the core package depends on a URL dependency.

---

_Comment by @charliermarsh on 2024-07-19 17:21_

That one seems a bit easier: couldn't you just add that URL dependency as a direct dependency? But point taken!

---

_Comment by @slochower on 2024-07-19 17:23_

We could... but lots of packages would have to be updated and managed rather than the core dependency :)

---

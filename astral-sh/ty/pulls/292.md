```yaml
number: 292
title: Reference documentation
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: update/ruff
created_at: 2025-05-09T12:25:33Z
updated_at: 2025-05-12T06:43:19Z
url: https://github.com/astral-sh/ty/pull/292
synced_at: 2026-01-10T02:34:10Z
```

# Reference documentation

---

_Pull request opened by @MichaReiser on 2025-05-09 12:25_

## Summary

Extend the release script to copy the reference documentation to the `docs` folder because GitHub doesn't support relative links into submodules (and I think it's confusing if we redirect users to the ruff repository)


Closes https://github.com/astral-sh/ty/issues/304





---

_Renamed from "Update Ruff commit" to "Reference documentation" by @MichaReiser on 2025-05-09 12:31_

---

_@MichaReiser reviewed on 2025-05-09 12:33_

---

_Review comment by @MichaReiser on `scripts/release.sh`:56 on 2025-05-09 12:33_

@zanieb requesting review for this crime ;)

---

_Review requested from @zanieb by @MichaReiser on 2025-05-09 12:33_

---

_Label `documentation` added by @MichaReiser on 2025-05-09 12:33_

---

_@zanieb reviewed on 2025-05-10 00:00_

---

_Review comment by @zanieb on `scripts/release.sh`:56 on 2025-05-10 00:00_

Ah gee, that's weird. I'll give that some thought — duplicating it seems rather unfortunate.

---

_@zanieb reviewed on 2025-05-10 00:01_

---

_Review comment by @zanieb on `scripts/release.sh`:56 on 2025-05-10 00:01_

This is the generated reference documentation? Why aren't we just generating it here?

---

_Review comment by @zanieb on `scripts/release.sh`:56 on 2025-05-10 00:02_

I think it seems reasonable to just move that generation step from the ruff repository to here and require it on submodule update.

---

_@zanieb reviewed on 2025-05-10 00:02_

---

_@MichaReiser reviewed on 2025-05-10 06:59_

---

_Review comment by @MichaReiser on `scripts/release.sh`:56 on 2025-05-10 06:59_

I like it in the ruff repo because it visualizes the impact of changes.

I also don't think it's that different. It's either copying files (which is super fast) or generating them (which is very slow and requires the script to write outside its root)

So I actually prefer this. Seems simpler overall and doesn't require more time 

---

_@MichaReiser reviewed on 2025-05-10 10:20_

---

_Review comment by @MichaReiser on `scripts/release.sh`:56 on 2025-05-10 10:20_

I'll go and merge this. I understand the concern around duplicating the file in both repositories but I think it's a very minor concern (and comes with trade-offs) and the solution to this is to generate and host our docs outside of github. 

---

_Merged by @MichaReiser on 2025-05-10 10:26_

---

_Closed by @MichaReiser on 2025-05-10 10:26_

---

_Branch deleted on 2025-05-10 10:26_

---

_@zanieb reviewed on 2025-05-10 12:32_

---

_Review comment by @zanieb on `scripts/release.sh`:56 on 2025-05-10 12:32_

For me, it's not about the duplication across repositories — it's about the duplication within this repository where, now, the submodule contents could accidentally drift from the documentation. I think it's minor, but it feels like the sloppy approach. Anyway, easy to fix if it causes tangible problems so 🤷‍♀️ 

---

_@MichaReiser reviewed on 2025-05-12 06:43_

---

_Review comment by @MichaReiser on `scripts/release.sh`:56 on 2025-05-12 06:43_

I see, although it's not clear to me how generating the docs in the ty repository would avoid the "risk of divergence". The docs would diverge when updating the ruff module without regenerating the docs. 

However, we could see the divergence as a feature. It gives us versioned documentation that only gets updated with every release ;)



---

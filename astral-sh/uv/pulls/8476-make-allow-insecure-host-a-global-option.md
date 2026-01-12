```yaml
number: 8476
title: "Make `--allow-insecure-host` a global option"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - breaking
assignees: []
merged: true
base: tracking/050
head: konsti/global-allow-insecure-host
created_at: 2024-10-22T19:27:47Z
updated_at: 2024-10-28T19:46:43Z
url: https://github.com/astral-sh/uv/pull/8476
synced_at: 2026-01-12T16:08:20Z
```

# Make `--allow-insecure-host` a global option

---

_@konstin_

Not verifying the certificates of certain hosts should be supported for all kinds of HTTPS connections, so we're making it a global option, just like native tls. This fixes the remaining places using a client but were not configuring allow insecure host.

Fixes #6983 (i think)
Closes #6983


---

_Review requested from @zanieb by @konstin on 2024-10-22 19:27_

---

_Label `bug` added by @konstin on 2024-10-22 19:30_

---

_@zanieb approved on 2024-10-22 19:31_

---

_@zanieb reviewed on 2024-10-22 19:32_

---

_Review comment by @zanieb on `docs/reference/settings.md`:1508 on 2024-10-22 19:32_

Is this supposed to be gone? I think this is breaking

---

_@zanieb reviewed on 2024-10-22 19:33_

---

_Review comment by @zanieb on `docs/reference/cli.md`:83 on 2024-10-22 19:33_

Why did this disappear from the CLI reference?

---

_Comment by @zanieb on 2024-10-22 19:34_

A couple concerning changes

---

_@konstin reviewed on 2024-10-23 14:16_

---

_Review comment by @konstin on `docs/reference/settings.md`:1508 on 2024-10-23 14:16_

Good catch

---

_Converted to draft by @konstin on 2024-10-23 14:18_

---

_Marked ready for review by @konstin on 2024-10-23 14:31_

---

_@charliermarsh reviewed on 2024-10-23 18:45_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:1535 on 2024-10-23 18:45_

Should we pass in the owned value? We're cloning it at all the sites, aren't we?

---

_@charliermarsh reviewed on 2024-10-23 18:45_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:84 on 2024-10-23 18:45_

I think this should be using `combine` so that we merge the two, right?

---

_@charliermarsh reviewed on 2024-10-23 18:46_

---

_Review comment by @charliermarsh on `docs/reference/settings.md`:336 on 2024-10-23 18:46_

Does this now appear multiple times? I see it's already included twice on main...

---

_@konstin reviewed on 2024-10-23 19:04_

---

_Review comment by @konstin on `docs/reference/settings.md`:336 on 2024-10-23 19:04_

I'm not clear about how the options flow, at least uvx on windows has some duplication (https://github.com/astral-sh/uv/actions/runs/11482045623/job/31954176059?pr=8476), i appreciate guidance on where allow insecure host is supposed to live (or not).

My basic thinking is:
* It needs to be in the global scope
* For backwards compatibility, it also needs to be in the pip scope, and take precedence over the global option.
The latter point seems to thread this option through a lot of other places.

---

_@charliermarsh reviewed on 2024-10-23 23:39_

---

_Review comment by @charliermarsh on `docs/reference/settings.md`:336 on 2024-10-23 23:39_

Ok, I can take a look at how that's popping up here.

---

_Comment by @charliermarsh on 2024-10-25 01:32_

@konstin -- The current version I have here _is_ breaking because I removed the separate setting for the `uv pip` interface. It seems like if a setting is global, it should by definition probably be shared between the two CLIs. What do you think? (We can just ship this with v0.5, it's fine if it's breaking.)

---

_Comment by @konstin on 2024-10-25 07:11_

Shipping this a breaking change with 0.5 sounds fine

---

_Label `breaking` added by @charliermarsh on 2024-10-25 13:05_

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-10-28 19:02_

---

_Renamed from "Make allow insecure host a global option" to "Make `--allow-insecure-host` a global option" by @charliermarsh on 2024-10-28 19:26_

---

_Merged by @charliermarsh on 2024-10-28 19:44_

---

_Closed by @charliermarsh on 2024-10-28 19:44_

---

_Branch deleted on 2024-10-28 19:44_

---

_Comment by @konstin on 2024-10-28 19:46_

Thank you!

---

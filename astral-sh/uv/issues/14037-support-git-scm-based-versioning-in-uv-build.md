```yaml
number: 14037
title: "Support Git scm-based versioning in `uv` build backend"
type: issue
state: open
author: blueraft
labels:
  - enhancement
  - needs-decision
  - build-backend
assignees: []
created_at: 2025-06-14T09:25:44Z
updated_at: 2026-01-12T01:55:51Z
url: https://github.com/astral-sh/uv/issues/14037
synced_at: 2026-01-12T02:26:25Z
```

# Support Git scm-based versioning in `uv` build backend

---

_Issue opened by @blueraft on 2025-06-14 09:25_

### Summary

It would be helpful if `uv` build-backend supported Git-based versioning out of the box, similar to:

- [`setuptools-scm`](https://github.com/pypa/setuptools_scm) – [20k+ references](https://github.com/search?q=tool.setuptools_scm++path%3A**%2Fpyproject.toml&type=code&p=3)
- [`hatch-vcs`](https://github.com/ofek/hatch-vcs) – [6k+ references](https://github.com/search?q=hatch-vcs++path%3A**%2Fpyproject.toml&type=code)
- [`poetry-dynamic-versioning`](https://github.com/mtkennerly/poetry-dynamic-versioning) – [2k+ references](https://github.com/search?q=tool.poetry-dynamic-versioning++path%3A**%2Fpyproject.toml&type=code)

These tools infer versions from Git tags/commits and are widely used in Python packaging workflows.

Related to https://github.com/astral-sh/uv/issues/11718

### Example

_No response_

---

_Label `enhancement` added by @blueraft on 2025-06-14 09:25_

---

_Label `needs-decision` added by @konstin on 2025-06-18 09:08_

---

_Label `build-backend` added by @konstin on 2025-06-26 14:40_

---

_Comment by @paveldikov on 2025-07-05 16:21_

IIRC `pdm-backend` allows for dynamic versions to be wired using a [fairly simple entrypoint-like `package.module:function` syntax](https://backend.pdm-project.org/metadata/#get-with-a-specific-function).

I think such a solution would work very nicely as a MVP (if not even a final solution), as the community can easily come up with a whole host of build system plugins. In fact I reckon a lot of pre-existing plugins could be upcycled with no extra work.

---

_Comment by @winwinashwin on 2025-07-24 08:49_

Hi,

Is this feature on the roadmap? This is our last hurdle in ditching setuptools_scm and completely moving to uv build backend 

---

_Comment by @winwinashwin on 2025-10-10 19:14_

Any update on this feature? 

---

_Comment by @czechnology on 2025-11-11 15:23_

Would be great to see this supported, it would simplify build&deploy pipelines. Calling `uv version` is an extra step which causes changes to the codebase and just unnecessary extra complexity.
It is blocking us from migrating our libraries to `uv_build`. I have also seen OSS projects which are migrating to `uv` but in the process need to use `setuptools` or `hatchling` instead of `uv_build` because of this missing feature, seems like a missed opportunity for uv [build] :)

---

_Comment by @DhavalGojiya on 2025-11-14 20:22_

Eagerly Waiting for this feature.

---

_Comment by @mcarans on 2026-01-12 01:55_

I'm surprised this feature is missing. I was in the process of trying to switch from Hatch to uv and did not expect this hurdle. Now I'm unsure about migrating and considering waiting until Git versioning is available. 

---

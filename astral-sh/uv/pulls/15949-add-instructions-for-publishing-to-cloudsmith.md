```yaml
number: 15949
title: Add instructions for publishing to Cloudsmith
type: pull_request
state: open
author: ndouglas-cloudsmith
labels:
  - documentation
assignees: []
base: main
head: patch-2
created_at: 2025-09-19T13:47:45Z
updated_at: 2025-10-07T19:09:39Z
url: https://github.com/astral-sh/uv/pull/15949
synced_at: 2026-01-10T06:36:15Z
```

# Add instructions for publishing to Cloudsmith

---

_Pull request opened by @ndouglas-cloudsmith on 2025-09-19 13:47_

## Summary
Add instructions for publishing to Cloudsmith into [documentation](https://docs.astral.sh/uv/guides/integration/alternative-indexes/).

Example to push and pull artifacts into Cloudsmith using 'uv': https://docs.cloudsmith.com/formats/python-repository#uv-support

Related Issue: [#15948](https://github.com/astral-sh/uv/issues/15948)

## Test Plan
I ran the documentation locally to confirm it works

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:392 on 2025-09-19 18:29_

I think if you want to include this it should be as `default = true` in the `pyproject.toml` example

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:393 on 2025-09-19 18:30_

Should this be a pull command in this example? e.g., `uv sync`?

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:404 on 2025-09-19 18:31_

Maybe we should move these down to before the `publish` command?

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:411 on 2025-09-19 18:31_

We should use `uv build` here

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:414 on 2025-09-19 18:31_

We try to avoid saying "just" in this way.

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:371 on 2025-09-19 18:33_

Can you do a pass on the style and structure of this content with a focus on having it be similar to the existing ones?

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:408 on 2025-09-19 18:34_

I don't think we refer to building elsewhere in this guide, so I might just drop it?

We could link out to https://docs.astral.sh/uv/guides/package/ though.

---

_Comment by @zanieb on 2025-09-19 18:34_

Thanks for taking the time to put up a pull request!

---

_@zanieb requested changes on 2025-09-19 19:35_

---

_Comment by @konstin on 2025-09-23 12:13_

Not exactly this PR, but I've been looking at the https://docs.cloudsmith.com/formats/python-repository#uv-support you linked, and `python setup.py bdist_wheel --universal` is deprecated (https://packaging.python.org/en/latest/discussions/setup-py-deprecated/). The standard way of building a package is now `python -m build` (or of course  our re-implementation, `uv build` ;) )

---

_Label `documentation` added by @konstin on 2025-09-25 15:21_

---

_Renamed from "Docs: add instructions for publishing to Cloudsmith" to "Add instructions for publishing to Cloudsmith" by @konstin on 2025-09-25 15:21_

---

_Comment by @pabloopez on 2025-09-26 08:54_

hi @zanieb and @konstin,
Thank you for kindly reviewing this PR! I think I addressed all your comments Zanie, including the structure matching you suggested to resembled other entries in this page.

I also tested with my own repo that all changes are working fine. This should be ready to merge, whenever you validate all new changes. 

Thanks all!

cc @ndouglas-cloudsmith 

---

_@pabloopez reviewed on 2025-10-07 08:34_

---

_Review comment by @pabloopez on `docs/guides/integration/alternative-indexes.md`:371 on 2025-10-07 08:34_

Hi @zanieb, could you kindly confirm if the new suggested changes are aligned with the current structure and the other feedback in this PR? 

Thanks for your time, happy to make any other changes if required. 

---

_Comment by @zanieb on 2025-10-07 19:09_

Thanks for addressing the feedback! I'll give it another review soon.

---

_Assigned to @zanieb by @zanieb on 2025-10-07 19:09_

---

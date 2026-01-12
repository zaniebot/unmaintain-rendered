```yaml
number: 6260
title: Bump version to 0.3.0
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/030
created_at: 2024-08-20T16:50:13Z
updated_at: 2024-08-20T17:30:00Z
url: https://github.com/astral-sh/uv/pull/6260
synced_at: 2026-01-12T16:07:18Z
```

# Bump version to 0.3.0

---

_@zanieb_

[Rendered](https://github.com/astral-sh/uv/blob/zb/030/CHANGELOG.md#030)

---

_Label `release` added by @zanieb on 2024-08-20 16:50_

---

_Renamed from "zb/030" to "Bump version to 0.3.0" by @zanieb on 2024-08-20 16:50_

---

_@charliermarsh reviewed on 2024-08-20 16:55_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:6 on 2024-08-20 16:55_

Maybe "standard" instead of "normal".

---

_@charliermarsh reviewed on 2024-08-20 16:57_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:29 on 2024-08-20 16:57_

```suggestion
- Remove `--legacy-setup-py` command-line argument ([#4255](https://github.com/astral-sh/uv/pull/4255))
```

---

_Review comment by @charliermarsh on `CHANGELOG.md`:70 on 2024-08-20 16:58_

```suggestion
- Respect release-only semantics of `python_full_version` when constructing markers ([#6171](https://github.com/astral-sh/uv/pull/6171))
```

---

_@charliermarsh reviewed on 2024-08-20 16:58_

---

_@charliermarsh reviewed on 2024-08-20 17:06_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:12 on 2024-08-20 17:06_

Lets put this first, IMO. Or even its own paragraph before a list of items.

---

_Review comment by @konstin on `CHANGELOG.md`:5 on 2024-08-20 17:06_

What about "This release stabilizes a set of new features in uv. See our [blog post](#) for an overview. All features previously in preview are now stable. In brief, this means that:"? I want to avoid the impression that you needed to have followed the preview development for the stable changelog to make sense (something that other python projects have done)

---

_Review comment by @konstin on `CHANGELOG.md`:29 on 2024-08-20 17:06_

```suggestion
- Move concurrency settings to top-level ([#4257](https://github.com/astral-sh/uv/pull/4257))
```

---

_@charliermarsh approved on 2024-08-20 17:07_

---

_Review comment by @konstin on `CHANGELOG.md`:38 on 2024-08-20 17:09_

It's odd that these mismatch the actual big improvement we've done, we should give some indication that this changelog is different because all the actual enhancements are in the blog post and docs, maybe up in the into paragraph

---

_@konstin approved on 2024-08-20 17:10_

---

_@zanieb reviewed on 2024-08-20 17:11_

---

_Review comment by @zanieb on `CHANGELOG.md`:5 on 2024-08-20 17:11_

This changed now — but yes we can and should link to the blog post.

---

_@charliermarsh reviewed on 2024-08-20 17:12_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:5 on 2024-08-20 17:12_

"This release introduces the...", maybe?

---

_@zanieb reviewed on 2024-08-20 17:13_

---

_Review comment by @zanieb on `CHANGELOG.md`:38 on 2024-08-20 17:13_

I don't think the breaking changes list being short is problematic. I mean, we can enumerate every new command but this is better done in the documentation. 

---

_@zanieb reviewed on 2024-08-20 17:17_

---

_Review comment by @zanieb on `CHANGELOG.md`:5 on 2024-08-20 17:17_

Tweaked

---

_Review comment by @konstin on `CHANGELOG.md`:38 on 2024-08-20 17:18_

I meant it the other way round: Tell the user that the changelog below doesn't represent all the enhancement made (during the preview phase). Normally, i'd go to the changelog to learn about all the new features and changes i want to make, but for this release you should go to the blog post and then the docs. Something like saying "The changelog below includes only the changes made to the previously-preview features since the last release. The changes we made are too large to list them here, please refer to the blog post and the docs to learn about the new uv interface"

---

_@konstin reviewed on 2024-08-20 17:18_

---

_@charliermarsh reviewed on 2024-08-20 17:20_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:24 on 2024-08-20 17:20_

Not sure on this second sentence, I'd probably remove it.

---

_@zanieb reviewed on 2024-08-20 17:22_

---

_Review comment by @zanieb on `CHANGELOG.md`:38 on 2024-08-20 17:22_

Done. Will link to the blog when it's published in a fast-follow.

---

_Merged by @zanieb on 2024-08-20 17:29_

---

_Closed by @zanieb on 2024-08-20 17:29_

---

_Branch deleted on 2024-08-20 17:30_

---

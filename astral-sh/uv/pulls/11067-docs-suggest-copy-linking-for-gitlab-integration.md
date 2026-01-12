```yaml
number: 11067
title: "docs: suggest copy linking for GitLab integration guide"
type: pull_request
state: merged
author: fgreinacher
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/gitlab-ci
created_at: 2025-01-29T16:08:48Z
updated_at: 2025-01-30T07:09:33Z
url: https://github.com/astral-sh/uv/pull/11067
synced_at: 2026-01-12T16:09:39Z
```

# docs: suggest copy linking for GitLab integration guide

---

_@fgreinacher_

## Summary

Hardlinking does not work in that context and raises a warning.

Setting the link mode to copy makes the warning go away.

## Test Plan

Tested on gitlab.com and our self-hosted GitLab instance.

Before changing the link mode:

https://gitlab.com/fgreinacher/uv-test/-/jobs/8986967570

After changing the link mode:

https://gitlab.com/fgreinacher/uv-test/-/jobs/8987026307.

:hammer_and_pick: with :heart: by [Siemens](https://opensource.siemens.com/)


---

_Review comment by @fgreinacher on `docs/guides/integration/gitlab.md`:21 on 2025-01-29 16:10_

I have removed the `stage` configuration because it is not needed for this example.

---

_@fgreinacher reviewed on 2025-01-29 16:10_

---

_@zanieb reviewed on 2025-01-29 16:22_

---

_Review comment by @zanieb on `docs/guides/integration/gitlab.md`:21 on 2025-01-29 16:22_

Can we add a comment above this suggesting why it's used?

---

_@zanieb reviewed on 2025-01-29 16:23_

---

_Review comment by @zanieb on `docs/guides/integration/gitlab.md`:21 on 2025-01-29 16:23_

(Above `UV_LINK_MODE`)

---

_@fgreinacher reviewed on 2025-01-29 16:39_

---

_Review comment by @fgreinacher on `docs/guides/integration/gitlab.md`:21 on 2025-01-29 16:39_

Sure, done in [263d1e7](https://github.com/astral-sh/uv/pull/11067/commits/263d1e7fb6aa8da9a90214a508bf72842295acb7).

---

_Review requested from @zanieb by @fgreinacher on 2025-01-29 16:39_

---

_@zanieb reviewed on 2025-01-29 17:56_

---

_Review comment by @zanieb on `docs/guides/integration/gitlab.md`:20 on 2025-01-29 17:56_

```suggestion
  # so we need to copy instead of using hard links.
```

---

_Label `documentation` added by @zanieb on 2025-01-29 17:57_

---

_@zanieb approved on 2025-01-29 17:57_

---

_Merged by @zanieb on 2025-01-29 17:58_

---

_Closed by @zanieb on 2025-01-29 17:58_

---

_Branch deleted on 2025-01-30 07:09_

---

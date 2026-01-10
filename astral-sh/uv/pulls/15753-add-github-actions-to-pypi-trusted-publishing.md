```yaml
number: 15753
title: Add GitHub Actions to PyPI trusted publishing example
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/add-trusted-publishing-example
created_at: 2025-09-09T14:43:17Z
updated_at: 2025-09-17T17:25:19Z
url: https://github.com/astral-sh/uv/pull/15753
synced_at: 2026-01-10T06:36:15Z
```

# Add GitHub Actions to PyPI trusted publishing example

---

_Pull request opened by @konstin on 2025-09-09 14:43_

Add a complete example for the most common publishing workflow, GitHub Actions to PyPI, with screenshots for settings and a standalone companion repo.

Closes #14398


---

_Review requested from @zanieb by @konstin on 2025-09-09 14:43_

---

_Label `documentation` added by @konstin on 2025-09-09 14:43_

---

_@konstin reviewed on 2025-09-09 14:43_

---

_Review comment by @konstin on `docs/guides/package.md`:128 on 2025-09-09 14:43_

The guide could arguably be in either document, it's the intersection of GitHub and building&publishing.

---

_@konstin reviewed on 2025-09-09 14:44_

---

_Review comment by @konstin on `docs/guides/integration/github.md`:397 on 2025-09-09 14:44_

It's a bit annoying that the main column size is unsteady and resizes the images, the text can get a blurry look from this.

---

_Review comment by @zsol on `docs/guides/integration/github.md`:397 on 2025-09-16 15:21_

is there an equivalent `gh` command we could list here?

---

_Review comment by @zsol on `docs/guides/integration/github.md`:409 on 2025-09-16 15:23_

Add a note about the tag naming pattern?
```suggestion
Finally, tag a release (make sure it starts with `v` to match the pattern in the workflow) and push it:
```

---

_@zsol approved on 2025-09-16 15:23_

---

_@konstin reviewed on 2025-09-16 16:12_

---

_Review comment by @konstin on `docs/guides/integration/github.md`:397 on 2025-09-16 16:12_

Doesn't seem like it https://github.com/cli/cli/issues/5149

---

_Merged by @konstin on 2025-09-17 17:25_

---

_Closed by @konstin on 2025-09-17 17:25_

---

_Branch deleted on 2025-09-17 17:25_

---

```yaml
number: 15966
title: Expand the contribution docs on issue selection
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/contributing-docs
created_at: 2025-09-20T18:05:38Z
updated_at: 2025-09-25T08:16:35Z
url: https://github.com/astral-sh/uv/pull/15966
synced_at: 2026-01-12T16:12:02Z
```

# Expand the contribution docs on issue selection

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2025-09-20 18:05_

---

_@geofft approved on 2025-09-22 14:03_

---

_@sharkdp approved on 2025-09-22 14:06_

---

_Review comment by @AlexWaygood on `CONTRIBUTING.md`:31 on 2025-09-22 14:10_

I think for Ruff/ty I'd probably go for slightly softer language here (since even a new lint rule counts as a "feature" in Ruff, and we generally don't insta-close those) -- something like this?

```suggestion
Please do not open pull requests for large new features without prior discussion. While we appreciate
exploration of new features, we will often close these pull requests immediately. Adding a
large new feature to uv creates a long-term maintenance burden and requires strong consensus from the uv
team before it is appropriate to begin work on an implementation.
```

But I can also see that the concerns might be different in the different projects -- we might want slightly different language in the equivalent sections in different repos, and that seems fine

---

_@AlexWaygood approved on 2025-09-22 14:10_

This is great -- thanks for doing this!

---

_@zanieb reviewed on 2025-09-22 14:12_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:31 on 2025-09-22 14:12_

Yeah I think I'd prefer to leave this as-is for uv but it makes sense to modify for ty and ruff.

---

_Merged by @zanieb on 2025-09-22 20:57_

---

_Closed by @zanieb on 2025-09-22 20:57_

---

_Branch deleted on 2025-09-22 20:57_

---

_@MichaReiser reviewed on 2025-09-25 08:16_

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:31 on 2025-09-25 08:16_

I think we're strict enough about adding new lint rules that I highly recommend opening an issue before creating a PR. We should probably be more strict about closing PRs for Ruff.

---

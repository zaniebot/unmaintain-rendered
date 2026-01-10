```yaml
number: 729
title: Copy basic template and styling for documentation from uv
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: feat/docs
head: zb/docs
created_at: 2025-06-30T22:15:11Z
updated_at: 2025-07-01T19:14:58Z
url: https://github.com/astral-sh/ty/pull/729
synced_at: 2026-01-10T02:34:10Z
```

# Copy basic template and styling for documentation from uv

---

_Pull request opened by @zanieb on 2025-06-30 22:15_

This copies the documentation scaffolding from uv, doing the minimum amount of work to make it viable.

---

_Label `documentation` added by @zanieb on 2025-06-30 22:15_

---

_Label `do-not-merge` added by @zanieb on 2025-07-01 16:02_

---

_Comment by @zanieb on 2025-07-01 16:02_

We'll want to make sure we've updated the README and such and that the new documentation is ready before merging this because it'll regress the current documentation experience.

---

_Marked ready for review by @zanieb on 2025-07-01 16:02_

---

_Label `do-not-merge` removed by @zanieb on 2025-07-01 16:14_

---

_Comment by @zanieb on 2025-07-01 16:14_

I've based this on #744 so we can merge it and use separate pull requests for additional changes.

---

_@zanieb reviewed on 2025-07-01 16:18_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:108 on 2025-07-01 16:18_

@sharkdp this is the easiest way to run the documentation locally

---

_Review comment by @zanieb on `CONTRIBUTING.md`:133 on 2025-07-01 16:18_

We might want to drop this in favor of pre-commit? We don't have pre-commit in uv.

---

_@zanieb reviewed on 2025-07-01 16:18_

---

_Review comment by @zanieb on `mkdocs.template.yml`:5 on 2025-07-01 16:19_

We need to add `docs/assets` for ty

---

_@zanieb reviewed on 2025-07-01 16:19_

---

_@zanieb reviewed on 2025-07-01 16:20_

---

_Review comment by @zanieb on `mkdocs.template.yml`:5 on 2025-07-01 16:20_

(It appears this doesn't error so we can do so as a follow-up)

---

_Review comment by @zanieb on `mkdocs.template.yml`:92 on 2025-07-01 16:21_

I'm not sure if we need to redo this or not, most of these probably are reference items?

---

_@zanieb reviewed on 2025-07-01 16:21_

---

_Comment by @zanieb on 2025-07-01 16:54_

This can be previewed at https://d15f5491.docs-119.pages.dev/ty/

---

_@zanieb reviewed on 2025-07-01 16:54_

---

_Review comment by @zanieb on `mkdocs.template.yml`:92 on 2025-07-01 16:54_

I think the main step will be splitting up the "Overview" into separate pages.

---

_@zanieb reviewed on 2025-07-01 17:01_

---

_Review comment by @zanieb on `mkdocs.template.yml`:5 on 2025-07-01 17:01_

See

- #746
- #747

---

_Label `internal` added by @zanieb on 2025-07-01 17:04_

---

_Label `documentation` removed by @zanieb on 2025-07-01 17:04_

---

_@zanieb reviewed on 2025-07-01 17:25_

---

_Review comment by @zanieb on `mkdocs.template.yml`:92 on 2025-07-01 17:25_

Started in https://github.com/astral-sh/ty/pull/748

---

_Review comment by @sharkdp on `docs/stylesheets/extra.css`:115 on 2025-07-01 18:52_

Not that it matters much...
```suggestion
/* Omits the nav title "ty" entirely unless on a small screen, in which case
```

---

_Review comment by @sharkdp on `CONTRIBUTING.md`:108 on 2025-07-01 18:55_

Works great!

---

_Review comment by @sharkdp on `CONTRIBUTING.md`:133 on 2025-07-01 18:58_

Makes sense to me. We also auto-format our Markdown tests for ty using a pre-commit hook in the ruff repo. I added this as a task to https://github.com/astral-sh/ty/pull/744

---

_@sharkdp approved on 2025-07-01 18:59_

Thank you!

---

_Merged by @zanieb on 2025-07-01 19:14_

---

_Closed by @zanieb on 2025-07-01 19:14_

---

_Branch deleted on 2025-07-01 19:14_

---

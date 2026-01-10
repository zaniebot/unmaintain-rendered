```yaml
number: 748
title: "Naively split the \"Overview\" page into child pages"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: feat/docs
head: zb/docs-ii-nav
created_at: 2025-07-01T17:24:22Z
updated_at: 2025-07-02T13:25:09Z
url: https://github.com/astral-sh/ty/pull/748
synced_at: 2026-01-10T02:34:10Z
```

# Naively split the "Overview" page into child pages

---

_Pull request opened by @zanieb on 2025-07-01 17:24_

I don't think this is "good", but it splits each top-level heading of the existing documentation into a dedicated page as a starting point.

---

_Review comment by @zanieb on `docs/configuration.md`:30 on 2025-07-01 17:33_

We'll need to rewrite these to use `!!! note` admonitions instead. This is a follow-up task noted in #744 

---

_@zanieb reviewed on 2025-07-01 17:33_

---

_@zanieb reviewed on 2025-07-01 17:33_

---

_Review comment by @zanieb on `mkdocs.template.yml`:86 on 2025-07-01 17:33_

We'll want an `index.md` here eventually so the header is clickable like the other ones, but we'll want to settle on the content first to avoid churn.

---

_@sharkdp reviewed on 2025-07-01 19:11_

---

_Review comment by @sharkdp on `docs/configuration.md`:30 on 2025-07-01 19:11_

I can take care of this tomorrow.

---

_Review comment by @sharkdp on `docs/index.md`:312 on 2025-07-02 10:05_

This chapter was missing completely, added.

---

_@sharkdp reviewed on 2025-07-02 10:05_

---

_Marked ready for review by @sharkdp on 2025-07-02 10:07_

---

_@sharkdp approved on 2025-07-02 10:08_

Thank you!

---

_Merged by @sharkdp on 2025-07-02 10:09_

---

_Closed by @sharkdp on 2025-07-02 10:09_

---

_Branch deleted on 2025-07-02 10:09_

---

_@zanieb reviewed on 2025-07-02 13:25_

---

_Review comment by @zanieb on `docs/index.md`:312 on 2025-07-02 13:25_

Oh tragic, it's sitting untracked in my repo — not sure why that one got missed. Thanks for catching it!

---

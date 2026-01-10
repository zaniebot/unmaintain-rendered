```yaml
number: 11753
title: Remove the last edited date from documentation pages
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/edited
created_at: 2025-02-24T18:55:24Z
updated_at: 2025-02-24T19:27:03Z
url: https://github.com/astral-sh/uv/pull/11753
synced_at: 2026-01-10T11:10:38Z
```

# Remove the last edited date from documentation pages

---

_Pull request opened by @zanieb on 2025-02-24 18:55_

I am bothered by the positioning of this immediately following the content. I explored some other things, like forcing it the bottom of the article, but in the end it was easiest to just hide it entirely

I think this belongs somewhere else, like in the footer — but I believe that requires theme changes which are a bit more complicated than its worth. https://timvink.github.io/mkdocs-git-revision-date-localized-plugin/howto/override-a-theme/

The main goal here was SEO metadata anyway.

Originally added in https://github.com/astral-sh/uv/pull/11164

Before

<img width="1334" alt="Screenshot 2025-02-24 at 12 57 56 PM" src="https://github.com/user-attachments/assets/3f7423ff-fc18-40e8-be8a-f2e611af8221" />

Now, it's omitted.



---

_Label `documentation` added by @zanieb on 2025-02-24 18:55_

---

_Marked ready for review by @zanieb on 2025-02-24 18:58_

---

_@charliermarsh approved on 2025-02-24 19:04_

---

_Merged by @zanieb on 2025-02-24 19:27_

---

_Closed by @zanieb on 2025-02-24 19:27_

---

_Branch deleted on 2025-02-24 19:27_

---

```yaml
number: 3187
title: Fix Markdown errors in docs
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-mkdocs-headers
created_at: 2023-02-23T19:09:47Z
updated_at: 2023-02-24T18:29:10Z
url: https://github.com/astral-sh/ruff/pull/3187
synced_at: 2026-01-12T04:39:44Z
```

# Fix Markdown errors in docs

---

_Pull request opened by @JonathanPlasse on 2023-02-23 19:09_

Add missing titles
Fix headers depth
Remove trailing empty lines

Before:
![before](https://live.staticflickr.com/65535/52706952594_51598c1f6f_b.jpg)

After:
![after](https://live.staticflickr.com/65535/52706705181_b0a55846e5_b.jpg)

---

_@charliermarsh reviewed on 2023-02-23 21:28_

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:41 on 2023-02-23 21:28_

What's this condition here?

---

_@JonathanPlasse reviewed on 2023-02-23 22:02_

---

_Review comment by @JonathanPlasse on `scripts/generate_mkdocs.py`:41 on 2023-02-23 22:02_

This was used for the acknowledgment section.

---

_@JonathanPlasse reviewed on 2023-02-23 22:03_

---

_Review comment by @JonathanPlasse on `scripts/generate_mkdocs.py`:41 on 2023-02-23 22:03_

Which is not included anymore in the documentation.
Should it be added back?

---

_@charliermarsh reviewed on 2023-02-23 22:04_

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:41 on 2023-02-23 22:04_

Oh, I think it's fine for that to exist in the README but not the docs. Same with "Who's Using Ruff".

---

_Review comment by @JonathanPlasse on `scripts/generate_mkdocs.py`:41 on 2023-02-23 22:22_

I thought you mentioned the `if not lines[0].startswith("# "):` that was removed in the last commit.
This one is used to avoid cleaning `Overview` which is already without error and does not need to change the header's depth.

---

_@JonathanPlasse reviewed on 2023-02-23 22:22_

---

_Comment by @JonathanPlasse on 2023-02-24 17:13_

- Is it possible to merge this before #3191?
- I would then rebase #3191 and make sure there is no markdown error left.

---

_Merged by @charliermarsh on 2023-02-24 18:06_

---

_Closed by @charliermarsh on 2023-02-24 18:06_

---

_Branch deleted on 2023-02-24 18:29_

---

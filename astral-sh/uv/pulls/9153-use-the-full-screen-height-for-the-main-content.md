```yaml
number: 9153
title: Use the full screen height for the main content to stabilize the nav
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-nav-height
created_at: 2024-11-15T16:14:51Z
updated_at: 2024-11-19T19:53:27Z
url: https://github.com/astral-sh/uv/pull/9153
synced_at: 2026-01-10T12:00:00Z
```

# Use the full screen height for the main content to stabilize the nav

---

_Pull request opened by @zanieb on 2024-11-15 16:14_

On large screens, we require scrolling below the fold for the next page / prev page navigation footer. This dramatically improves visibility of the left nav when looking at small pages like section overviews. Critically, this stops the height of the navigation from jumping around depending on the page you're on. On small screens, the positioning is unchanged since the nav is in a hamburger menu and it'd be annoying to scroll.

Eventually, we could move the next / prev nav out of the footer and into the content, e.g., as in https://github.com/astral-sh/uv/pull/9121#issuecomment-2479282706.

These images don't quite do the change in experience justice. It's the consistency when changing pages that feels the most different.

Before

<img width="1484" alt="Screenshot 2024-11-15 at 10 16 30 AM" src="https://github.com/user-attachments/assets/e0729691-31ea-46cc-9679-636fb144eab7">

After

<img width="1474" alt="Screenshot 2024-11-15 at 10 15 26 AM" src="https://github.com/user-attachments/assets/d01ae5cd-1347-45de-a294-fbd56b2d6fb5">


---

_Label `documentation` added by @zanieb on 2024-11-15 16:18_

---

_Comment by @zanieb on 2024-11-15 16:36_

cc @dhruvmanila 

---

_@dhruvmanila approved on 2024-11-18 05:38_

This is much better, thanks!

I haven't looked at the code changes (requires rebasing), but ran it locally to experience it. I can look at it when rebased but feel free to merge without it.

---

_Comment by @zanieb on 2024-11-18 15:29_

Ah sorry, rebased.

---

_@dhruvmanila approved on 2024-11-18 18:35_

Thanks!

---

_Merged by @zanieb on 2024-11-19 19:53_

---

_Closed by @zanieb on 2024-11-19 19:53_

---

_Branch deleted on 2024-11-19 19:53_

---

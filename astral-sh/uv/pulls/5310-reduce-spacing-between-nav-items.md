```yaml
number: 5310
title: Reduce spacing between nav items
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-nav
created_at: 2024-07-22T20:56:23Z
updated_at: 2024-07-23T16:50:03Z
url: https://github.com/astral-sh/uv/pull/5310
synced_at: 2026-01-10T13:37:23Z
```

# Reduce spacing between nav items

---

_Pull request opened by @zanieb on 2024-07-22 20:56_

**Before**
<img width="1475" alt="Screenshot 2024-07-22 at 3 56 46 PM" src="https://github.com/user-attachments/assets/a9c6b929-0569-45a6-9840-d49d955dd55a">

--------

**After**
<img width="1475" alt="Screenshot 2024-07-22 at 3 56 38 PM" src="https://github.com/user-attachments/assets/98c85e03-fe62-4894-99f6-a49293eacf0b">

---

_Label `documentation` added by @zanieb on 2024-07-22 20:56_

---

_Label `preview` added by @zanieb on 2024-07-22 20:56_

---

_Marked ready for review by @zanieb on 2024-07-22 20:58_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-22 21:05_

---

_Comment by @zanieb on 2024-07-22 21:06_

This gets us 6 more items above the fold.

---

_@charliermarsh reviewed on 2024-07-22 21:18_

---

_Review comment by @charliermarsh on `docs/stylesheets/extra.css`:114 on 2024-07-22 21:18_

I'll admit that I'm hesitant to add stuff like this for a few reasons: (1) we need to test it against all responsive breakpoints, not just on desktop; (2) it's not robust to updates to MkDocs or Material for MkDocs (what if they change the styling, or even the class names -- we'd have no way of knowing that this broke other than visual inspection).


---

_Review comment by @zanieb on `docs/stylesheets/extra.css`:114 on 2024-07-22 21:23_

I understand the concern, but the default navigation is going to be a problem. I'm not sure what to suggest as an alternative.

---

_@zanieb reviewed on 2024-07-22 21:23_

---

_@zanieb reviewed on 2024-07-22 21:27_

---

_Review comment by @zanieb on `docs/stylesheets/extra.css`:113 on 2024-07-22 21:27_

Note this specific change was to prevent the right-side TOC from also being condensed.

---

_@charliermarsh reviewed on 2024-07-22 21:29_

---

_Review comment by @charliermarsh on `docs/stylesheets/extra.css`:114 on 2024-07-22 21:29_

Ok. There is a very good chance that this will silently break at some point. But I think you hear me.

At the very least, could I request that you: (1) add comments to each change in the CSS, and (2) add screenshots for all breakpoints?


---

_@zanieb reviewed on 2024-07-22 21:36_

---

_Review comment by @zanieb on `docs/stylesheets/extra.css`:114 on 2024-07-22 21:36_

I definitely hear you. I'm happy to take an alternative approach if there is one. Otherwise, I think this is (1) not a huge deal if there's a regression, though I could see how changes could interact badly if / when we have more custom CSS (2) a temporary solution until we revisit documentation rendering as a whole.

---

_@zanieb reviewed on 2024-07-22 21:37_

---

_Review comment by @zanieb on `docs/stylesheets/extra.css`:114 on 2024-07-22 21:37_

(Sent a DM about the breakpoints)

---

_@charliermarsh approved on 2024-07-22 22:23_

---

_Comment by @zanieb on 2024-07-22 22:39_

<img width="1475" alt="Screenshot 2024-07-22 at 5 25 20 PM" src="https://github.com/user-attachments/assets/27e0d9f0-b395-40dd-9a2a-5f82ff4544be">
<img width="1475" alt="Screenshot 2024-07-22 at 5 25 30 PM" src="https://github.com/user-attachments/assets/6b9d88cc-529c-4ece-9e04-2231a5ea44ca">
<img width="1475" alt="Screenshot 2024-07-22 at 5 25 39 PM" src="https://github.com/user-attachments/assets/d13f5ec9-4eab-435c-ab0b-9cbe566fbfbd">
<img width="1475" alt="Screenshot 2024-07-22 at 5 39 04 PM" src="https://github.com/user-attachments/assets/48ac768f-3645-42f0-b184-609cf2571282">


---

_Comment by @zanieb on 2024-07-22 22:39_

Dropped #5130 from here — it's more complicated. The useless "uv" title on the navbar remains.

---

_Merged by @zanieb on 2024-07-23 16:50_

---

_Closed by @zanieb on 2024-07-23 16:50_

---

_Branch deleted on 2024-07-23 16:50_

---

```yaml
number: 9095
title: Update styles to support collapsible nav
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/docs-collapsible
created_at: 2024-11-13T18:44:52Z
updated_at: 2024-11-15T23:00:25Z
url: https://github.com/astral-sh/uv/pull/9095
synced_at: 2026-01-12T16:08:39Z
```

# Update styles to support collapsible nav

---

_@zanieb_

In preparation for adding collapsible sections, cleans up the CSS styling we apply to the nav.

The differences here are very subtle without collapsible sections enabled. Note these screenshots show an increase in the secondary nav font size (table of contents) which I subsequently fixed.

Before

<img width="1169" alt="Screenshot 2024-11-13 at 12 45 49 PM" src="https://github.com/user-attachments/assets/10ea345f-73d3-430c-a6f0-8448e2471b60">

After

<img width="1169" alt="Screenshot 2024-11-13 at 12 45 29 PM" src="https://github.com/user-attachments/assets/6c1af159-1684-4d00-b33d-c0a88b5e0aa9">

However, with collapsible sections you can see the bugs we're addressing

Before

<img width="1169" alt="Screenshot 2024-11-13 at 12 48 19 PM" src="https://github.com/user-attachments/assets/7902791f-6c4d-4c84-b366-ba9c4bd96214">

After

<img width="1169" alt="Screenshot 2024-11-13 at 12 47 30 PM" src="https://github.com/user-attachments/assets/6e0c4c7c-4a41-4dd5-a9f1-beabff965dbb">



---

_Label `internal` added by @zanieb on 2024-11-13 18:44_

---

_Comment by @zanieb on 2024-11-13 18:52_

The small screen experience remains unchanged

<img width="1169" alt="Screenshot 2024-11-13 at 12 51 54 PM" src="https://github.com/user-attachments/assets/f5d5fccf-55e2-482a-97c9-70b8232d029d">


---

_@charliermarsh approved on 2024-11-15 04:01_

---

_@charliermarsh reviewed on 2024-11-15 04:02_

---

_Review comment by @charliermarsh on `docs/stylesheets/extra.css`:120 on 2024-11-15 04:02_

Where does this value come from?

---

_@charliermarsh approved on 2024-11-15 04:02_

---

_@zanieb reviewed on 2024-11-15 04:36_

---

_Review comment by @zanieb on `docs/stylesheets/extra.css`:120 on 2024-11-15 04:36_

This value is from the main stylesheet, I figured it'd be best to match it though I think it is equivalent for most purposes?

---

_Merged by @zanieb on 2024-11-15 23:00_

---

_Closed by @zanieb on 2024-11-15 23:00_

---

_Branch deleted on 2024-11-15 23:00_

---

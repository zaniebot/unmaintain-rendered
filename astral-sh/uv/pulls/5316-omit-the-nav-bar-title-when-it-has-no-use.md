```yaml
number: 5316
title: Omit the nav bar title when it has no use
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-nav-title
created_at: 2024-07-22T22:46:32Z
updated_at: 2024-07-31T15:08:24Z
url: https://github.com/astral-sh/uv/pull/5316
synced_at: 2026-01-12T16:06:45Z
```

# Omit the nav bar title when it has no use

---

_@zanieb_

Turns out this is needed for navigation on mobile, but useless on larger screens. 

Closes #5130

<img width="1551" alt="Screenshot 2024-07-22 at 5 47 49 PM" src="https://github.com/user-attachments/assets/2173549e-e65d-4691-be83-5e3bf0191dd5">
<img width="1551" alt="Screenshot 2024-07-22 at 5 48 02 PM" src="https://github.com/user-attachments/assets/81012ba8-ce85-4276-8ffa-5c7ef556c389">
<img width="1551" alt="Screenshot 2024-07-22 at 5 48 08 PM" src="https://github.com/user-attachments/assets/ecf8fcd7-e5d4-45c4-8d46-d09d91a8bbe9">


---

_Label `documentation` added by @zanieb on 2024-07-22 22:46_

---

_Label `preview` added by @zanieb on 2024-07-22 22:46_

---

_Renamed from "Omit the nav bar title" to "Omit the nav bar title when it has no use" by @zanieb on 2024-07-22 22:46_

---

_Comment by @zanieb on 2024-07-22 22:50_

A little hesitant on the trade-off here (compared to something more impactful like #5310)

---

_Review requested from @charliermarsh by @zanieb on 2024-07-23 16:50_

---

_Marked ready for review by @zanieb on 2024-07-23 16:50_

---

_Comment by @zanieb on 2024-07-23 20:36_

Note without this there is a slight blur from the title element...

<img width="251" alt="Screenshot 2024-07-23 at 3 36 03 PM" src="https://github.com/user-attachments/assets/0bf6edcd-a73d-4659-8f26-98a9862ed352">


---

_Comment by @zanieb on 2024-07-30 21:15_

With #5628 I feel like this is even more relevant.

---

_@charliermarsh approved on 2024-07-31 14:31_

---

_Merged by @zanieb on 2024-07-31 15:08_

---

_Closed by @zanieb on 2024-07-31 15:08_

---

_Branch deleted on 2024-07-31 15:08_

---

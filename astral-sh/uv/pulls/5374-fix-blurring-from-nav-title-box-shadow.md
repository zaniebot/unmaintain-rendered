```yaml
number: 5374
title: Fix blurring from nav title box shadow
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/nav-shadow
created_at: 2024-07-23T20:54:52Z
updated_at: 2024-07-23T22:33:59Z
url: https://github.com/astral-sh/uv/pull/5374
synced_at: 2026-01-12T16:06:47Z
```

# Fix blurring from nav title box shadow

---

_@zanieb_

Fixes blur noted in https://github.com/astral-sh/uv/pull/5316 but doesn't drop the title entirely. https://github.com/astral-sh/uv/pull/5316 is my preferred design, if the implementation was cost-free.


<img width="1259" alt="Screenshot 2024-07-23 at 3 53 48 PM" src="https://github.com/user-attachments/assets/6f9b828b-884f-447d-8508-ba4023152e2f">

(nothing to see at the rest of the breakpoints >1220)

<img width="1259" alt="Screenshot 2024-07-23 at 3 53 56 PM" src="https://github.com/user-attachments/assets/b892cd76-cd91-4e78-b8c8-58e16a8b1130">


---

_Label `documentation` added by @zanieb on 2024-07-23 20:54_

---

_Label `preview` added by @zanieb on 2024-07-23 20:54_

---

_@charliermarsh approved on 2024-07-23 22:28_

I think it'd be helpful to include one "Before" pic so we can have a "Before" vs. "After". As-is, it's a little hard for me to know what the effect was as the reviewer.

---

_Comment by @zanieb on 2024-07-23 22:33_

Sorry still trying to figure out how to present all the breakpoints as well as the key change.

The "before" image is at https://github.com/astral-sh/uv/pull/5316#issuecomment-2246265587

<img width="341" alt="Screenshot 2024-07-23 at 5 33 18 PM" src="https://github.com/user-attachments/assets/a2b7bcd3-84a6-456c-8d7b-12ac6f9fd099">

Here's the "after":

<img width="341" alt="Screenshot 2024-07-23 at 5 32 50 PM" src="https://github.com/user-attachments/assets/017e8cc9-d2d4-432f-a467-0a446892254c">


---

_Merged by @zanieb on 2024-07-23 22:33_

---

_Closed by @zanieb on 2024-07-23 22:33_

---

_Branch deleted on 2024-07-23 22:33_

---

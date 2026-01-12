```yaml
number: 5674
title: Disable collapsible navigation in the documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-sections
created_at: 2024-07-31T20:50:51Z
updated_at: 2024-08-16T23:09:23Z
url: https://github.com/astral-sh/uv/pull/5674
synced_at: 2026-01-12T16:06:57Z
```

# Disable collapsible navigation in the documentation

---

_@zanieb_

Removes the collapsible sections in favor of larger font navigation headings. I found the bold to be too distracting from the content itself — it might be okay in the future if the navigation bar is further left.

Before:

<img width="1356" alt="Screenshot 2024-08-16 at 6 03 57 PM" src="https://github.com/user-attachments/assets/75e49216-dc0d-4d26-a0d8-0283c29f9b81">

After:

<img width="1324" alt="Screenshot 2024-08-16 at 6 05 36 PM" src="https://github.com/user-attachments/assets/cbce96ce-0969-46c5-80b6-e163481b8bfa">

(No change to the mobile view)

<img width="823" alt="Screenshot 2024-08-16 at 6 05 03 PM" src="https://github.com/user-attachments/assets/b450e413-d5a4-4d2d-9905-e8eb6ac6f546">
<img width="823" alt="Screenshot 2024-08-16 at 6 05 13 PM" src="https://github.com/user-attachments/assets/bd251ea0-58d8-456e-bdc8-4e3699061e6c">



---

_Label `documentation` added by @zanieb on 2024-07-31 20:50_

---

_Label `preview` added by @zanieb on 2024-07-31 20:50_

---

_Comment by @eth3lbert on 2024-08-01 04:12_

I believe we can achieve indented headings using CSS.

---

_Comment by @zanieb on 2024-08-01 12:23_

I'm not even sure the indent is better with the bolds though 😭 

---

_Marked ready for review by @zanieb on 2024-08-16 23:07_

---

_Renamed from "Switch to mkdocs "sections" feature instead of "expanded" navigation sections" to "Disable collapsible navigation in the documentation" by @zanieb on 2024-08-16 23:09_

---

_Merged by @zanieb on 2024-08-16 23:09_

---

_Closed by @zanieb on 2024-08-16 23:09_

---

_Branch deleted on 2024-08-16 23:09_

---

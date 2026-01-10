```yaml
number: 7946
title: Clearly specify the minimum supported Windows Server version in the document
type: pull_request
state: merged
author: FishAlchemist
labels:
  - windows
assignees: []
merged: true
base: main
head: doc_minimum_windows_server_version
created_at: 2024-10-06T06:37:57Z
updated_at: 2024-10-06T14:35:22Z
url: https://github.com/astral-sh/uv/pull/7946
synced_at: 2026-01-10T12:54:00Z
```

# Clearly specify the minimum supported Windows Server version in the document

---

_Pull request opened by @FishAlchemist on 2024-10-06 06:37_

## Summary
https://doc.rust-lang.org/1.81.0/rustc/platform-support.html
![image](https://github.com/user-attachments/assets/f3921374-5f49-4f89-99d8-4808d94b4647)

This is actually part of the change in minimum Windows version requirements in Rust 1.78.0, but subsequent versions of the documentation clearly specify the minimum version of Windows Server.
https://github.com/rust-lang/rust/pull/126034

## Test Plan
Run the document server locally.
![image](https://github.com/user-attachments/assets/94f6bbd0-7aa6-4a47-aee2-d6f9ee35d5e5)



---

_Label `windows` added by @zanieb on 2024-10-06 14:33_

---

_@zanieb approved on 2024-10-06 14:33_

---

_Merged by @zanieb on 2024-10-06 14:33_

---

_Closed by @zanieb on 2024-10-06 14:33_

---

_Branch deleted on 2024-10-06 14:35_

---

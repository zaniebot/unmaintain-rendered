---
number: 8141
title: brew install exceeds expectations
type: issue
state: closed
author: wu-clan
labels: []
assignees: []
created_at: 2024-10-12T04:40:53Z
updated_at: 2024-10-12T09:52:15Z
url: https://github.com/astral-sh/uv/issues/8141
synced_at: 2026-01-07T13:12:17-06:00
---

# brew install exceeds expectations

---

_Issue opened by @wu-clan on 2024-10-12 04:40_

I ran `brew install uv` on an intel Macbook Pro and the build took hours, unbelievable!

![image](https://github.com/user-attachments/assets/a188fd91-ad45-4fdb-86b7-f4c9391fb502)

---

_Comment by @kahojyun on 2024-10-12 08:48_

It seems that you are using old macos version `monterey` and lots of brew packages drop prebuilt binary support for it, including `xz`, `cmake`, `python`, `llvm`. You can check if a package has prebuilt binary for your system [here](https://formulae.brew.sh/formula/uv#default).

---

_Comment by @wu-clan on 2024-10-12 09:52_

Thanks for clearing this up, brew has already issued a MacOS 12 deprecation warning, I think that's exactly why!

---

_Closed by @wu-clan on 2024-10-12 09:52_

---

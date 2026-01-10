```yaml
number: 3731
title: Add instructions for building and updating uv-trampolines
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
  - windows
assignees: []
merged: true
base: main
head: charlie/trampoline
created_at: 2024-05-22T02:10:08Z
updated_at: 2024-05-22T02:49:15Z
url: https://github.com/astral-sh/uv/pull/3731
synced_at: 2026-01-10T14:32:20Z
```

# Add instructions for building and updating uv-trampolines

---

_Pull request opened by @charliermarsh on 2024-05-22 02:10_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2024-05-22 02:11_

---

_Label `windows` added by @charliermarsh on 2024-05-22 02:11_

---

_Comment by @ofek on 2024-05-22 02:14_

Maybe one more thing you could add that would have helped me earlier today: to get Rust Analyzer to work in Visual Studio Code I had to add the following to `.cargo/config.toml`:

```toml
[build]
target = "x86_64-pc-windows-msvc"
```

---

_Comment by @charliermarsh on 2024-05-22 02:21_

Were you developing on Windows or is that from cross?

---

_Comment by @ofek on 2024-05-22 02:33_

I'm on Windows 11

---

_Merged by @charliermarsh on 2024-05-22 02:49_

---

_Closed by @charliermarsh on 2024-05-22 02:49_

---

_Branch deleted on 2024-05-22 02:49_

---

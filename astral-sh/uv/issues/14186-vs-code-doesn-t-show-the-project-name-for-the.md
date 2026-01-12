```yaml
number: 14186
title: "VS Code doesn't show the project name for the selected uv interpreter"
type: issue
state: open
author: FLL-Team-24277
labels:
  - question
assignees: []
created_at: 2025-06-21T12:59:50Z
updated_at: 2025-06-22T14:02:27Z
url: https://github.com/astral-sh/uv/issues/14186
synced_at: 2026-01-12T16:01:44Z
```

# VS Code doesn't show the project name for the selected uv interpreter

---

_@FLL-Team-24277_

### Question

On one computer, this displays correctly

![Image](https://github.com/user-attachments/assets/5f59ac38-02f5-42ad-a941-52114ab61ac9)

On another computer, after cloning the repo where I have also included uv.lock and pyproject.toml in the repo, I have this after running uv clone and selecting ".venv" because it didn't list the virtual environment by name

![Image](https://github.com/user-attachments/assets/99e94adb-a1c8-4b29-981c-5d8fe618f170)

As you can see, the command prompt is correct, but the selected interpreter on the bottom right isn't. These were my only options:

![Image](https://github.com/user-attachments/assets/56ddeea7-49e6-4aad-bd6d-5ae963e8fdb1)

Why would the interpreter display correctly on one computer but not others?

### Platform

Win 11

### Version

uv 0.7.13 (62ed17b23 2025-06-12)

---

_Label `question` added by @FLL-Team-24277 on 2025-06-21 12:59_

---

_Comment by @konstin on 2025-06-22 14:02_

This does not look like a problem with uv, but one with VS Code, can you open the issue in the VS Code repo instead?

---

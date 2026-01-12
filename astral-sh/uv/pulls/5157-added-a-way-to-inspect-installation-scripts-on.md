```yaml
number: 5157
title: Added a way to inspect installation scripts on Powershell(Windows)
type: pull_request
state: merged
author: FishAlchemist
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: FishAlchemist/add_inspect_windows
created_at: 2024-07-17T17:59:49Z
updated_at: 2024-07-19T14:59:14Z
url: https://github.com/astral-sh/uv/pull/5157
synced_at: 2026-01-12T16:06:40Z
```

# Added a way to inspect installation scripts on Powershell(Windows)

---

_@FishAlchemist_

## Summary
### command
 ```powershell
    powershell -c "irm https://astral.sh/uv/install.ps1 | more"
 ```
Add a method to inspect installation script files that works on Windows, it may not work on other platforms supported by PowerShell. <br>
Other platforms information:
[PowerShell differences on non-Windows platforms / General Unix interoperability changes](https://learn.microsoft.com/en-us/powershell/scripting/whats-new/unix-support?view=powershell-7.4#general-unix-interoperability-changes)
[Differences between Windows PowerShell 5.1 and PowerShell 7.x / Removal of the more function](https://learn.microsoft.com/en-us/powershell/scripting/whats-new/differences-from-windows-powershell?view=powershell-7.4#removal-of-the-more-function)
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
**terminal**
* Powershell Version: 5.1.22621.3880 (Windows)
![image](https://github.com/user-attachments/assets/418c10a4-f672-4c79-9d3b-7c58822316dd)
* Powershell Version: 7.4.3 (Windows)
![image](https://github.com/user-attachments/assets/9ee73310-a244-4320-a092-df8c6f0745c5)

## Result (Website)
![image](https://github.com/user-attachments/assets/3faede7d-4004-4e2c-b35a-80147532019b)



---

_Assigned to @zanieb by @zanieb on 2024-07-18 04:05_

---

_Review comment by @zanieb on `docs/installation.md`:29 on 2024-07-19 14:27_

```suggestion
    ```

```

---

_@zanieb reviewed on 2024-07-19 14:27_

---

_@zanieb approved on 2024-07-19 14:28_

Thanks! Updated this to match the code block style above.

---

_Merged by @zanieb on 2024-07-19 14:28_

---

_Closed by @zanieb on 2024-07-19 14:28_

---

_Label `documentation` added by @zanieb on 2024-07-19 14:28_

---

_Label `preview` added by @zanieb on 2024-07-19 14:28_

---

_Comment by @FishAlchemist on 2024-07-19 14:36_

> Thanks! Updated this to match the code block style above.

I intentionally split them into two code blocks so that only the expected commands would be copied to the clipboard.

---

_Comment by @zanieb on 2024-07-19 14:46_

Fair, but we need to be consistent — we can't change styles mid-section like that. We should consider changing both instead.

---

_Branch deleted on 2024-07-19 14:58_

---

_Branch restored on 2024-07-19 14:59_

---

_Branch deleted on 2024-07-19 14:59_

---

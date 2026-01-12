```yaml
number: 9019
title: "Add the differences between ``pip install`` and ``uv pip install`` installation directories in the pip compatibility document."
type: pull_request
state: open
author: FishAlchemist
labels:
  - documentation
  - compatibility
assignees: []
base: main
head: doc_pip_compatibility_pip_install_folder
created_at: 2024-11-11T16:07:37Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9019
synced_at: 2026-01-12T16:08:36Z
```

# Add the differences between ``pip install`` and ``uv pip install`` installation directories in the pip compatibility document.

---

_@FishAlchemist_

## Summary
https://github.com/astral-sh/uv/issues/8965#issuecomment-2466743508 mentions an expected difference about ``uv pip install folder``. 
Let's document it in the pip compatibility section.


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Run doc server in local.
(update)
![image](https://github.com/user-attachments/assets/6aee121a-3a85-4629-9ad0-c08f6a48b126)


<!-- How was it tested? -->


---

_Comment by @FishAlchemist on 2024-11-11 16:07_

However, I'm not sure if I've understood correctly.

---

_@xentec reviewed on 2024-11-11 19:57_

---

_Review comment by @xentec on `docs/pip/compatibility.md`:470 on 2024-11-11 19:57_

Missing space in `time.This`. :wink: 

---

_@FishAlchemist reviewed on 2024-11-12 01:47_

---

_Review comment by @FishAlchemist on `docs/pip/compatibility.md`:470 on 2024-11-12 01:47_


![image](https://github.com/user-attachments/assets/6ab30d3b-b365-4243-8ea9-032684bf13bc)
Thank you, I think I got your point.

---

_@samypr100 reviewed on 2024-11-21 01:43_

---

_Review comment by @samypr100 on `docs/pip/compatibility.md`:468 on 2024-11-21 01:43_

```suggestion
uv does not reinstall already installed packages by default.
```

Small spelling adjust per `STYLE.md`

---

_@samypr100 reviewed on 2024-11-21 01:43_

---

_Review comment by @samypr100 on `docs/pip/compatibility.md`:471 on 2024-11-21 01:43_

```suggestion
expected behavior, as uv anticipates that you might want to update to a newer version.
```

Small spelling adjust per `STYLE.md`

---

_Label `documentation` added by @samypr100 on 2024-11-21 02:31_

---

_Label `compatibility` added by @samypr100 on 2024-11-21 02:31_

---

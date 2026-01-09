---
number: 10056
title: "\"uv add PySide6\" fails when the latest version doesn't have a wheel for the current platform"
type: issue
state: closed
author: borco
labels:
  - bug
  - duplicate
assignees: []
created_at: 2024-12-20T14:53:44Z
updated_at: 2025-01-08T13:32:54Z
url: https://github.com/astral-sh/uv/issues/10056
synced_at: 2026-01-07T13:12:18-06:00
---

# "uv add PySide6" fails when the latest version doesn't have a wheel for the current platform

---

_Issue opened by @borco on 2024-12-20 14:53_

At the time of writing this, the latest version of `PySide6` is `6.8.1.1`. I assume this is a patch release only for macos, as there is only one wheel for macos in it.

When I try to install `PySide6` on Windows, I get an error that no source or wheel is available:

```bash
$ uv add pyside6
Resolved 5 packages in 6ms
error: Distribution `pyside6==6.8.1.1 @ registry+https://pypi.org/simple` can't be installed 
because it doesn't have a source distribution or wheel for the current platform
```

However, the older release `6.8.1` contains wheels for many other platforms. If I blacklist the `6.8.1.1` version, then installing on Windows works:

```bash
$ uv add "pyside6>=6.8.1,!=6.8.1.1"
Resolved 5 packages in 257ms
Installed 4 packages in 1.05s
 + pyside6==6.8.1
 + pyside6-addons==6.8.1
 + pyside6-essentials==6.8.1
 + shiboken6==6.8.1
```

Is it possible to automatically detect such a case and install from the older version without having to explicitly blacklist the latest version that only contains wheels for other platform(s)?

[PySide 6.8.1.1 files](https://pypi.org/project/PySide6/6.8.1.1/#files)

[PySide 6.8.1 files](https://pypi.org/project/PySide6/6.8.1/#files)

![Image](https://github.com/user-attachments/assets/56ee276a-60d1-4cb2-ab9e-aa7ec35b19f0)

![Image](https://github.com/user-attachments/assets/a9f461af-7d63-413a-9902-c7fcafd7d165)


---

_Comment by @charliermarsh on 2024-12-20 16:51_

This is tracked in https://github.com/astral-sh/uv/issues/9711 (though this is really a package issue -- they should be uploading a consistent set of wheels, the system isn't designed to be used this way).

---

_Referenced in [astral-sh/uv#9711](../../astral-sh/uv/issues/9711.md) on 2024-12-20 16:51_

---

_Label `bug` added by @charliermarsh on 2024-12-20 16:51_

---

_Label `duplicate` added by @charliermarsh on 2024-12-20 16:51_

---

_Referenced in [OpenAdaptAI/OpenAdapt#931](../../OpenAdaptAI/OpenAdapt/pulls/931.md) on 2024-12-31 00:29_

---

_Referenced in [cbrnr/mnelab#470](../../cbrnr/mnelab/pulls/470.md) on 2025-01-08 08:09_

---

_Comment by @cbrnr on 2025-01-08 08:14_

I agree that this is really a packaging issue, and the list of packages that misuse the system is kind of shocking! Is there any workaround for this particular case that we could use in the meantime?

Related issue: https://bugreports.qt.io/browse/PYSIDE-2971

---

_Comment by @cbrnr on 2025-01-08 09:44_

They've uploaded the missing packages so this issue can be closed.

---

_Comment by @charliermarsh on 2025-01-08 13:32_

Oh great, thank you!

---

_Closed by @charliermarsh on 2025-01-08 13:32_

---

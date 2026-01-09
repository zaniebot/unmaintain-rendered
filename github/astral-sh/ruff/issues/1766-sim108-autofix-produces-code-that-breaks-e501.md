---
number: 1766
title: SIM108 autofix produces code that breaks E501
type: issue
state: closed
author: Czaki
labels:
  - fixes
assignees: []
created_at: 2023-01-10T10:20:11Z
updated_at: 2023-01-12T00:21:32Z
url: https://github.com/astral-sh/ruff/issues/1766
synced_at: 2026-01-07T13:12:14-06:00
---

# SIM108 autofix produces code that breaks E501

---

_Issue opened by @Czaki on 2023-01-10 10:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

When bumpy ruff from 207 to 217 (If I correctly check, the problem was introduced in 213) I got multiple autofixes from SIM108 that led to multiple errors reports from E501 as it creates very long lines (always creates single line if)

https://github.com/4DNucleome/PartSeg/pull/879

Sample diff 
```
--- a/package/PartSeg/_launcher/check_version.py
+++ b/package/PartSeg/_launcher/check_version.py
@@ -67,24 +67,7 @@ class CheckVersionThread(QThread):
         my_version = packaging.version.parse(self.base_release)
         remote_version = packaging.version.parse(self.release)
         if remote_version > my_version:
-            if getattr(sys, "frozen", False):
-                message = QMessageBox(
-                    QMessageBox.Information,
-                    "New release",
-                    f"You use outdated version of PartSeg. "
-                    f"Your version is {my_version} and current is {remote_version}. "
-                    f"You can download next release form {self.url}",
-                    QMessageBox.Ok | QMessageBox.Ignore,
-                )
-            else:
-                message = QMessageBox(
-                    QMessageBox.Information,
-                    "New release",
-                    f"You use outdated version of PartSeg. "
-                    f"Your version is {my_version} and current is {remote_version}. "
-                    "You can update it from pypi (pip install -U PartSeg)",
-                    QMessageBox.Ok | QMessageBox.Ignore,
-                )
+            message = QMessageBox(QMessageBox.Information, "New release", f"You use outdated version of PartSeg. Your version is {my_version} and current is {remote_version}. You can download next release form {self.url}", QMessageBox.Ok | QMessageBox.Ignore) if getattr(sys, "frozen", False) else QMessageBox(QMessageBox.Information, "New release", f"You use outdated version of PartSeg. Your version is {my_version} and current is {remote_version}. You can update it from pypi (pip install -U PartSeg)", QMessageBox.Ok | QMessageBox.Ignore)
```

For me, at least SIM108 should create E501 compatible code or even not fix it if the line is too long (as it will reduce readability). 

In this example, even black cannot help as it concatenates strings.


---

_Label `autofix` added by @charliermarsh on 2023-01-10 12:22_

---

_Referenced in [astral-sh/ruff#1719](../../astral-sh/ruff/issues/1719.md) on 2023-01-11 23:00_

---

_Referenced in [astral-sh/ruff#1802](../../astral-sh/ruff/pulls/1802.md) on 2023-01-12 00:21_

---

_Closed by @charliermarsh on 2023-01-12 00:21_

---

_Referenced in [astral-sh/ruff#2621](../../astral-sh/ruff/issues/2621.md) on 2023-02-07 15:48_

---

_Referenced in [astral-sh/ruff#8106](../../astral-sh/ruff/issues/8106.md) on 2023-10-22 23:53_

---

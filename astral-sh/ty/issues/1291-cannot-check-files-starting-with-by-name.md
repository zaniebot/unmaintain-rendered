```yaml
number: 1291
title: "Cannot check files starting with @ by name"
type: issue
state: closed
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-10-01T00:02:10Z
updated_at: 2025-10-01T00:34:08Z
url: https://github.com/astral-sh/ty/issues/1291
synced_at: 2026-01-12T15:54:24Z
```

# Cannot check files starting with @ by name

---

_@MeGaGiGaGon_

### Summary

If a file's name starts with `@` and you try to specify it on the command line, it will fail. Checking works fine if the file is gotten automatically, ie by checking `.`

If the file name has no special characters after the `@`, ty will instead check all files in the folder as if no argument had been passed. (Notice how in both demos it says 4/4 files checked instead of 1/1)
If the file name has a special character after the `@`, checking will fail completely.

<details>
<summary>
Powershell demo
</summary>

```powershell
PS ~\...\issue>ls

    Directory: C:\...\issue

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           9/30/2025  4:44 PM              2 @!.py
-a---           9/30/2025  4:42 PM              2 @.py
-a---           9/30/2025  4:41 PM              2 @issue.py
-a---           9/30/2025  4:40 PM              2 issue.py

PS ~\...\issue>uvx ty check "issue.py"
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                         All checks passed!
PS ~\...\issue>uvx ty check "@issue.py"
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 4/4 files                                         All checks passed!
PS ~\...\issue>uvx ty check "@.py"
ty failed
  Cause: Failed to read CLI arguments from file
  Cause: failed to open file `.py`
  Cause: The system cannot find the file specified. (os error 2)
```

</details>

<details>
<summary>
WSL demo
</summary>

```bash
user@DESKTOP-44VGRD0:~/issue$ ls
@.py  @issue.py  issue.py  issue@.py
user@DESKTOP-44VGRD0:~/issue$ uvx ty check issue.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                         All checks passed!
user@DESKTOP-44VGRD0:~/issue$ uvx ty check @issue.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 4/4 files                                         All checks passed!
user@DESKTOP-44VGRD0:~/issue$ uvx ty check @.py
ty failed
  Cause: Failed to read CLI arguments from file
  Cause: failed to open file `.py`
  Cause: No such file or directory (os error 2)
```

</details>

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @MeGaGiGaGon on 2025-10-01 00:34_

This is intentional, since they are treated like fromfiles, like https://docs.python.org/3/library/argparse.html#fromfile-prefix-chars .
https://github.com/astral-sh/ruff/issues/20655#issuecomment-3354189610
Using `/@<file name>` works

---

_Closed by @MeGaGiGaGon on 2025-10-01 00:34_

---

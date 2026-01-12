```yaml
number: 1045
title: Oddities with ANSI escape codes and terminal output in the PyCharm console on Windows
type: issue
state: closed
author: AA-Turner
labels:
  - cli
assignees: []
created_at: 2025-08-18T20:19:13Z
updated_at: 2025-08-19T09:13:05Z
url: https://github.com/astral-sh/ty/issues/1045
synced_at: 2026-01-12T15:54:24Z
```

# Oddities with ANSI escape codes and terminal output in the PyCharm console on Windows

---

_@AA-Turner_

### Summary

When running `ty check` in the PyCharm console on Windows, the first line ("WARN ty is pre-release ...") is not properly formatted, but contains ANSI escape codes:

```ps1con
PS> ty check
?[1;33mWARN?[0m ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 740/740 files
```

<img width="215" height="79" alt="Image" src="https://github.com/user-attachments/assets/fb8f4f8a-b7c3-4329-bb3e-7f81874e6ac8" />

All other lines are properly formatted with colour and boldface text, it just seems to the the first line that's wrong.

Another problem is piping to a file -- the progress indicator is printed several times (with no formatting, but this seems expected):

```ps1con
PS> ty check >tyout.txt
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/740 files
Checking ------------------------------------------------------------ 2/740 files
Checking ------------------------------------------------------------ 3/740 files
Checking ------------------------------------------------------------ 4/740 files
Checking ------------------------------------------------------------ 5/740 files
Checking ------------------------------------------------------------ 6/740 files
Checking ------------------------------------------------------------ 7/740 files
Checking ------------------------------------------------------------ 8/740 files
Checking ------------------------------------------------------------ 9/740 files
Checking ------------------------------------------------------------ 10/740 files
Checking ------------------------------------------------------------ 11/740 files
Checking ------------------------------------------------------------ 12/740 files
Checking ------------------------------------------------------------ 13/740 files
Checking ------------------------------------------------------------ 14/740 files
Checking ------------------------------------------------------------ 15/740 files
Checking ------------------------------------------------------------ 16/740 files
Checking ------------------------------------------------------------ 18/740 files
Checking ------------------------------------------------------------ 18/740 files
Checking ------------------------------------------------------------ 19/740 files
Checking ------------------------------------------------------------ 20/740 files
Checking ------------------------------------------------------------ 21/740 files
Checking ------------------------------------------------------------ 74/740 files
Checking ------------------------------------------------------------ 171/740 files
Checking ------------------------------------------------------------ 304/740 files
Checking ------------------------------------------------------------ 419/740 files
Checking ------------------------------------------------------------ 610/740 files
Checking ------------------------------------------------------------ 740/740 files
```

This is specific to the embeded PyCharm terminal, the Windows Terminal programme has neither of these errors - the WARN line is printed in orange and only one progress indicator line is printed.

Both terminal emulators (PyCharm embedded & Windows Terminal) are running PowerShell Core 7.5.2, with the same active virtual environment running ty alpha 18.

Sorry that this is a rather odd bug report, but the behaviour is spooky! Feel free to close if the problem is with PyCharm, but I thought worth reporting as it's something other users may run in to.

Thanks,
Adam

### Version

ty 0.0.1-alpha.18 (d697cc092 2025-08-14)

---

_Label `cli` added by @carljm on 2025-08-19 00:00_

---

_Closed by @sharkdp on 2025-08-19 09:13_

---

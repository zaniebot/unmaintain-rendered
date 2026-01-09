---
number: 16126
title: Command requires double quote escapes to work on Windows
type: issue
state: closed
author: raffaem
labels:
  - documentation
  - question
assignees: []
created_at: 2025-10-06T00:36:30Z
updated_at: 2025-11-03T02:45:34Z
url: https://github.com/astral-sh/uv/issues/16126
synced_at: 2026-01-07T13:12:19-06:00
---

# Command requires double quote escapes to work on Windows

---

_Issue opened by @raffaem on 2025-10-06 00:36_

### Summary

[Here](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies) the command requires double quote escapes to work on Windows:

```
uv add --script example.py "requests<3" "rich"
```

Running it like it is right now:

```
uv add --script example.py 'requests<3' 'rich'
```

returns

```
The system cannot find the file specified.
```

### Platform

Windows

### Version

0.8.23

### Python version

_No response_

---

_Label `bug` added by @raffaem on 2025-10-06 00:36_

---

_Comment by @FishAlchemist on 2025-10-06 01:50_

To add context for reproducing the issue: This error occurs in ``Windows cmd``. In PowerShell, both single and double quotes can be used for strings; while there are differences, it does not affect the outcome in this specific scenario.

Ref: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_quoting_rules

> If the directory path, files, or any information you supply contains spaces, you must use double quotation marks (" ") around the text, such as "Computer Name".

Ref: https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/cmd#remarks

---

_Label `bug` removed by @konstin on 2025-10-06 09:18_

---

_Label `question` added by @konstin on 2025-10-06 09:18_

---

_Label `documentation` added by @konstin on 2025-10-06 09:18_

---

_Comment by @konstin on 2025-10-06 09:19_

Quoting and escaping are unfortunately a problem where we can't provide a universal example: On Unix (posix) shells, strings between double quotes my be interpolated, so we need single quotes to disable that. On Windows Command Prompt, only double quotes are supported. For the specific example double quotes should work on Unix too, though generally most users will either be on a Unix shell or in a Windows PowerShell.

---

_Closed by @charliermarsh on 2025-11-03 02:45_

---

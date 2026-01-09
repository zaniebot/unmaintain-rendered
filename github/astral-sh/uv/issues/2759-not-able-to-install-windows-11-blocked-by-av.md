---
number: 2759
title: Not able to install windows 11 blocked by AV
type: issue
state: closed
author: sanjithacks
labels: []
assignees: []
created_at: 2024-04-01T17:33:22Z
updated_at: 2024-04-01T17:37:25Z
url: https://github.com/astral-sh/uv/issues/2759
synced_at: 2026-01-07T13:12:17-06:00
---

# Not able to install windows 11 blocked by AV

---

_Issue opened by @sanjithacks on 2024-04-01 17:33_

Installation blocked by BitDefender

```sh

Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Users\xyz> powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
At line:1 char:1
+ powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This script contains malicious content and has been blocked by your antivirus software.
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : ScriptContainedMaliciousContent

```



![image](https://github.com/astral-sh/uv/assets/35589762/f9cac962-18f8-43fb-b191-3fb806166d30)



---

_Comment by @sanjithacks on 2024-04-01 17:35_

Finally installed with 🥲

```sh
pip install uv
```

---

_Closed by @sanjithacks on 2024-04-01 17:35_

---

_Comment by @charliermarsh on 2024-04-01 17:37_

🥲

---

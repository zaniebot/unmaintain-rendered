```yaml
number: 9503
title: The command to changing the install path in the document does not work on Windows
type: issue
state: closed
author: A-kirami
labels: []
assignees: []
created_at: 2024-11-28T15:12:18Z
updated_at: 2024-11-28T17:51:31Z
url: https://github.com/astral-sh/uv/issues/9503
synced_at: 2026-01-12T15:59:52Z
```

# The command to changing the install path in the document does not work on Windows

---

_@A-kirami_

I attempted to follow the instructions in the [document](https://docs.astral.sh/uv/configuration/installer/#changing-the-install-path) to changing the install path using the following command, but encountered an error on a Windows system.

```
$env:UV_INSTALL_DIR = "C:\Custom\Path" powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

in PowerShell 7:

```
ParserError:
Line |
   1 |  $env:UV_INSTALL_DIR = "C:\Custom\Path" powershell -ExecutionPolicy By …
     |                                         ~~~~~~~~~~
     | Unexpected token 'powershell' in expression or statement.
```

in Windows PowerShell:

```
所在位置 行:1 字符: 40
+ $env:UV_INSTALL_DIR = "C:\Custom\Path" powershell -ExecutionPolicy By ...
+                                        ~~~~~~~~~~
表达式或语句中包含意外的标记“powershell”。
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : UnexpectedToken
```

---

_Comment by @FishAlchemist on 2024-11-28 17:38_

Although I'm not sure if this method(https://github.com/astral-sh/uv/pull/9507) will be accepted, at least it can solve your current problem.
```powershell
 $env:UV_INSTALL_DIR = "C:\Custom\Path"; powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

---

_Comment by @A-kirami on 2024-11-28 17:51_

> Although I'm not sure if this method([#9507](https://github.com/astral-sh/uv/pull/9507)) will be accepted, at least it can solve your current problem.
> 
>  $env:UV_INSTALL_DIR = "C:\Custom\Path"; powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

Thank you, that resolved my issue.

---

_Closed by @A-kirami on 2024-11-28 17:51_

---

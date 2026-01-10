```yaml
number: 12394
title: Error while trying to find the Ruff binary - VS Code Ruff Extension
type: issue
state: closed
author: DG119
labels:
  - bug
  - server
assignees: []
created_at: 2024-07-18T20:48:25Z
updated_at: 2024-12-30T18:16:05Z
url: https://github.com/astral-sh/ruff/issues/12394
synced_at: 2026-01-10T11:09:54Z
```

# Error while trying to find the Ruff binary - VS Code Ruff Extension

---

_Issue opened by @DG119 on 2024-07-18 20:48_

* The current Ruff settings: all default with Lint disabled
* The current Ruff version: 2024.32.0
In the newly released version 2024.32.0 for Ruff VS Code Extesion, the following error is logged on VS Code on start up:
```
2024-07-18 21:32:04.253 [info] Name: Ruff
2024-07-18 21:32:04.254 [info] Module: ruff
2024-07-18 21:32:04.254 [info] Python extension loading
2024-07-18 21:32:04.254 [info] Waiting for interpreter from python extension.
2024-07-18 21:32:04.421 [info] Python extension loaded
2024-07-18 21:33:41.151 [error] Error while trying to find the Ruff binary: Error: Command failed: c:\Users\D Censored\Desktop\Python\Xreply\Scripts\python.exe c:\Users\D Censored\.vscode\extensions\charliermarsh.ruff-2024.32.0-win32-x64\bundled\tool\find_ruff_binary_path.py
'c:\Users\D' �����ڲ����ⲿ���Ҳ���ǿ����еĳ���
���������ļ���

2024-07-18 21:33:41.211 [info] Falling back to bundled executable: c:\Users\D Censored\.vscode\extensions\charliermarsh.ruff-2024.32.0-win32-x64\bundled\libs\bin\ruff.exe
```

There is a space in my user name, which I suspect is the reason for this error. This did not happen in the previous version. I think some thing broke it in version 2024.32.0
![2024-07-18 214632](https://github.com/user-attachments/assets/5996f729-f1d6-4163-8304-71abf1f47b98)


---

_Comment by @charliermarsh on 2024-07-18 22:46_

I'll look into it...

---

_Comment by @charliermarsh on 2024-07-18 22:48_

Okay yeah I see the problem. Will fix!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-18 22:48_

---

_Label `bug` added by @charliermarsh on 2024-07-18 22:48_

---

_Label `server` added by @charliermarsh on 2024-07-18 22:48_

---

_Comment by @charliermarsh on 2024-07-18 22:53_

I'll ship a fix for this tonight, apologies.

---

_Closed by @charliermarsh on 2024-07-18 23:26_

---

_Closed by @charliermarsh on 2024-07-18 23:26_

---

_Comment by @charliermarsh on 2024-07-19 02:16_

Ok, I actually did fix this (https://github.com/astral-sh/ruff-vscode/pull/539), but Azure is down so we can't ship new versions of the extension. Sorry! Will go out tomorrow.

---

_Comment by @MichaReiser on 2024-07-19 06:34_

I just published the release. @DG119 could you update your extension and give it another try?

---

_Comment by @DG119 on 2024-07-19 10:35_

> I just published the release. @DG119 could you update your extension and give it another try?

I can confirm it's working now after installing the latest pre-release. Thank you all for the fast action.

---

_Comment by @alejosas398 on 2024-12-30 18:16_

error al intentar encontrar el binario de ruff visual studio code .como lo soluciono?


---

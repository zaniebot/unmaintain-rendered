```yaml
number: 5918
title: F811 not raised for redefinitions of functions inside class
type: issue
state: closed
author: ragardner
labels:
  - bug
assignees: []
created_at: 2023-07-20T07:48:07Z
updated_at: 2023-07-20T16:10:27Z
url: https://github.com/astral-sh/ruff/issues/5918
synced_at: 2026-01-12T15:54:45Z
```

# F811 not raised for redefinitions of functions inside class

---

_@ragardner_

I am not 100% sure but isn't ruff meant to highlight redefinitions of functions outside as well as inside classes?

Inside class - not underlined - not working?
![image](https://github.com/astral-sh/ruff-vscode/assets/26602401/40806fd8-101c-420c-982a-7ea6c2d85838)

```python
class X:
    def testfunc(self):
        x = None
        
    def testfunc(self):
        x = None
```

Outside class - underlined - working
![image](https://github.com/astral-sh/ruff-vscode/assets/26602401/4193c2f7-a8fb-46e7-968d-1108c9723002)
```python
def testfunc():
    x = None
    
def testfunc():
    x = None
```

Only setting I'm running on ruff is line length

ruff: `v2023.32.0` ? seems latest on extensions

OS: `Linux Pop OS` latest

VSCode about:
```
Version: 1.80.1
Commit: 74f6148eb9ea00507ec113ec51c489d6ffb4b771
Date: 2023-07-12T17:22:25.257Z
Electron: 22.3.14
ElectronBuildId: 21893604
Chromium: 108.0.5359.215
Node.js: 16.17.1
V8: 10.8.168.25-electron.0
OS: Linux x64 6.2.6-76060206-generic
```

Python Version: `3.10.6`


---

_Comment by @zanieb on 2023-07-20 13:23_

Thanks for the report!

I can confirm that `F811` is raised for the function definition but not for the class method definition however flake8 would raise here. It looks like a bug in our implementation — I don't think this is a VSCode related issue though so I'm going to transfer it over to the ruff repository.

---

_Renamed from "Ruff doesn't seem to be detecting redefinitions of functions inside class?" to "F811 not raised for redefinitions of functions inside class" by @zanieb on 2023-07-20 13:24_

---

_Label `bug` added by @zanieb on 2023-07-20 13:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-20 13:36_

---

_Comment by @charliermarsh on 2023-07-20 14:00_

This is my bad :)

---

_Closed by @charliermarsh on 2023-07-20 16:10_

---

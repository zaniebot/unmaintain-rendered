---
number: 6329
title: "[Linter panic] --select=D203"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-08-04T07:07:17Z
updated_at: 2023-08-04T16:08:38Z
url: https://github.com/astral-sh/ruff/issues/6329
synced_at: 2026-01-07T13:12:15-06:00
---

# [Linter panic] --select=D203

---

_Issue opened by @FishAlchemist on 2023-08-04 07:07_

### Ruff Version
 commit message: Make the Nodes vector generic on node type (#6328)
https://github.com/astral-sh/ruff/tree/8a5bc93fdd621aec745b6b86f7a4b880a5342625
### Command
```powershell
cargo run -p ruff_cli -- check --no-cache --select=D203 --isolated "D:\ruff-temp" *> D:\ruff-temp\output.txt
```
### Content of output.txt 
[output.txt](https://github.com/astral-sh/ruff/files/12257757/output.txt)
### Linting panicked file
[panicked_file.zip](https://github.com/astral-sh/ruff/files/12257923/panicked_file.zip)
virus scan:
https://www.virustotal.com/gui/file/47ce6dbf86e248dfeb40e4c8b10edf7031a8e5c8741a8d20354e3d2cf4f8eeb3
If you are worried about something wrong with the zip file, you can find me on Discord to get the python file in the zip.
### Ruff settings
nothing. 
### Remark
I've executed these commands before checking, I'm not sure if it has any effect.
```powershell
cargo clean
cargo build -p ruff_cli
```


---

_Renamed from "[Linter panic] D203" to "[Linter panic] --select=D203" by @FishAlchemist on 2023-08-04 07:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-04 13:52_

---

_Comment by @charliermarsh on 2023-08-04 14:43_

Thanks! Here's an MRE:
```python
def test_class(self):
    class C: "New-style class"
```

---

_Referenced in [astral-sh/ruff#6344](../../astral-sh/ruff/pulls/6344.md) on 2023-08-04 15:27_

---

_Label `bug` added by @charliermarsh on 2023-08-04 15:27_

---

_Closed by @charliermarsh on 2023-08-04 16:08_

---

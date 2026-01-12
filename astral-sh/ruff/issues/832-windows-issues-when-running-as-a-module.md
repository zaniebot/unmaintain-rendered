```yaml
number: 832
title: "[Windows] Issues when running as a module"
type: issue
state: closed
author: andriilahuta
labels:
  - bug
assignees: []
created_at: 2022-11-20T16:08:27Z
updated_at: 2023-06-15T18:58:02Z
url: https://github.com/astral-sh/ruff/issues/832
synced_at: 2026-01-12T15:54:40Z
```

# [Windows] Issues when running as a module

---

_@andriilahuta_

If the interpreter path contains spaces (e.g. `C:\Program Files\Python311\python.exe`), then ruff produces the following output: `Files\Python311\Scripts\ruff:0:0: E902 The system cannot find the path specified` and exits with code 1.

Replacing
```py
sys.exit(os.spawnv(os.P_WAIT, ruff, [ruff, *sys.argv[1:]]))
```
with
```py
sys.exit(subprocess.call([ruff, *sys.argv[1:]]))
```
in `__main__.py` seems to resolve the issue.

---

_Label `bug` added by @charliermarsh on 2022-11-20 17:38_

---

_Comment by @sbrugman on 2023-01-30 12:20_

@andriilahuta Could you confirm this is still an issue? We've added windows testing in the meantime, so if it's still an issue we should extend our testing suite (and fix it).

---

_Comment by @andriilahuta on 2023-01-30 16:13_

@sbrugman Still an issue.  
To reproduce, you can run ruff like this: `"C:\Program Files\Python311\python.exe" -m ruff .`.  
Replacing `os.spawnv` with `subprocess.call` fixes the issue for me.

---

_Comment by @sbrugman on 2023-01-31 22:55_

Suppose we could add this to the maturin test stage in the CI pipeline. Needs some testing to ensure there are no regressions.

---

_Comment by @srhinos on 2023-03-06 09:17_

Coming in to confirm this is happening for me too. Synced the vscode setting from my macbook (where ruff runs fine), to my windows PC and when ruff is set to handle `source.fixAll`, its actually wiping the python file on save.

Replacing the file with 
```
[
    {
        "code": "E902",
        "message": "The system cannot find the file specified. (os error 2)",
        "fix": null,
        "location": {"row": 1, "column": 1},
        "end_location": {"row": 1, "column": 1},
        "filename": "c:\\Users\\rhino\\Downloads\\folder with spaces in it\\-",
    },
    {
        "code": "E902",
        "message": "The system cannot find the file specified. (os error 2)",
        "fix": null,
        "location": {"row": 1, "column": 1},
        "end_location": {"row": 1, "column": 1},
        "filename": "c:\\Users\\rhino\\Downloads\\folder with spaces in it\\line-length = 100",
    },
]
```

Thankfully, black comes in after and auto corrects the JSON to a Python friendly dict or I'd have been REALLY sad :,)

---

_Comment by @charliermarsh on 2023-03-06 13:17_

Woah, the Python file contents are _replaced_ by that JSON?

---

_Comment by @srhinos on 2023-03-06 13:56_

yeah, super weird interaction!

Running python 3.11 but it might be an issue too with the fact that my Windows PC's installation is syncing settings from my Macbook's with no regard for what that might mean. I can dig into what settings are needed for repro if y'all can't get a repro of the issue down

---

_Comment by @konstin on 2023-06-15 18:58_

We've now implemented this change in #5115, this should also fix your problem. If it doesn't, please feel free to reopen.

---

_Closed by @konstin on 2023-06-15 18:58_

---

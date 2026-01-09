---
number: 3106
title: Suggested fix columns in JSON output may be off by 1
type: issue
state: closed
author: tasgon
labels:
  - breaking
assignees: []
created_at: 2023-02-21T23:01:46Z
updated_at: 2023-05-10T21:56:13Z
url: https://github.com/astral-sh/ruff/issues/3106
synced_at: 2026-01-07T13:12:14-06:00
---

# Suggested fix columns in JSON output may be off by 1

---

_Issue opened by @tasgon on 2023-02-21 23:01_

Consider the following file:
```py
import sys, json # comment
sys.exit(42)
```

Running `ruff check --output json --select F401` on that yields:
```
[
  {
    "code": "F401",
    "message": "`json` imported but unused",
    "fix": {
      "content": "import sys",
      "message": "Remove unused import: `json`",
      "location": {
        "row": 1,
        "column": 0
      },
      "end_location": {
        "row": 1,
        "column": 16
      }
    },
    "location": {
      "row": 1,
      "column": 13
    },
    "end_location": {
      "row": 1,
      "column": 17
    },
    "filename": "test.py"
  }
]
```

Is this intended, or should the column fields in the `fix` object be 1 and 17, respectively?

---

_Comment by @charliermarsh on 2023-02-21 23:18_

Yeah this is bad but "known" (and existing clients, like the LSP, rely on it). It happened unintentionally. RustPython returns locations with a one-indexed row and a zero-indexed column. When we create those top-level locations, we modify the column to be one-indexed, which is more natural when reporting. However, we forgot to do the same for the fix locations (which don't appear in the standard output, so it was an oversight).


---

_Comment by @charliermarsh on 2023-02-21 23:19_

You're welcome to rely on these semantics as other clients already do so. Perhaps I'll fix this as a breaking change when we go to 1.0.


---

_Label `breaking` added by @charliermarsh on 2023-02-21 23:19_

---

_Comment by @youknowone on 2023-04-20 15:11_

Is this a bug of Location or only related to spec of json output?

---

_Comment by @charliermarsh on 2023-04-21 02:03_

@youknowone - It's a bug on our end -- we're just not consistent about using zero- or one-based indexing for these, but @MichaReiser has a fix for it on our side.

---

_Comment by @charliermarsh on 2023-05-10 21:56_

I believe this was fixed by #4007.

---

_Closed by @charliermarsh on 2023-05-10 21:56_

---

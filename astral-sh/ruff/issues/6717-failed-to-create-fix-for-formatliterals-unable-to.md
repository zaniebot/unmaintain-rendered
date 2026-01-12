```yaml
number: 6717
title: "Failed to create fix for FormatLiterals: Unable to identify format literals"
type: issue
state: open
author: qarmin
labels:
  - needs-decision
  - fuzzer
assignees: []
created_at: 2023-08-21T08:58:02Z
updated_at: 2023-10-01T08:20:33Z
url: https://github.com/astral-sh/ruff/issues/6717
synced_at: 2026-01-12T15:54:46Z
```

# Failed to create fix for FormatLiterals: Unable to identify format literals

---

_@qarmin_

Ruff 0.0.285

```
ruff  *.py --select ALL --no-cache
```

file content:
```

'{' '0}'.format(1)
```

error:
```
error: Failed to create fix for FormatLiterals: Unable to identify format literals

```


[PY_FILE_TEST_887401168.py.zip](https://github.com/astral-sh/ruff/files/12394238/PY_FILE_TEST_887401168.py.zip)


---

_Label `fuzzer` added by @charliermarsh on 2023-08-21 12:56_

---

_Comment by @charliermarsh on 2023-08-21 12:57_

I'm not sure what to do here -- this is expected, and it doesn't actually error for the user (in the sense that the program does not return a non-zero status code and the program still behaves reasonably), it just surfaces that we weren't able to analyze the format string. To me this is reasonable behavior.

---

_Label `needs-decision` added by @charliermarsh on 2023-08-21 12:58_

---

_Comment by @qarmin on 2023-08-21 15:01_

For me this looks like internal ruff message, that is not really interesting from user perspective - there is no file or place where problem happened and how to fix it.

So I think that this should be at least hidden for end-user(could be available in verbose mode) or should contains info how user can fix this or to report problem issue to this repo

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-21 20:54_

---

_Renamed from "Failed to create fix for FormatLiterals: Unable to identify format literals" to "Failed to create fix for FormatLiterals: Unable to identify format literals - Rule UP030" by @qarmin on 2023-10-01 08:20_

---

_Renamed from "Failed to create fix for FormatLiterals: Unable to identify format literals - Rule UP030" to "Failed to create fix for FormatLiterals: Unable to identify format literals" by @qarmin on 2023-10-01 08:20_

---

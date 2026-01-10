```yaml
number: 3339
title: "Warn when `# noqa: <code>` contains an invalid code"
type: issue
state: closed
author: martinlehoux
labels:
  - suppression
assignees: []
created_at: 2023-03-04T16:36:30Z
updated_at: 2023-07-08T16:37:57Z
url: https://github.com/astral-sh/ruff/issues/3339
synced_at: 2026-01-10T11:09:46Z
```

# Warn when `# noqa: <code>` contains an invalid code

---

_Issue opened by @martinlehoux on 2023-03-04 16:36_

With the following snippet, I can ignore the issue in 3 ways, but the 4th fails to ignore, and it's not consistant:
- Add `# noqa: N815` on the line :heavy_check_mark: 
- Add `# noqa: mixed-case-variable-in-class-scope` on the line :heavy_check_mark: 
- Add `# ruff: noqa: N815` on the first line of the file :heavy_check_mark:
- Add `# ruff: noqa: mixed-case-variable-in-class-scope` on the first line of the file :stop_sign: 

```py
from typing import TypedDict


class Request(TypedDict):
    queryString: str -> Variable `queryString` in class scope should not be mixedCaseRuff(N815)
```

---

_Comment by @charliermarsh on 2023-03-04 16:38_

The second way is actually kind of a bug but it's consistent with Flake8 IIUC -- `# noqa: mixed-case-variable-in-class-scope` ignores _all_ errors on that line.

---

_Comment by @martinlehoux on 2023-03-04 16:42_

:open_mouth: Damn that could lead me to unexpected ignores, I thought it was normal behavior. I have `# noqa: hardcoded-password-string` or `# noqa: ambiguous-unicode-character-string` all over the place

---

_Comment by @charliermarsh on 2023-03-04 16:43_

I thought we warned about that, I'll have to double-check. (I see stuff like `# noqa: F401` often, which Flake8 treats as "ignore all rules".)

We don't currently support using the human-readable names for ignores, we need to finalize the names first.

---

_Comment by @charliermarsh on 2023-03-04 17:17_

Wait sorry sorry.

---

_Comment by @charliermarsh on 2023-03-04 17:17_

I misspoke. Let me clarify.

---

_Comment by @charliermarsh on 2023-03-04 17:18_

`# noqa: F401` only ignores `F401` on a given line.

`# flake8: noqa: F401` on its own line ignores _all_ errors in a file (which is a common mistake).

---

_Comment by @charliermarsh on 2023-03-04 17:19_

`# noqa: mixed-case-variable-in-class-scope` ignores all errors on a given line, erroneously (in both Ruff and Flake8).

---

_Comment by @charliermarsh on 2023-03-04 17:20_

If you do `# ruff: noqa: mixed-case-variable-in-class-scope`, we do properly warn that `mixed-case-variable-in-class-scope` is an unknown code.

---

_Comment by @charliermarsh on 2023-03-04 17:20_

We should probably warn that `# noqa: mixed-case-variable-in-class-scope` is an unknown code.

---

_Renamed from "Inconsistent ruff: noqa: <full-name> behavior on file first line" to "Warn when `# noqa: <code>` contains an invalid code" by @charliermarsh on 2023-03-04 20:54_

---

_Label `noqa` added by @charliermarsh on 2023-03-04 22:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-05 01:32_

---

_Closed by @charliermarsh on 2023-07-08 16:37_

---

---
number: 13155
title: "Support passing file content from stdin when `--add-noqa` is used"
type: issue
state: open
author: InSyncWithFoo
labels:
  - cli
  - suppression
assignees: []
created_at: 2024-08-30T01:21:42Z
updated_at: 2024-08-30T08:10:11Z
url: https://github.com/astral-sh/ruff/issues/13155
synced_at: 2026-01-07T13:12:15-06:00
---

# Support passing file content from stdin when `--add-noqa` is used

---

_Issue opened by @InSyncWithFoo on 2024-08-30 01:21_

Currently this treats `-` as a directory name:

```shell
$ ruff check --add-noqa -
```

The use case is editor integration, where a file might not be physically stored on the user's machine and/or saving to disk is (slightly) undesirable. IIRC, the language server also doesn't support this.


---

_Comment by @dhruvmanila on 2024-08-30 08:09_

Seems reasonable to support.

> IIRC, the language server also doesn't support this.

The language server does support it but in a limited manner as it's provided as a code action to add `noqa` comment to disable a single rule.

<img width="947" alt="Screenshot 2024-08-30 at 13 39 39" src="https://github.com/user-attachments/assets/add06209-8f38-4543-952c-a5a3796cb3ec">


---

_Label `cli` added by @dhruvmanila on 2024-08-30 08:10_

---

_Label `suppression` added by @dhruvmanila on 2024-08-30 08:10_

---

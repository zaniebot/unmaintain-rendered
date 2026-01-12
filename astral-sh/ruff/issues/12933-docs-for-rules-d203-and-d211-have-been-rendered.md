```yaml
number: 12933
title: Docs for rules D203 and D211 have been rendered inaccurate by Black formatting
type: issue
state: closed
author: veaviticus
labels:
  - bug
  - documentation
assignees: []
created_at: 2024-08-16T14:54:26Z
updated_at: 2024-08-16T15:27:38Z
url: https://github.com/astral-sh/ruff/issues/12933
synced_at: 2026-01-12T15:54:52Z
```

# Docs for rules D203 and D211 have been rendered inaccurate by Black formatting

---

_@veaviticus_

The docs for rule linting rule D203 fell afoul of last week's black upgrade.

In this commit black was run: https://github.com/astral-sh/ruff/commit/597c5f912411725e17970fe9d34de4b0c33a8e85

Which formatted the example code for the lint, and removed the blank line that the example is supposed to be showcasing: https://github.com/astral-sh/ruff/commit/597c5f912411725e17970fe9d34de4b0c33a8e85#diff-2f8b0cca706f79d3614697888a3fb04bad222bb0f5849b08c2411ad112ea3351L121

The two examples now appear equal:

![image](https://github.com/user-attachments/assets/0d1b5b88-9de0-4e6b-a81e-9f26cd37e74a)

https://docs.astral.sh/ruff/rules/one-blank-line-before-class/

This was released yesterday in 0.6.0 and the website was updated, making the examples on the website now invalid


---

_Comment by @charliermarsh on 2024-08-16 14:59_

Hah, thank you.

---

_Label `documentation` added by @charliermarsh on 2024-08-16 14:59_

---

_Label `bug` added by @charliermarsh on 2024-08-16 14:59_

---

_Comment by @charliermarsh on 2024-08-16 15:00_

\cc @dhruvmanila 

---

_Comment by @dhruvmanila on 2024-08-16 15:02_

Oops, thank you.

---

_Comment by @veaviticus on 2024-08-16 15:05_

FYI this also hit D211, and possibly others that deal with "extra newlines" that black could remove

---

_Renamed from "Docs for rule D203 have been formatted improperly by Black" to "Docs for rules D203 and D211 have been rendered inaccurate by Black formatting" by @veaviticus on 2024-08-16 15:07_

---

_Comment by @dhruvmanila on 2024-08-16 15:18_

Thanks, I'll send in a quick fix. @AlexWaygood Just a heads up that we should include this in the `0.6.1` release.

---

_Comment by @veaviticus on 2024-08-16 15:23_

I did a quick scan of the black reformat and I think these might be the only two rules affected.
All the others are just changing the location of the `...` filler

```
/// def foo(*args: int):
///     ...
/// def foo(*args: int): ...
```

which, to be honest, I kinda prefer it on the next line down to indicate "here is the function" rather than being on the same line, even if they are semantically equal

---

_Comment by @dhruvmanila on 2024-08-16 15:24_

I also searched the diff files for "line", "blank", "empty", "before" and these are the only two references I could find.

---

_Closed by @dhruvmanila on 2024-08-16 15:27_

---

_Closed by @dhruvmanila on 2024-08-16 15:27_

---

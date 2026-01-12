```yaml
number: 6019
title: "[Proposal] Ability to ignore certain types of commented-out code in `ERA001`"
type: issue
state: open
author: tylerlaprade
labels:
  - needs-decision
assignees: []
created_at: 2023-07-23T23:25:24Z
updated_at: 2024-11-03T21:39:24Z
url: https://github.com/astral-sh/ruff/issues/6019
synced_at: 2026-01-12T15:54:45Z
```

# [Proposal] Ability to ignore certain types of commented-out code in `ERA001`

---

_@tylerlaprade_

We have quite a few of these scattered around our codebase. `T201` reminds us if we forget to comment them back out after we're done debugging, but I wish I could avoid the warning while they're commented.
<img width="431" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/008f1a20-ca32-42b3-91fa-0b42124533fc">
I tried `task-tags=["TODO", "print"]`, but this didn't work since there's no space after the `print`. I'd like to see either parens added to the delimiter for `task-tags` exclusions, or a separate setting added for allowed functions in commented-out code, please.


---

_Label `needs-decision` added by @charliermarsh on 2023-07-24 02:23_

---

_Comment by @berzi on 2024-01-21 17:27_

Upvoted. PyCharm allows comments like `# language=regexp` to inject a language into a string in the following line, but this gets detected as commented out code by ruff and it's annoying to have to explicitly append a `# noqa: ERA001` each time.

The setting could work exactly like `dummy-variable-rgx`, meaning a Regex pattern that, if matched, causes the error not to trigger for that particular line (or better yet for it and following lines that would otherwise trigger it).

---

_Label `needs-decision` removed by @charliermarsh on 2024-11-03 21:26_

---

_Label `bug` added by @charliermarsh on 2024-11-03 21:26_

---

_Label `accepted` added by @charliermarsh on 2024-11-03 21:26_

---

_Label `bug` removed by @charliermarsh on 2024-11-03 21:39_

---

_Label `accepted` removed by @charliermarsh on 2024-11-03 21:39_

---

_Label `needs-decision` added by @charliermarsh on 2024-11-03 21:39_

---

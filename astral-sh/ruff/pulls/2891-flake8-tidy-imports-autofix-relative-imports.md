```yaml
number: 2891
title: "[`flake8-tidy-imports`] autofix relative imports"
type: pull_request
state: merged
author: sbrugman
labels:
  - bug
assignees: []
merged: true
base: main
head: tidy-imports-relative-autofix
created_at: 2023-02-14T08:55:38Z
updated_at: 2023-02-15T21:54:24Z
url: https://github.com/astral-sh/ruff/pull/2891
synced_at: 2026-01-12T15:55:11Z
```

# [`flake8-tidy-imports`] autofix relative imports

---

_@sbrugman_

Previous fix was bugged. This one is only fixing when the `module_path` is present, making it far more robust.

Closes #2764 and closes #2869

(Good luck with the Jetbrains Webinar tonight)

---

_Label `bug` added by @charliermarsh on 2023-02-14 22:20_

---

_Comment by @charliermarsh on 2023-02-14 22:20_

> (Good luck with the Jetbrains Webinar tonight)

Thank you, this was nice to read :)


---

_Merged by @charliermarsh on 2023-02-14 22:24_

---

_Closed by @charliermarsh on 2023-02-14 22:24_

---

_Branch deleted on 2023-02-14 23:10_

---

_Comment by @not-my-profile on 2023-02-15 11:06_

The first commit created `resources/test/popmon` as a git submodule? o.O What's up with that?

---

_Comment by @sbrugman on 2023-02-15 11:35_

Ah, that needs to be removed (thought that I had...). I use it for local testing only

---

_Comment by @gertvdijk on 2023-02-15 21:54_

It works, thanks all! 🙏🏼 

---

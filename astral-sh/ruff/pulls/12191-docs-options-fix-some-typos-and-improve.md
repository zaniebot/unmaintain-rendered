```yaml
number: 12191
title: "docs(options): fix some typos and improve consistency"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc/misc-options-improvements
created_at: 2024-07-04T21:25:45Z
updated_at: 2024-07-04T23:23:29Z
url: https://github.com/astral-sh/ruff/pull/12191
synced_at: 2026-01-10T21:47:02Z
```

# docs(options): fix some typos and improve consistency

---

_Pull request opened by @mkniewallner on 2024-07-04 21:25_

## Summary

Fixes a few typos and consistency issues in the "Settings" documentation:
- use "Ruff" consistently in the few places where "ruff" is used
- use double quotes in the few places where single quotes are used
- add backticks around rule codes where they are currently missing
- update a few example values where they are the same as the defaults, for consistency

2nd commit might be controversial, as there are many options mentioned where we don't currently link to the documentation sections, so maybe it's done on purpose, as this will also appear in the JSON schema where it's not desirable? If that's the case, I can easily drop it.

## Test Plan

Local testing.

---

_Marked ready for review by @mkniewallner on 2024-07-04 22:38_

---

_@charliermarsh approved on 2024-07-04 23:03_

Wow, this is excellent -- thank you! I skimmed through all the changes. How did you find these?

---

_Label `documentation` added by @charliermarsh on 2024-07-04 23:03_

---

_Comment by @charliermarsh on 2024-07-04 23:05_

> 2nd commit might be controversial, as there are many options mentioned where we don't currently link to the documentation sections, so maybe it's done on purpose, as this will also appear in the JSON schema where it's not desirable? If that's the case, I can easily drop it.

I think this _is_ true (that it will appear in the JSON Schema). But I think it's worth it.


---

_Merged by @charliermarsh on 2024-07-04 23:05_

---

_Closed by @charliermarsh on 2024-07-04 23:05_

---

_Branch deleted on 2024-07-04 23:06_

---

_Comment by @mkniewallner on 2024-07-04 23:07_

> Wow, this is excellent -- thank you! I skimmed through all the changes. How did you find these?

By manually skimming through the documentation (yeah, for real 😄).

---

_Comment by @charliermarsh on 2024-07-04 23:23_

I'm very impressed, thanks!

---

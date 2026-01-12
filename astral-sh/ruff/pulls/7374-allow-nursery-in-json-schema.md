```yaml
number: 7374
title: "Allow `NURSERY` in JSON Schema"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/json
created_at: 2023-09-14T00:52:49Z
updated_at: 2023-09-14T18:15:41Z
url: https://github.com/astral-sh/ruff/pull/7374
synced_at: 2026-01-12T15:55:23Z
```

# Allow `NURSERY` in JSON Schema

---

_@charliermarsh_

## Summary

At some point, we removed these so that they wouldn't be autocompleted for users, since we wanted to discourage usage of `ALL`. But given that they're valid values, I think that was a bad idea -- it leads to an even more confusing experience whereby JSON Schema validators tell you that you have an error, when you don't.

Closes https://github.com/astral-sh/ruff/issues/7261.

---

_Renamed from "Allow ALL, PREVIEW, and NURSERY in JSON Schema" to "Allow `ALL`, `PREVIEW`, and `NURSERY` in JSON Schema" by @charliermarsh on 2023-09-14 00:52_

---

_Review requested from @zanieb by @charliermarsh on 2023-09-14 00:53_

---

_Label `bug` added by @charliermarsh on 2023-09-14 00:53_

---

_Comment by @github-actions[bot] on 2023-09-14 01:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@dhruvmanila approved on 2023-09-14 03:16_

Makes sense 👍 

---

_Comment by @zanieb on 2023-09-14 04:22_

I'm debating removing the preview selector. Perhaps we should hold off for a little longer?

---

_Comment by @bersbersbers on 2023-09-14 08:52_

<s>By the way, it seems the schema is missing the `preview` property - this could be added in this PR maybe:</s>

![image](https://github.com/astral-sh/ruff/assets/12128514/da489d3f-1b5d-4652-abdc-e7cb049da7fe)

Never mind, I see it's working in the playground. Looks like https://json.schemastore.org/ruff.json is out of date.

---

_Comment by @zanieb on 2023-09-14 14:14_

@bersbersbers thanks I'll update that!

---

_Comment by @charliermarsh on 2023-09-14 15:22_

@zanieb - I can't remember if you've run it before but we have `scripts/update_schemastore.py` which will automate the update for you and even push the branch etc.

---

_Renamed from "Allow `ALL`, `PREVIEW`, and `NURSERY` in JSON Schema" to "Allow `NURSERY` in JSON Schema" by @charliermarsh on 2023-09-14 18:01_

---

_@zanieb approved on 2023-09-14 18:03_

I wish we could deprecate it here but 🤷‍♀️ 

---

_Merged by @charliermarsh on 2023-09-14 18:09_

---

_Closed by @charliermarsh on 2023-09-14 18:09_

---

_Branch deleted on 2023-09-14 18:09_

---

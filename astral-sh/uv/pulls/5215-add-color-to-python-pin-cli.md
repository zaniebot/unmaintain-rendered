```yaml
number: 5215
title: "Add color to `python pin` CLI"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/pin
created_at: 2024-07-19T13:03:21Z
updated_at: 2024-07-19T17:19:30Z
url: https://github.com/astral-sh/uv/pull/5215
synced_at: 2026-01-10T13:42:52Z
```

# Add color to `python pin` CLI

---

_Pull request opened by @charliermarsh on 2024-07-19 13:03_

## Summary

![Screenshot 2024-07-19 at 9 03 10 AM](https://github.com/user-attachments/assets/5668bd23-3f09-4964-bc09-9f3788f5a841)


---

_Label `cli` added by @charliermarsh on 2024-07-19 13:03_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 13:03_

---

_Label `preview` added by @charliermarsh on 2024-07-19 13:03_

---

_Marked ready for review by @charliermarsh on 2024-07-19 13:03_

---

_@charliermarsh reviewed on 2024-07-19 13:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/pin.rs`:78 on 2024-07-19 13:04_

I opted to remove this, I didn't find it necessary, but not a strong opinion. If we _do_ show it, I feel like it should tell you what we updated the pin _from_. That adds some complexity because it could be multiple values, right?

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:78 on 2024-07-19 13:52_

I saw Cargo use "Replaced" in its CLI and I liked it. I would like to show what we change from, that sounds nice. If it's multiple values, I think we can just take the first one which is effectively the "pinned" version.

---

_@zanieb reviewed on 2024-07-19 13:52_

---

_@charliermarsh reviewed on 2024-07-19 13:54_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/pin.rs`:78 on 2024-07-19 13:54_

Ok

---

_@zanieb reviewed on 2024-07-19 13:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:78 on 2024-07-19 13:57_

The utility to retrieve the version request from files will retrieve the first line for you already

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/pin.rs`:88 on 2024-07-19 16:42_

Like or dislike the choice to use `.python-version` in the message?

![Screenshot 2024-07-19 at 12 41 50 PM](https://github.com/user-attachments/assets/ffb3d95f-601d-4fc5-adc6-1042957baff5)


---

_@charliermarsh reviewed on 2024-07-19 16:42_

---

_Review request for @zanieb removed by @charliermarsh on 2024-07-19 16:42_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 16:42_

---

_@zanieb reviewed on 2024-07-19 16:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:88 on 2024-07-19 16:42_

That seems nice, I think, for when we add #4970.

---

_@zanieb reviewed on 2024-07-19 16:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:80 on 2024-07-19 16:44_

Should we use a right arrow symbol, e.g., →?

---

_@charliermarsh reviewed on 2024-07-19 17:03_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/pin.rs`:80 on 2024-07-19 17:03_

Done, I used the same arrow as in `uv lock --upgrade`

---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 17:03_

---

_Merged by @charliermarsh on 2024-07-19 17:19_

---

_Closed by @charliermarsh on 2024-07-19 17:19_

---

_Branch deleted on 2024-07-19 17:19_

---

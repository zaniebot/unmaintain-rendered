```yaml
number: 7867
title: "Less scary `ruff format` message"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: stablize-ruff-format-a-little
created_at: 2023-10-09T12:23:36Z
updated_at: 2023-10-11T11:55:21Z
url: https://github.com/astral-sh/ruff/pull/7867
synced_at: 2026-01-12T15:55:25Z
```

# Less scary `ruff format` message

---

_@konstin_

The ruff formatter has stabilized a good bit since when we added the original message "intended only for experimentation" message. It's time for a less scary message to indicate it is much closer to stable usage than it used to be.

I'm not particular about the specific wording, feel free to change the text to something better.

![image](https://github.com/astral-sh/ruff/assets/6826232/225db573-bfda-446f-a403-a577ee270a0b)


---

_Label `formatter` added by @konstin on 2023-10-09 12:23_

---

_Review requested from @charliermarsh by @konstin on 2023-10-09 12:23_

---

_Review requested from @zanieb by @konstin on 2023-10-09 12:23_

---

_@charliermarsh reviewed on 2023-10-09 19:24_

I would suggest:

```rust
warn_user_once!("`ruff format` is not yet stable, and subject to change in future versions.");
```


---

_@charliermarsh approved on 2023-10-11 01:02_

---

_Merged by @charliermarsh on 2023-10-11 11:46_

---

_Closed by @charliermarsh on 2023-10-11 11:46_

---

_Branch deleted on 2023-10-11 11:46_

---

_Comment by @github-actions[bot] on 2023-10-11 11:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

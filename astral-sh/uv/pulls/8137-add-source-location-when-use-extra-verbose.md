```yaml
number: 8137
title: add source location when use extra verbose
type: pull_request
state: closed
author: Renkai
labels:
  - tracing
assignees: []
base: main
head: add-source-location-in-extra-verbose
created_at: 2024-10-12T01:01:58Z
updated_at: 2024-10-28T15:54:51Z
url: https://github.com/astral-sh/uv/pull/8137
synced_at: 2026-01-12T16:08:10Z
```

# add source location when use extra verbose

---

_@Renkai_

_No description provided._

---

_Marked ready for review by @Renkai on 2024-10-12 01:04_

---

_Label `tracing` added by @charliermarsh on 2024-10-12 03:44_

---

_Comment by @charliermarsh on 2024-10-22 21:37_

I defer to @konstin on whether this is helpful or harmful.

Before:

![Screenshot 2024-10-22 at 5 37 16 PM](https://github.com/user-attachments/assets/0c2510e1-bc82-434f-9eb1-c6f279b3a8f4)

After:

![Screenshot 2024-10-22 at 5 37 13 PM](https://github.com/user-attachments/assets/c479e641-4e5f-448f-9569-21e4dfbe5691)


---

_Assigned to @konstin by @charliermarsh on 2024-10-22 21:37_

---

_Review requested from @BurntSushi by @konstin on 2024-10-23 09:19_

---

_Review requested from @zanieb by @konstin on 2024-10-23 09:19_

---

_Comment by @konstin on 2024-10-23 09:20_

This is fine by me since i barely ever use `-vvv` and i see how it helps in some cases; tagging other team members who may use `-vvv` to not break their workflows.

---

_@konstin approved on 2024-10-23 09:20_

---

_Comment by @BurntSushi on 2024-10-28 13:39_

I don't use `-vvv` often either. This does seem to really increase the amount of output, but I don't think I have any strong objections. With that said, I think merging this would probably mean that I would definitely _never_ use `-vvv` because the output is such that it's hard to read.

Is there a specific problem this extra level of logging is trying to solve?

---

_Comment by @zanieb on 2024-10-28 13:51_

Users do sometimes report things with `-vvv` assuming that's best practice for reporting an issue.

---

_Comment by @charliermarsh on 2024-10-28 15:54_

Let's close this out for now since the general consensus on the team seems to be that it's a net negative. Sorry about that and thanks for the contribution @Renkai.

---

_Closed by @charliermarsh on 2024-10-28 15:54_

---

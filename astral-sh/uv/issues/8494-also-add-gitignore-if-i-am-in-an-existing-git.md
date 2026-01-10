---
number: 8494
title: "Also add `.gitignore` if I am in an existing git repository"
type: issue
state: closed
author: kamulos
labels:
  - needs-decision
assignees: []
created_at: 2024-10-23T09:51:36Z
updated_at: 2024-10-23T12:59:25Z
url: https://github.com/astral-sh/uv/issues/8494
synced_at: 2026-01-10T01:24:29Z
---

# Also add `.gitignore` if I am in an existing git repository

---

_Issue opened by @kamulos on 2024-10-23 09:51_

My `uv` versuin is `0.4.25`.

I just initialized a folder in my existing git repo with a new `uv` project. In this case it seems to skip creating the `.gitignore` file in that folder even though the folder was completely empty when I executed `uv init`. My preference would be to create the default `.gitignore` in this case, but maybe some other thoughts went into it when it was implemented. What do you think?


---

_Comment by @charliermarsh on 2024-10-23 12:57_

I think this was done intentionally to match Cargo.

---

_Label `needs-decision` added by @charliermarsh on 2024-10-23 12:58_

---

_Comment by @charliermarsh on 2024-10-23 12:59_

I'm going to close as "working as intended" though I know it's sometimes desired.

---

_Closed by @charliermarsh on 2024-10-23 12:59_

---

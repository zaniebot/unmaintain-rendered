---
number: 5551
title: 0.0.277 release was not completed
type: issue
state: closed
author: chenrui333
labels: []
assignees: []
created_at: 2023-07-06T02:27:08Z
updated_at: 2023-07-06T03:44:54Z
url: https://github.com/astral-sh/ruff/issues/5551
synced_at: 2026-01-10T01:22:44Z
---

# 0.0.277 release was not completed

---

_Issue opened by @chenrui333 on 2023-07-06 02:27_

👋 it looks like 0.0.277 release was not completed, see [this action run](https://github.com/astral-sh/ruff/actions/runs/5458523781), raise this issue for some awareness. Thanks!

relates to https://github.com/Homebrew/homebrew-core/pull/135796

---

_Comment by @charliermarsh on 2023-07-06 02:34_

Hmm, I think it was completed? This is the actual release run: https://github.com/astral-sh/ruff/actions/runs/5458525309.

---

_Comment by @chenrui333 on 2023-07-06 02:39_

somehow the release attached builds attached to the git tag did not show as success
![image](https://github.com/astral-sh/ruff/assets/1580956/a6e3b5bd-442c-4ec7-9ba8-167a5f16fbb5)

Also there is no release labeling for 0.0.277

![image](https://github.com/astral-sh/ruff/assets/1580956/98932b3d-0e74-43ee-a743-a7ed67582074)


---

_Comment by @charliermarsh on 2023-07-06 02:44_

Oh, it was marked as a draft. I just marked it as latest: https://github.com/astral-sh/ruff/releases/tag/v0.0.277. But the tag already existed?

---

_Comment by @charliermarsh on 2023-07-06 02:56_

(That first screenshot is the release dry-run jobs running on commit, not the actual release job.)

---

_Comment by @charliermarsh on 2023-07-06 02:59_

Can I help at all w/ the Homebrew release? Is it blocked?

---

_Comment by @chenrui333 on 2023-07-06 03:27_

yeah, it was blocked mostly due to the draft release thing. I think I am unblocked now, thanks!

---

_Closed by @chenrui333 on 2023-07-06 03:27_

---

_Comment by @chenrui333 on 2023-07-06 03:33_

shipping the homebrew formula update now, thanks again!

---

_Comment by @charliermarsh on 2023-07-06 03:41_

Thank you! Sorry for the confusion, new release workflow as of recently.

---

_Comment by @chenrui333 on 2023-07-06 03:44_

no worries, happy to help

---

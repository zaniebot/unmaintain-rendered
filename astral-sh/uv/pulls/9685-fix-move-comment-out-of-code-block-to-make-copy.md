```yaml
number: 9685
title: "fix: move comment out of code block to make copy/paste easier"
type: pull_request
state: closed
author: lifehackett
labels: []
assignees: []
base: main
head: update-docs
created_at: 2024-12-06T15:06:36Z
updated_at: 2025-04-28T12:56:47Z
url: https://github.com/astral-sh/uv/pull/9685
synced_at: 2026-01-12T16:08:56Z
```

# fix: move comment out of code block to make copy/paste easier

---

_@lifehackett_

## Summary

Improve the docs and make getting started easier. Move the comments on which environment/tool to use out of the code blocks so you can copy and paste directly into the terminal


---

_Comment by @zanieb on 2024-12-06 15:12_

These should be copy-pastable into the terminal already.

See the previous change that touched these blocks https://github.com/astral-sh/uv/pull/8853

---

_Comment by @zanieb on 2024-12-06 15:13_

Did these not work as-is for you? It may still be reasonable to move the comments out, I think it just consumes more space in the README that way.

---

_Comment by @lifehackett on 2024-12-06 15:17_

Retried and it worked. Looked at the previous failure and I had a lingering character that I missed. I prefer to copy/paste without the comment, but that's just personal preference. It's your repo, so happy to close this

---

_Comment by @zanieb on 2024-12-06 15:21_

Just for comparison-sake

<img width="1755" alt="Screenshot 2024-12-06 at 9 21 10 AM" src="https://github.com/user-attachments/assets/33cde73f-fe91-4f96-aecf-529f3ad58a20">

The vertical space consumption is negligible imo, but I don't prefer the readability / styling. I'm not sure how we could improve that.

---

_Comment by @lifehackett on 2024-12-06 15:34_

Checked a few places to compare

Headers & bullets
https://github.com/pypa/pipx?tab=readme-ov-file#install-pipx

Uses a table format at the expense of copy/paste
https://github.com/ohmyzsh/ohmyzsh?tab=readme-ov-file#basic-installation

Poetry & Httpie refer you to their docs site


---

_Comment by @konstin on 2025-04-28 12:56_

Closing this PR, since we're unlikely to change this for now.

---

_Closed by @konstin on 2025-04-28 12:56_

---

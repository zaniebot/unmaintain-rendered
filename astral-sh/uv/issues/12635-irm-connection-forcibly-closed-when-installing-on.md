```yaml
number: 12635
title: irm connection forcibly closed when installing on windows
type: issue
state: open
author: oseymour
labels:
  - question
  - external
assignees: []
created_at: 2025-04-02T18:51:07Z
updated_at: 2026-01-14T12:04:33Z
url: https://github.com/astral-sh/uv/issues/12635
synced_at: 2026-01-14T12:44:04Z
```

# irm connection forcibly closed when installing on windows

---

_@oseymour_

### Question

I tried running the following command to install on Windows: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`. From the [installation docs page](https://docs.astral.sh/uv/getting-started/installation/). This command worked for me previously.

But I'm getting the following error now:
![Image](https://github.com/user-attachments/assets/095aa375-0bd0-4abe-b468-ca2353f41f06)

This is on my work laptop (corporate IT-managed 🤮). But as I said, this command worked for me previously. Has something changed on the uv side that would cause this error? Or do I need to talk to our IT department?

### Platform

Windows 10 x64

### Version

n/a

---

_Label `question` added by @oseymour on 2025-04-02 18:51_

---

_Comment by @zanieb on 2025-04-02 19:21_

Nothing has changed on our end, afaik.

---

_Label `external` added by @zanieb on 2025-04-02 19:21_

---

_Comment by @RangerCoder99 on 2026-01-14 12:03_

Same issue here :(

---

_Comment by @konstin on 2026-01-14 12:04_

Can you share more details on your setup? To debug this, we need to figure why this happens for you, but not for other users.

---

---
number: 12933
title: Website scrolling behaviour
type: issue
state: closed
author: valetobias
labels:
  - documentation
  - needs-mre
assignees: []
created_at: 2025-04-17T06:41:00Z
updated_at: 2025-04-22T10:10:17Z
url: https://github.com/astral-sh/uv/issues/12933
synced_at: 2026-01-10T01:25:27Z
---

# Website scrolling behaviour

---

_Issue opened by @valetobias on 2025-04-17 06:41_

### Summary

Your [website](https://docs.astral.sh/uv/) does not allow scrolling on Chrome.

When scrolling it jumps back up to the top. I presume this is due to your dynamic updating of the link with the section you are in.

The website works as expected on Firefox.

### Platform

Windows 11 x86_64, Chrome 135.0.7049.86

### Version

irrelevant

### Python version

irrelevant

---

_Label `bug` added by @valetobias on 2025-04-17 06:41_

---

_Comment by @konstin on 2025-04-17 09:28_

I can't reproduce this, what is your screen resolution and are you using any add-ons?

---

_Label `bug` removed by @konstin on 2025-04-17 09:28_

---

_Label `documentation` added by @konstin on 2025-04-17 09:28_

---

_Label `needs-mre` added by @konstin on 2025-04-17 09:28_

---

_Comment by @valetobias on 2025-04-17 14:43_

I can't reproduce it anymore either, have however installed a chrome update since then, consider solved I guess

---

_Closed by @valetobias on 2025-04-17 14:44_

---

_Comment by @laszlovandenhoek on 2025-04-22 10:10_

Can confirm that the problem stopped occurring after upgrading to Chrome 135.0.7049.96.

---

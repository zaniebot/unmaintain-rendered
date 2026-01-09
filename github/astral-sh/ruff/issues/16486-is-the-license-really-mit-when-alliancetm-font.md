---
number: 16486
title: Is the license really MIT when Alliance™ font family by Degarism Studio is a used font in the css
type: issue
state: closed
author: madelavar12
labels:
  - question
  - playground
assignees: []
created_at: 2025-03-04T00:16:49Z
updated_at: 2025-03-04T14:36:37Z
url: https://github.com/astral-sh/ruff/issues/16486
synced_at: 2026-01-07T13:12:16-06:00
---

# Is the license really MIT when Alliance™ font family by Degarism Studio is a used font in the css

---

_Issue opened by @madelavar12 on 2025-03-04 00:16_

### Question

Please verify that ruff license is really MIT since it appears that Alliance™ font family by Degarism Studio is used in the css files.

The files are located at:

playground/src/index.css

### Version

0.9.9

---

_Label `question` added by @madelavar12 on 2025-03-04 00:16_

---

_Comment by @InSyncWithFoo on 2025-03-04 01:45_

The font has been there ever since #5838 (cc @MichaReiser); it was probably copied from [the company website](https://astral.sh).

---

_Comment by @charliermarsh on 2025-03-04 01:51_

Yes of course. We can pull the fonts in externally though, not hard.

---

_Comment by @MichaReiser on 2025-03-04 09:46_

Hmm, good call. @charliermarsh, could you publish the fonts on a CDN or similar with a static URL? I can't use the URLs from the website because they use file hashes, which seems somewhat fragile.

---

_Label `playground` added by @MichaReiser on 2025-03-04 09:53_

---

_Comment by @charliermarsh on 2025-03-04 13:21_

Yeah no worries. I can do it.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-04 14:06_

---

_Referenced in [astral-sh/ruff#16498](../../astral-sh/ruff/pulls/16498.md) on 2025-03-04 14:19_

---

_Closed by @charliermarsh on 2025-03-04 14:36_

---

_Closed by @charliermarsh on 2025-03-04 14:36_

---

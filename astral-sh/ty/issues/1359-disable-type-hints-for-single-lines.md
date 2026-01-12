```yaml
number: 1359
title: Disable type hints for single lines
type: issue
state: closed
author: silvasta
labels:
  - question
  - server
assignees: []
created_at: 2025-10-15T11:39:16Z
updated_at: 2025-10-15T12:27:30Z
url: https://github.com/astral-sh/ty/issues/1359
synced_at: 2026-01-12T15:54:25Z
```

# Disable type hints for single lines

---

_@silvasta_

### Question

Most of the time the feature with the type hints is very useful so for sure I don't want to deactivate that also not for single files. Unfortunately I just found the instructions how to do that.

**Is it possible to deactivate type hints for single lines?**
 
For example like with the error messages for one line with # ty:ignore

In the attached image one can see the issue with some of the type hints:

<img width="2555" height="360" alt="Image" src="https://github.com/user-attachments/assets/6252f248-b006-40c0-949f-6a06f6f48f80" />

I copied a "1" for 500 times, that line was slightly longer than the hint...

<img width="366" height="125" alt="Image" src="https://github.com/user-attachments/assets/216e59f0-a38b-4a37-9b1b-06a1c8087516" />

**Would be amazing if you can tell me how to do that or include this feature if it wasn't me who missed something in the documentation.**
(I use v0.0.1a22 in neovim installed with mason v2.1.0)

Already now I like ty much more than pywright which is already uninstalled.
uv, ruff, ty - you guys are amazing 👍🏻 


### Version

0.0.1a22

---

_Label `question` added by @silvasta on 2025-10-15 11:39_

---

_Label `server` added by @AlexWaygood on 2025-10-15 11:43_

---

_Comment by @MichaReiser on 2025-10-15 12:27_

Yeah, this doesn't look very useful. There's no option to disable inlays for a single line. We should avoid rendering too long inlays, see https://github.com/astral-sh/ty/issues/494

---

_Closed by @MichaReiser on 2025-10-15 12:27_

---

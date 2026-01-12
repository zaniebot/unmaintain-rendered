```yaml
number: 5921
title: Prioritize platform-compatible distributions during universal resolve
type: issue
state: open
author: charliermarsh
labels:
  - enhancement
  - performance
assignees: []
created_at: 2024-08-08T17:20:51Z
updated_at: 2025-12-18T01:27:00Z
url: https://github.com/astral-sh/uv/issues/5921
synced_at: 2026-01-12T15:59:00Z
```

# Prioritize platform-compatible distributions during universal resolve

---

_@charliermarsh_

See: https://github.com/astral-sh/uv/pull/5845.

This is beneficial because (1) we can safely reuse state across the resolver and installer, and then (2) we're more likely to (e.g.) download wheels that we'll end up installing.

---

_Label `enhancement` added by @charliermarsh on 2024-08-08 17:20_

---

_Label `preview` added by @charliermarsh on 2024-08-08 17:20_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Label `performance` added by @charliermarsh on 2024-09-13 17:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-13 17:28_

---

_Comment by @charliermarsh on 2024-09-13 22:56_

The (major) downside here is that the lockfile resolution will now vary from machine to machine in the event that a package has inconsistent metadata across wheels...

---

_Comment by @hiromon0125 on 2025-12-13 07:34_

Hey, I was wondering if there was any resolution for this issue. I’ve been having a weird behavior for installing mediapipe as a dependency. Seems like people also tried tool.uv.sources and pointing directly to universal version and it still wants to reinstall after every run command. 

---

_Comment by @konstin on 2025-12-16 10:52_

@hiromon0125 This is a separate probolem, because mediapipe produces invalid wheels where the wheel tag and the entry in `WHEEL` are different: https://inspector.pypi.io/project/mediapipe/0.10.21/packages/66/85/5be11ee221b04cd1496131222199ad6469bd49e377157b84b0895325afb2/mediapipe-0.10.21-cp310-cp310-macosx_11_0_universal2.whl/mediapipe-0.10.21.dist-info/WHEEL#line.4 . This needs to be fixed upstream by the mediapipe developers (https://github.com/google-ai-edge/mediapipe/issues/6112), this is not a bug in uv.

Re this bug, I don't think we'll change the behavior (https://github.com/astral-sh/uv/pull/7381), the determinism is more important and the overhead is small with METADATA-in-index and range requests.

---

_Comment by @hiromon0125 on 2025-12-18 01:27_

I see. Thank you for the insight. 

---

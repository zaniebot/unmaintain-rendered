---
number: 13403
title: uv vs rye
type: issue
state: closed
author: PabloVD
labels:
  - question
assignees: []
created_at: 2025-05-12T10:10:41Z
updated_at: 2025-05-13T08:17:52Z
url: https://github.com/astral-sh/uv/issues/13403
synced_at: 2026-01-10T01:25:33Z
---

# uv vs rye

---

_Issue opened by @PabloVD on 2025-05-12 10:10_

### Question

I came to uv from [rye](https://rye.astral.sh/), as it was recommended in the home page as its successor. For now the migration has been pretty smooth, the user experience in both frameworks is very similar.

Is there any reason to keep using rye over uv? Any advantage of rye that uv does not support yet?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @PabloVD on 2025-05-12 10:10_

---

_Comment by @konstin on 2025-05-12 14:42_

Overall, uv is more mature, better supported and has more features than rye. The main feature that's still missing is the task runner (https://github.com/astral-sh/uv/issues/5903), but generally use uv over rye.

---

_Closed by @charliermarsh on 2025-05-13 02:58_

---

_Comment by @charliermarsh on 2025-05-13 02:59_

Yeah, we recommend using uv over Rye -- it's more feature-complete, better supported, and more extensively-tested. The main missing feature (as @konstin said) is the task runner.

---

_Comment by @PabloVD on 2025-05-13 08:17_

I see, thanks for your answers!

---

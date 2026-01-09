---
number: 1734
title: Implement W291 (trailing whitespace)?
type: issue
state: closed
author: harupy
labels: []
assignees: []
created_at: 2023-01-08T04:05:24Z
updated_at: 2023-01-08T04:46:18Z
url: https://github.com/astral-sh/ruff/issues/1734
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement W291 (trailing whitespace)?

---

_Issue opened by @harupy on 2023-01-08 04:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

It'd be nice if Ruff could detect trailing whitespace:

<img width="1009" alt="image" src="https://user-images.githubusercontent.com/17039389/211179938-4a48eefd-483d-4f07-972b-11e539501a92.png">

(even though black can eliminate them)

---

_Renamed from "Implement W291?" to "Implement W291 (trailing whitespace)?" by @harupy on 2023-01-08 04:06_

---

_Comment by @charliermarsh on 2023-01-08 04:16_

I don't know if it's relevant (haven't thought through how we would implement trailing whitespace), but I started implementing Pycodestyle's logical line detection in https://github.com/charliermarsh/ruff/pull/1130 -- although I felt at the time that it was a bit too slow to be worthwhile as a default.


---

_Comment by @charliermarsh on 2023-01-08 04:16_

Do these checks typically exclude trailing whitespace within strings?

---

_Comment by @not-my-profile on 2023-01-08 04:22_

Black preserves trailing whitespace in multiline strings (as it should). I just checked and pycodestyle does trigger W291 for trailing whitespace within multiline strings. To be honest I don't really see the point of this lint.

---

_Comment by @charliermarsh on 2023-01-08 04:24_

I think it's a perfectly useful rule, _if you don't use an autoformatter_.

---

_Comment by @not-my-profile on 2023-01-08 04:27_

Haha yes I don't see why you wouldn't.^^

---

_Comment by @harupy on 2023-01-08 04:35_

@charliermarsh @not-my-profile Agreed! Let me close this.

---

_Closed by @harupy on 2023-01-08 04:35_

---

_Comment by @harupy on 2023-01-08 04:37_

> Black preserves trailing whitespace in multiline strings (as it should). I just checked and pycodestyle does trigger W291 for trailing whitespace within multiline strings. To be honest I don't really see the point of this lint.

Yes, black does. We can align with black and ignore trailing whitespace in strings (as shown in the attached screenshot).

---

_Referenced in [astral-sh/ruff#3008](../../astral-sh/ruff/issues/3008.md) on 2023-02-18 12:57_

---

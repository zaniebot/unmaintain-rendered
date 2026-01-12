```yaml
number: 7331
title: Resolve with deps up to certain date
type: issue
state: closed
author: nmichlo
labels:
  - question
assignees: []
created_at: 2024-09-12T14:12:33Z
updated_at: 2024-09-12T14:24:56Z
url: https://github.com/astral-sh/uv/issues/7331
synced_at: 2026-01-12T15:59:12Z
```

# Resolve with deps up to certain date

---

_@nmichlo_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


Hi, it would be quite useful for old projects which don't have locked requirements to be able to resolve specifically using dependencies up to some point in time.

This would be hugely helpful in making sure that selected deps are actually compatible with the code when they haven't been locked by the original authors. Reps could normally resolve, but may be incompatible and you need to manually find version numbers up to a certain date. Furthermore, original authors might not have locked deps properly e.g. specifying `numpy>=1` instead of `numpy>=1,<2` -- If there was a lockfile for the project it would work fine, but if there isn't then it could easily resolve to v2 which might break things.


e.g. `uv pip compile -r requirements.txt --before=2018-02-30`



---

_Comment by @ibraheemdev on 2024-09-12 14:15_

Does `--exclude-newer 2018-02-30` work for your use case?

---

_Comment by @nmichlo on 2024-09-12 14:16_

Oh my, I missed this. Thank you

---

_Closed by @nmichlo on 2024-09-12 14:16_

---

_Label `question` added by @ibraheemdev on 2024-09-12 14:24_

---

_Label `question` added by @ibraheemdev on 2024-09-12 14:24_

---

_Label `question` added by @ibraheemdev on 2024-09-12 14:24_

---

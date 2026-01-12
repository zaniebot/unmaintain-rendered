```yaml
number: 2426
title: "What is uv's versioning policy?"
type: issue
state: closed
author: hugovk
labels:
  - question
assignees: []
created_at: 2024-03-13T20:36:48Z
updated_at: 2024-03-17T21:41:22Z
url: https://github.com/astral-sh/uv/issues/2426
synced_at: 2026-01-12T15:58:37Z
```

# What is uv's versioning policy?

---

_@hugovk_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When Ruff reached 0.1.0, a custom versioning scheme was adopted, which is sort of like SemVer with an extra 0. at the start:

* https://docs.astral.sh/ruff/versioning/
* https://github.com/astral-sh/ruff/discussions/6998

uv is already at 0.1.8 but it has a breaking change: https://github.com/astral-sh/uv/releases/tag/0.1.18. This is not a number I'd pay attention to for a breaking change :)

What is uv's versioning policy? SemVer? Custom like Ruff? Another custom scheme? Or even CalVer like pip?


---

_Comment by @zanieb on 2024-03-13 20:49_

We do not have a versioning policy yet since it's such a new product. I think we're likely to bump the minor version number for major breaking changes but there _will_ be small breaking changes in patch releases as we rapidly improve uv. We're very likely to switch to SemVer with `1.0.0` but may introduce a versioning policy before then.

---

_Label `question` added by @zanieb on 2024-03-13 20:50_

---

_Comment by @charliermarsh on 2024-03-17 21:41_

(Closing as answered _for now_.)

---

_Closed by @charliermarsh on 2024-03-17 21:41_

---

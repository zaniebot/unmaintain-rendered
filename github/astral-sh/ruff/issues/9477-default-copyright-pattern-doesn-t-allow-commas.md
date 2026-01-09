---
number: 9477
title: "Default copyright pattern doesn't allow commas"
type: issue
state: closed
author: dopplershift
labels:
  - bug
assignees: []
created_at: 2024-01-11T21:27:54Z
updated_at: 2024-03-22T18:43:25Z
url: https://github.com/astral-sh/ruff/issues/9477
synced_at: 2026-01-07T13:12:15-06:00
---

# Default copyright pattern doesn't allow commas

---

_Issue opened by @dopplershift on 2024-01-11 21:27_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

As of ruff 0.1.11, the default `notice-rgx` does not allow for headers that have commas, like:

> Copyright (c) 2018, 2019 Developers

[Part of the docs](https://github.com/astral-sh/ruff/blob/eb4ed2471b48d6ccba74064c65e37e32cba41a6c/crates/ruff_workspace/src/options.rs#L1065-L1076) do say that `(?i)Copyright\s+(\(C\)\s+)?\d{4}([-,]\d{4})*` is the default pattern (which works), but there's also `(?i)Copyright\s+(\(C\)\s+)?\d{4}(-\d{4})*` mentioned in a separate sentence in the docs and matches what's in [`COPYRIGHT`](https://github.com/astral-sh/ruff/blob/eb4ed2471b48d6ccba74064c65e37e32cba41a6c/crates/ruff_linter/src/rules/flake8_copyright/settings.rs#L16C29-L16C73) (and reflects actual behavior). Any reason commas shouldn't be accepted?


---

_Comment by @charliermarsh on 2024-01-12 00:29_

I think it might be an oversight. We should probably allow commas there.

---

_Label `bug` added by @charliermarsh on 2024-01-12 00:29_

---

_Referenced in [astral-sh/ruff#9498](../../astral-sh/ruff/pulls/9498.md) on 2024-01-12 21:17_

---

_Comment by @dopplershift on 2024-01-12 21:17_

PR submitted in #9498.

---

_Comment by @charliermarsh on 2024-01-25 05:10_

I believe this _is_ allowed, at least in my testing: https://play.ruff.rs/b1dc1b2a-afec-48e6-82b5-ce7eb68fbf33. Closing for now but happy to revisit!

---

_Closed by @charliermarsh on 2024-01-25 05:10_

---

_Comment by @dopplershift on 2024-02-14 22:43_

@charliermarsh I think this and #9498 were closed in error, my local testing still shows a need to manually set `notice_rgx` in order to get this behavior accepting commas.

I think the reason your testing didn't show a problem is because you have to set `tool.ruff.lint.flake8-copyright.author` to `"Developers"` in order to enable the check I believe.

I can't figure out how to share it, but I verified this by setting the author properly on your link above.

---

_Comment by @dopplershift on 2024-03-01 00:05_

Ping @charliermarsh 

---

_Comment by @charliermarsh on 2024-03-01 00:06_

Ack, will take another look!

---

_Comment by @dopplershift on 2024-03-22 17:35_

Ping since there's a PR to fix sitting in #9498.

---

_Reopened by @charliermarsh on 2024-03-22 18:13_

---

_Comment by @charliermarsh on 2024-03-22 18:14_

Thanks, I think you're right, that does it. I'll get it merged now, but need to add tests.

---

_Closed by @charliermarsh on 2024-03-22 18:42_

---

_Comment by @dopplershift on 2024-03-22 18:43_

Thanks!

---

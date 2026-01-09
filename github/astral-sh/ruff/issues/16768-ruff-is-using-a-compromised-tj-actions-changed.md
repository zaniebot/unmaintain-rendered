---
number: 16768
title: ruff is using a compromised tj-actions/changed-files GitHub actio
type: issue
state: closed
author: eslerm
labels:
  - ci
  - security
assignees: []
created_at: 2025-03-15T19:39:39Z
updated_at: 2025-03-17T12:47:09Z
url: https://github.com/astral-sh/ruff/issues/16768
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff is using a compromised tj-actions/changed-files GitHub actio

---

_Issue opened by @eslerm on 2025-03-15 19:39_

Filing a public issue instead of reporting this as a private vulnerability, since this malware is a publicly known and an urgent issue.

ruff uses a compromised version of tj-actions/changed-files. The compromised action appears to leak secrets the runner has in memory.

The action is included in: https://github.com/astral-sh/ruff/blob/main/.github/workflows/ci.yaml

Output of an affected run on ruff: https://github.com/astral-sh/ruff/actions/runs/13868731237/job/38812473949?pr=16641#step:3:113

Please review.

Learn about the compromise on [StepSecurity](https://www.stepsecurity.io/blog/harden-runner-detection-tj-actions-changed-files-action-is-compromised) of [Semgrep](https://semgrep.dev/blog/2025/popular-github-action-tj-actionschanged-files-is-compromised/).

---

_Comment by @ntBre on 2025-03-15 19:55_

Thank you! We were notified about this yesterday and currently have all non-Astral actions disabled (effectively disabling CI) while we work on a longer-term fix. We also rotated all of our secrets that could have been affected.

---

_Label `ci` added by @ntBre on 2025-03-15 19:56_

---

_Label `security` added by @ntBre on 2025-03-15 19:56_

---

_Referenced in [astral-sh/ruff#16765](../../astral-sh/ruff/pulls/16765.md) on 2025-03-16 22:25_

---

_Comment by @ntBre on 2025-03-17 12:47_

This should be resolved by:
1. https://github.com/astral-sh/ruff/pull/16788, which removed the compromised action
2. https://github.com/astral-sh/ruff/pull/16789, which should prevent similar tag-altering attacks in the future

We also replaced the action's functionality with a git command (https://github.com/astral-sh/ruff/pull/16796) like in uv (https://github.com/astral-sh/uv/pull/12188) and python-build-standalone (https://github.com/astral-sh/python-build-standalone/pull/559).

Thank you again for the report!

---

_Closed by @ntBre on 2025-03-17 12:47_

---

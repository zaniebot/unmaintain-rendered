---
number: 7993
title: Preview rule stabilizations for v0.2.0
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-10-16T19:29:32Z
updated_at: 2024-02-01T19:35:04Z
url: https://github.com/astral-sh/ruff/issues/7993
synced_at: 2026-01-07T13:12:15-06:00
---

# Preview rule stabilizations for v0.2.0

---

_Issue opened by @zanieb on 2023-10-16 19:29_

New rules are now added in preview. This issue serves as a central discussion for rules that should be promoted from preview to stable in the next breaking version of Ruff.

---

_Added to milestone `v0.2.0` by @zanieb on 2023-10-16 19:29_

---

_Referenced in [astral-sh/ruff#8005](../../astral-sh/ruff/issues/8005.md) on 2023-10-17 13:55_

---

_Renamed from "Rule stabilizations for v0.2.0" to "Preview rule stabilizations for v0.2.0" by @zanieb on 2023-10-17 13:56_

---

_Comment by @NeilGirdhar on 2023-10-17 17:42_

I tried out the previewed rules, and I got useful hits from:

* E114 Indentation is not a multiple of 4 (comment)
* E116 Unexpected indentation (comment)
* E203 [*] Whitespace before ','
* E241 Multiple spaces after comma
* E271 Multiple spaces after keyword
* FURB113 [*] Use `args.extend(...)` instead of repeatedly calling `args.append()`

Also, see this comment on [PLR6301](https://github.com/astral-sh/ruff/issues/8025)

---

_Comment by @dhruvmanila on 2023-10-18 05:12_

Do we want to add all the existing rules in preview here as a checklist?

---

_Referenced in [astral-sh/ruff#8752](../../astral-sh/ruff/issues/8752.md) on 2023-11-18 06:42_

---

_Comment by @charliermarsh on 2023-11-24 15:22_

I'd love to drop some duplicate rules in the next minor, e.g., https://github.com/astral-sh/ruff/issues/8830.

---

_Comment by @charliermarsh on 2023-11-30 01:31_

I think we should elevate the `C` rule fixes to "safe". I believe they're unsafe right now because we risk dropping comments, but that's not _really_ the intent of fix safety.

---

_Comment by @charliermarsh on 2023-11-30 01:41_

We should also elevate `UP006` and `UP007` to safe in some cases.

---

_Referenced in [astral-sh/ruff#8918](../../astral-sh/ruff/pulls/8918.md) on 2023-11-30 01:42_

---

_Comment by @zanieb on 2023-11-30 01:58_

> I'd love to drop some duplicate rules in the next minor, e.g., #8830.

Can we remove rules in preview? If not, maybe we should add a "Deprecated" rule group that acts as removed when preview mode is enabled.

---

_Comment by @charliermarsh on 2023-12-05 21:28_

I think we should also deprecate `PLR1706` (https://github.com/astral-sh/ruff/issues/9007).

---

_Referenced in [astral-sh/ruff#9461](../../astral-sh/ruff/issues/9461.md) on 2024-01-11 01:17_

---

_Referenced in [astral-sh/ruff#9680](../../astral-sh/ruff/pulls/9680.md) on 2024-01-30 19:18_

---

_Closed by @zanieb on 2024-02-01 19:35_

---

---
number: 10239
title: "File-scoped `flake8: noqa` directive with comment is not recognized"
type: issue
state: closed
author: tylerlaprade
labels:
  - suppression
assignees: []
created_at: 2024-03-05T17:45:38Z
updated_at: 2024-03-11T17:21:42Z
url: https://github.com/astral-sh/ruff/issues/10239
synced_at: 2026-01-07T13:12:15-06:00
---

# File-scoped `flake8: noqa` directive with comment is not recognized

---

_Issue opened by @tylerlaprade on 2024-03-05 17:45_

```
# flake8: noqa -- comment explaining why we're disabling flake8 for this file
```
As per [Ruff's documentation](https://docs.astral.sh/ruff/linter/#error-suppression), this is treated the same as `# ruff: noqa`, which doesn't allow comments. However, `flake8` _does_ allow comments - and I happen to believe comments should be required any time some/all rules are disabled to explain why. This particular comment is in fact being used to disable `flake8` for this file. The author would have liked to also disable Ruff for the whole file, but didn't realize that the comment could do both due to the comment, and ended up disabling the particular line with a Ruff violation.

This came to my attention when I upgraded to Ruff v0.3.0 (I'm very excited to try the new formatting, by the way!). I ran `ruff check` and got a warning every time:
```
warning: Invalid `# ruff: noqa` directive at abacus/migrations/0324_manual_recompute_all_patient_journeys.py:2: expected `:` followed by a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

Ideally, comments would be allowed on `ruff: noqa`. But even if not, Ruff should not be warning about a valid `flake8` directive.

---

_Comment by @charliermarsh on 2024-03-05 18:23_

I think a format we could allow (which would be consistent with `# noqa`) would be like: `# flake8: noqa # some comment`.

---

_Label `suppression` added by @charliermarsh on 2024-03-05 18:23_

---

_Comment by @tylerlaprade on 2024-03-05 20:31_

I got in the habit of using `--` from ESLint, but any consistent format would be good, and I think it'd be good if Ruff chose a single format, whatever that is, to help the community standardize (and to encourage commenting on why rules are suppressed).

That being said, anything that Flake8 currently allows should at the very least not trigger a Ruff warning, since a large legacy codebase might be discouraged from migrating if they see hundreds of warnings on previously valid code.

---

_Comment by @zanieb on 2024-03-11 17:17_

Related discussion at https://github.com/astral-sh/ruff/pull/8876#issuecomment-1831171627

---

_Referenced in [astral-sh/ruff#10343](../../astral-sh/ruff/issues/10343.md) on 2024-03-11 17:20_

---

_Comment by @zanieb on 2024-03-11 17:21_

Going to track this in https://github.com/astral-sh/ruff/issues/10343 instead since this is not a flake8-specific problem.

Thank you for raising! If you have thoughts on the design please respond over there.

---

_Closed by @zanieb on 2024-03-11 17:21_

---

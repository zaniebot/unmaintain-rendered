---
number: 2041
title: Support for kebab-case names in error output
type: issue
state: closed
author: scop
labels: []
assignees: []
created_at: 2023-01-20T22:10:45Z
updated_at: 2023-01-20T22:54:13Z
url: https://github.com/astral-sh/ruff/issues/2041
synced_at: 2026-01-07T13:12:14-06:00
---

# Support for kebab-case names in error output

---

_Issue opened by @scop on 2023-01-20 22:10_

The ability to specify `noqa` et al using the (now kebab-case) check names is great; they're much human friendlier than the flake8-style error codes.

Would be nice to have the error output contain those names some way, too.

This would however make the error messages longer, and simply replacing the current short check code with the long identifier before the message might push the message too far out of sight. mypy solves this by displaying the or long error identifier at the [end of line in square brackets](https://mypy.readthedocs.io/en/stable/error_codes.html#displaying-error-codes), pylint outputs the code before the message, and the long identifier at end of line in parenthesis. I suppose the pylint way would be at least more backwards compatible in ruff.

---

_Comment by @charliermarsh on 2023-01-20 22:54_

Yeah, we're planning to support kebab-case names in configuration files, `noqa` directives, and error outputs (probably -- details TBD). The plan is for it all to be done in a way that maintains compatibility with the existing rule code system.

I'm just gonna close this in favor of #1773 since that's the issue where we're tracking that migration.


---

_Closed by @charliermarsh on 2023-01-20 22:54_

---
